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

#doc_tokens = dfm(doc_tokens)
#head(doc_tokens)
#summary(doc_tokens)

```
Create the training and testing data sets
```{r}
n = .5*dim(edu_dat_class)[1]
raw_train <- edu_dat_class[1:n,]
raw_test <- edu_dat_class[n:nrow(edu_dat_class),]
dim(raw_train)
dim(raw_test)

dfm_train <- edu_dat_class_dfm[1:n,]
dfm_test <- edu_dat_class_dfm[n:nrow(edu_dat_class_dfm),]
```
Now build the model
```{r}
model_class = textmodel_nb(dfm_train, raw_train$CodesMatt)
summary(model_class)
```
Now get predictions
```{r}
pred_class = predict(model_class, newdata = dfm_test)
tab_class =  table(pred_class, raw_test$CodesMatt)

confusionMatrix(tab_class, mode = "everything")
```
Get all data coded
```{r}
edu_dat_class = edu_dat[1:286,]
```
