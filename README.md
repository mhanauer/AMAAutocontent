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
library(reshape2)
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
truth_dat = c(rep(0, n/3), rep(1, n/3), rep(2, n/3))
```
Now start to prep the data for Readme
```{r}
source_url("https://gist.github.com/benmarwick/9266072/raw/csv2txts.R")

csv2txt("~/Desktop/QualAuto", labels = 1)
```
Now create the control_file
```{r}
filename = replicate(dim(ama_edu)[1], rnorm(1,0,1))
filename = data.frame(t(filename))
names(filename) = paste0("~/Desktop/QualAuto/",1:ncol(filename), ".txt")
filename = melt(filename)
filename =  data.frame(filename$variable)
colnames(filename) = "filename"
filename = data.frame(filename)
dim(filename)
```
Create the truth file.  This file has the first responses with what the truth is and then the rest are blank which are filled in by ""
```{r}
truth_blank = data.frame(truth = rep("", dim(ama_edu)[1]-n))
truth_dat = data.frame(truth = truth_dat)
truth = rbind(truth_dat, truth_blank)
dim(truth)
dim(ama_edu)
```
Now put together whether the data is the training set or not
```{r}
trainingset = c(rep(1, n), rep(0, dim(ama_edu)[1]-n))
length(trainingset)
```
Now put together the data set
```{r}
control = cbind(filename, truth, trainingset)
write.table(control, "control.txt", row.names = FALSE, sep = ",")
```
Ok let's try it!!!
Need to get rid of the path just the number and txt
Put the files in the demofiles folder not the clinton posts
```{r}
oldwd <- getwd()
setwd(system.file("demofiles", package="ReadMe"))
undergrad.results <- undergrad(sep = ",")
undergrad.preprocess <- preprocess(undergrad.results)
readme.results <- readme(undergrad.preprocess, boot.se = TRUE)
```
