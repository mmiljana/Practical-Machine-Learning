Practical Machine Learning Project - Course Project assignment
========================================================




```r
library(caret)
```

```
## Warning: package 'caret' was built under R version 3.0.3
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 3.0.3
```

```r
library(randomForest)
```

```
## Warning: package 'randomForest' was built under R version 3.0.3
```

```
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.
```

```r
trainData <- read.csv("./data/pml-training.csv")
testData <- read.csv("./data/pml-testing.csv")
trainData <- trainData[, colSums(is.na(trainData)) == 0] 
testData <- testData[, colSums(is.na(testData)) == 0] 
trainData <- trainData[, !grepl("X|user|timestamp|window", names(trainData))]
trainReduced <- trainData[, sapply(trainData, is.numeric)]
trainReduced$classe <- trainData$classe
testData <- testData[, !grepl("X|user|timestamp|window", names(testData))]
testReduced <- testData[, sapply(testData, is.numeric)]
set.seed(1) 
cvTrain <- createDataPartition(trainReduced$classe, p=0.70, list=F)
trainData <- trainReduced[cvTrain, ]
testData <- trainReduced[-cvTrain, ]
model <- train(classe ~ ., data=trainData, method="rf", trControl=trainControl(method="cv", 3), ntree=20)
```

```
## Loading required namespace: e1071
```

```r
predict <- predict(model, testData)
accuracy <- postResample(predict, testData$classe)
accuracy
```

```
##  Accuracy     Kappa 
## 0.9925234 0.9905413
```

```r
result <- predict(model, testReduced[, -length(names(testReduced))])
result
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```

