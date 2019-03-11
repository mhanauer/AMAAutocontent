---
title: "Auto Content education"
output:
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Load the data and library the data
```{r}
setwd("~/Desktop")
ama_edu = read.csv("AMAEducation.csv", header = TRUE)
library(devtools)
library(ReadMe)
```
Start cleaning the data
Need to keep all the data, because you are going to put it back later on
```{r}
head(ama_edu)
library(stringr)

ama_edu = str_replace(ama_edu$Education, "/", "")
ama_edu = data.frame(id = length(ama_edu), edu = ama_edu)
head(ama_edu)
ama_edu$id = rep(1:dim(ama_edu)[1])
write.csv(ama_edu, "ama_edu.csv", row.names = FALSE)
```
Get a random sample, but get the same sample (try setting the seed)
```{r}
n  = round(.1*dim(ama_edu)[1],0)
n
set.seed(123)
ama_edu_sample = ama_edu[sample(nrow(ama_edu), n),]
head(ama_edu_sample)

```
Ok now just run assign random
```{r}
n/3
truth = c(rep(0, n/3), rep(1, n/3), rep(2, n/3))
```
Now start to prep the data for Readme
```{r}
source_url("https://gist.github.com/benmarwick/9266072/raw/csv2txts.R")

csv2txt("~/Desktop/QualAuto", labels = 1)
```
Now create the control_file
```{r}
col_name = replicate(dim(ama_edu)[1], rnorm(1,0,1))
col_name = data.frame(t(col_name))
names(col_name) = paste0("~/Desktop/QualAuto/",1:ncol(col_name), ".txt")
library(reshape2)
col_name = melt(col_name)
col_name =  data.frame(col_name$variable)
colnames(col_name) = "filename"
col_name = data.frame(col_name)
dim(col_name)
```
