# DataCleaningRBBCSC
# DataCleaningExample
---
title: "DataCleaning"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Here is an example cleaning a data from a survey that we from a local school district in Bloomington, IN.  
```{r}
setwd("~/Desktop")
rbbcsc = read.csv("RBBCSCStaffSurvey.csv", header = TRUE); head(rbbcsc)
```
When we exported the data from qualtrics, the first rows, were additionally information that qualtrics provides that is not relevant to the study.  Therefore, we deleted the first two rows across all the variables.
```{r}
rbbcsc = rbbcsc[-c(1:2),]; head(rbbcsc)
```
Next we select the variables from the dataset that we are interested in analyzing.  In this case, we are interested in evaluting the differences between the perceptions of current professional development for Social and Emotional learning of staff who are exclusively primary and secondary teachers.  Therefore, we must selet the two appropriate variables for this research question.  

```{r}
rbbcsc2 = rbbcsc[c("Q1_6")]
rbbcsc3 = rbbcsc[c("Q15")]
```
Because the responses to their perceptions of professional development are in a Likert Scale format, we need to transform each of the responses into a numerical value so we can conduct data analysis with them.  We use the apply function.  The apply function is a more compact form of an if statement that allows us to tranform the categorical responses (e.g. Strongly agree) into numerical ones.  In the first example, we changing the value Strongly agree is transformed into a 7.  We then need to transform that back into a data frame after we apply the apply function so that we can change the other categorical responses into numbers.  Although, there is likely a more efficient way for transforming the data, the strategy presented below does work and can be more intutive than a large for loop trying to make all of these changes at once. 
```{r}
rbbcsc2 = apply(rbbcsc2, 1, function(x){ifelse(x == "Strongly agree", 7, x)}); rbbcsc2
rbbcsc2 = as.data.frame(rbbcsc2)
rbbcsc2 = apply(rbbcsc2, 1, function(x){ifelse(x == "Agree", 6, x)}); rbbcsc2
rbbcsc2 = as.data.frame(rbbcsc2)
rbbcsc2 = apply(rbbcsc2, 1, function(x){ifelse(x == "Somewhat agree", 5, x)}); rbbcsc2
rbbcsc2 = as.data.frame(rbbcsc2)
rbbcsc2 = apply(rbbcsc2, 1, function(x){ifelse(x == "Neither agree nor disagree", 4, x)}); rbbcsc2
rbbcsc2 = as.data.frame(rbbcsc2)
rbbcsc2 = apply(rbbcsc2, 1, function(x){ifelse(x == "Somewhat disagree", 3, x)}); rbbcsc2
rbbcsc2 = as.data.frame(rbbcsc2)
rbbcsc2 = apply(rbbcsc2, 1, function(x){ifelse(x == "Disagree", 2, x)}); rbbcsc2
rbbcsc2 = as.data.frame(rbbcsc2)
rbbcsc2 = apply(rbbcsc2, 1, function(x){ifelse(x == "Strongly disagree", 1, x)}); rbbcsc2
rbbcsc2 = as.data.frame(rbbcsc2); rbbcsc2

```
Next we create numerical indexs for the Primary school teacher (1) and Secondary school teachers (2).  Again we use the apply function.  It will be become clear why we created the numerical indexs later in the example.

```{r}
rbbcsc3 = apply(rbbcsc3, 1, function(x){ifelse(x == "Primary school teacher", 1, x)}); rbbcsc3
rbbcsc3 = as.data.frame(rbbcsc3); rbbcsc3
rbbcsc3 = apply(rbbcsc3, 1, function(x){ifelse(x == "Secondary school teacher", 2, x)}); rbbcsc3
rbbcsc3 = as.data.frame(rbbcsc3); rbbcsc3
```
Now that we have data coded for each of variables of interest, we can combine them back into one data frame using the cbind funciton.
```{r}
rbbcsc4 = cbind(rbbcsc2, rbbcsc3); rbbcsc4
```
Finally, we grab data where a respondent is either a primary (1) or secondary teacher exclusively.  Because those staff members are coded as 1 or 2, we can grab data for those two groups using the subsetting logic displayed below.
```{r}
rbbcsc5 = rbbcsc4[rbbcsc4$rbbcsc3 %in% c(1,2), ]
rbbcsc5
```
Finally, we create a csv file that can be used in data analysis.
