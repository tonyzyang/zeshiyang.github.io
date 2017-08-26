---
layout: post
title: Machine Learning: Movie Rating Prediction
subtitle: Using Decision Tree method to predict movie rating based on IMDB dataset from kaggle. 
author: Caesar F. Yang
featured-image: 
tags: [Machine Learning, R, Data Analysis]
date-string: JANUARY 03, 2017
---

If you like to improve the code on this blog, you can find the dataset at: 
[https://www.kaggle.com/deepmatrix/imdb-5000-movie-dataset](https://www.kaggle.com/deepmatrix/imdb-5000-movie-dataset).

# Question

1. How can we tell the quality of a movie before we actually paid for it? 
2. Can we predict movie rating based on historical data?

# Variables that I used

* title_year
* budget
* gross
* duration
* facenumber_in_poster
* director_facebook_likes
* imdb_score


```{r}
# Load the packages
library(readr)
library(caret)
library(rpart)
library(rpart.plot)
library(class)
```

```{r}
# Load data
# select Data
movie_metadata <- read_csv("//mac/Home/Desktop/movie_metadata.csv")
movie <- dplyr::select(movie_metadata, title_year, budget, gross, duration, facenumber_in_poster, director_facebook_likes, imdb_score)
movie <- as.data.frame(movie)
```

```{r}
#Create standard for the target Variable
movie$overall[movie$imdb_score  > 8.5] <- "Classic"
movie$overall[movie$imdb_score >= 6.5 &
                movie$imdb_score <= 8.5] <- "general"
movie$overall[movie$imdb_score  < 6.5] <- "Junk"
```

```{r}
#Remove rows with NAs in data.frame
movie <- movie[complete.cases(movie),]
```

```{r}
#Set X and Y Variables
Y_movie <- movie$overall
#title_year, budget, gross, duration, facenumber_in_poster, 
#director_facebook_likes, imdb_score
X_movie <- movie[, 1:6]
```

```{r}
#Split the data
set.seed(1)
movie_split <- createDataPartition(Y_movie, p=0.80, list=FALSE)
training_movie <- X_movie[movie_split,]
test_movie <- X_movie[-movie_split,]

training_movie_labels <- Y_movie[movie_split]
test_movie_labels <- Y_movie[-movie_split]
```

```{r}
#Decision tree
fit = rpart(training_movie_labels ~., 
            data = training_movie, 
            method="class")

summary(fit)
rpart.plot(fit)
```

```{r}
#prediction
test_movie$Prediction <- predict(fit, test_movie, type = "class")
```

```{r}
#Evaluation cross tabs
CrossTable(x = test_movie_labels, y= test_movie$Prediction,
           prop.chisq = FALSE,
           prop.r = FALSE,
           prop.c = FALSE,
           prop.t = FALSE)
```

```{r}
#Compute the accurary of prediction
mean(test_movie_labels == test_movie$Prediction)
table(test_movie_labels, test_movie$Prediction)
prop.table(table(test_movie_labels, test_movie$Prediction),1)
```


