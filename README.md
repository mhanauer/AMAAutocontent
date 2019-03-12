---
title: "AMA Results"
output:
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Libraring packages
```{r}
library(stringr)
library(prettyR)
library(Zelig)
```
Loading data
```{r}
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/AMAData")
AMAData = read.csv("AMAData.csv", header = TRUE, na.strings = c("NULL"))
```
Try installing package
```{r}
library(quanteda)
library(readtext)
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/AMAData")
edu_dat = read.csv("Education.csv", header = TRUE)
```
Need a coprus which is a document term matrix, then maybe we can use sentence instead of a word as the token instead of the word

Try making it a corpus and see what that means
Need to make all the words characters
Token is the number of words including repeats
Types is the number of unique words
```{r}
head(edu_dat)
edu_dat$id = 1:dim(edu_dat)[1]
edu_dat = as.character(edu_dat$Education)
edu_dat_corp = corpus(edu_dat)
summary(edu_dat_corp)
```
