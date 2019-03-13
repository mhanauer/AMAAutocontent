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
library(caret)
library(quanteda)
library(readtext)
```
Loading data
```{r}
setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/AMAData")
AMAData = read.csv("AMAData.csv", header = TRUE, na.strings = c("NULL"))
```
Try installing package
```{r}

setwd("S:/Indiana Research & Evaluation/Matthew Hanauer/AMAData")
edu_dat = read.csv("Education.csv", header = TRUE)
```
Need a coprus which is a document term matrix, then maybe we can use sentence instead of a word as the token instead of the word

Try making it a corpus and see what that means
Need to make all the words characters
Token is the number of words including repeats
Types is the number of unique words

Example: https://blog.paperspace.com/intro-to-datascience/
```{r}
head(edu_dat)
edu_dat$id = 1:dim(edu_dat)[1]

####Only get those that we classified
edu_dat_class = edu_dat[1:286,]
head(edu_dat_class)
edu_dat_class$Education = as.character(edu_dat_class$Education)
edu_dat_class_corp = corpus(edu_dat_class$Education)
summary(edu_dat_class_corp)
docvars(edu_dat_class_corp) = edu_dat_class$CodesMatt
summary(edu_dat_class_corp)
edu_dat_class_dfm = dfm(edu_dat_class_corp, tolower = TRUE)
dim(edu_dat_class_dfm)
#### Now full
edu_dat_full = edu_dat
head(edu_dat_full)
edu_dat_full$Education = as.character(edu_dat_full$Education)
edu_dat_full_corp = corpus(edu_dat_full$Education)
summary(edu_dat_full_corp)
docvars(edu_dat_full_corp) = edu_dat_full$CodesMatt
summary(edu_dat_full_corp)

edu_dat_full_dfm = dfm(edu_dat_full_corp, tolower = TRUE)
dim(edu_dat_full_dfm)


```
Create the training and testing data sets 
```{r}

######Classified
n = 286/2
raw_train_class <- edu_dat_class[1:n,]
raw_test_class <- edu_dat_class[n:nrow(edu_dat_class),]
dim(raw_train_class)
dim(raw_test_class)

dfm_train_class <- edu_dat_class_dfm[1:n,]
dfm_test_class <- edu_dat_class_dfm[n:nrow(edu_dat_class_dfm),]

#######Full
n = 286/2
raw_train_full <- edu_dat_full[1:n,]
raw_test_full <- edu_dat_full[n:nrow(edu_dat_full),]
dim(raw_train_full)
dim(raw_test_full)

dfm_train_full <- edu_dat_full_dfm[1:n,]
dfm_test_full <- edu_dat_full_dfm[n:nrow(edu_dat_full_dfm),]

```
Now build the model
```{r}
model_class = textmodel_nb(dfm_train_class, raw_train_class$CodesMatt)
summary(model_class)

model_full = textmodel_nb(dfm_train_full, raw_train_full$CodesMatt)
summary(model_full)

```
Now get predictions
```{r}
pred_class = predict(model_class, newdata = dfm_test_class)

length(pred_class)
tab_class =  table(pred_class, raw_test_class$CodesMatt)
confusionMatrix(tab_class, mode = "everything")

pred_full = predict(model_full, newdata = dfm_test_full)
length(pred_full)
tab_full =  table(pred_full, raw_test_full$CodesMatt)
confusionMatrix(tab_full, mode = "everything")
head(pred_full)
```










