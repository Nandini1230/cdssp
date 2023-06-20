# cdssp
1.
install.packages("tidyverse")
library(tidyverse)
df <- iris
print("Head")
print("--------------------")
print(head(df))
print('-----------------------')
print("Tail")
print(tail(df))
print(str(df)) # structure of data
print(summary(df)) # summary of data
print("----------------------------------------------------")
print(quantile(df$mpg)) # quantile of data
df <- iris
# Subset of data
print(summary(df))
print(df[1:3])
print(subset(df,Species == "versicolor"))
print(subset(df,Sepal.Length == 6.2))
print(aggregate(df$Sepal.Width,list(df$Sepal.Width),FUN = mean))
2.
#data("iris")
#str(iris)
df <- iris
df$Sepal.Length[c(15, 20, 50, 67, 97, 118)] <- NA
df$Sepal.Width[c(4, 80, 97, 106)] <- NA
df$Petal.Length[c(5, 17, 35, 49)] <- NA
summary(df)
length(which(is.na(df)))
iris_NA <- df[!complete.cases(df), ]
iris_NA
iris_NA <- df[rowSums(is.na(df)) > 0, ]
iris_NA
complete.cases(df)
iris_clean <- df[complete.cases(df), ]
length(which(is.na(iris_clean)))
df[is.na(df$Sepal.Length) & (df$Species == "setosa"),"Sepal.Length"] <-median(df$Sepal.Length[which(df$Species == "setosa")], na.rm = TRUE)
iris_NA <- df[!complete.cases(df), ]
iris_NA
# Now we have removed 3 NA's. Only 11 left
df[is.na(df$Sepal.Length) & (df$Species == "versicolor"),"Sepal.Length"] <- median(df$Sepal.Length[which(df$Species == "versicolor")], na.rm = TRUE)
iris_NA <- df[!complete.cases(df), ]
iris_NA
# Now we have removed 2 NA's. Only 9 left
iris_copy[is.na(iris_copy$Sepal.Length) & (iris_copy$Species == "virginica"),"Sepal.Length"] <- median(iris_copy$Sepal.Length[which(iris_copy$Species == "virginica")], na.rm = TRUE)
iris_NA <- iris_copy[!complete.cases(iris_copy), ]
iris_NA
# Now we have removed 1 NA's. Only 8 left
iris_copy[is.na(iris_copy$Sepal.Width) & (iris_copy$Species == "setosa"),"Sepal.Width"] <-median(iris_copy$Sepal.Width[which(iris_copy$Species == "setosa")], na.rm = TRUE)
iris_NA <- iris_copy[!complete.cases(iris_copy), ]
iris_NA
# Now we have removed 1 NA's. Only 7 left
iris_copy[is.na(iris_copy$Petal.Length) & (iris_copy$Species == "setosa"),"Petal.Length"] <-median(iris_copy$Petal.Length[which(iris_copy$Species == "setosa")], na.rm = TRUE)
iris_NA <- iris_copy[!complete.cases(iris_copy), ]
iris_NA
3.
data(iris)
head(iris, 4)
plot(iris)
sepal_length<-iris$Sepal.Length
hist(sepal_length)
hist(sepal_length, main="Histogram of Sepal Length", xlab="Sepal Length", xlim=c(4,8), 
     col="blue", freq=FALSE)
sepal_width<-iris$Sepal.Width
hist(sepal_width, main="Histogram of Sepal Width", xlab="Sepal Width", xlim=c(2,5), 
     col="darkorchid", freq=FALSE)
irisVer <- subset(iris, Species == "versicolor")
irisSet <- subset(iris, Species == "setosa")
irisVir <- subset(iris, Species == "virginica")
par(mfrow=c(1,3),mar=c(6,3,2,1))
boxplot(irisVer[,1:4], main="Versicolor, Rainbow Palette",ylim = c(0,8),las=2, col=rainbow(4))
boxplot(irisSet[,1:4], main="Setosa, Heat color Palette",ylim = c(0,8),las=2, col=heat.colors(4))
boxplot(irisVir[,1:4], main="Virginica, Topo colors Palette",ylim = c(0,8),las=2, col=topo.colors(4))
4.
library(datasets)
#data(iris)
summary(iris)
# Correlation Plot
plot(iris)
# Correlation Matrix
print(cor(iris[, 1:4]))
# Diff Correlations
print(plot(iris$Sepal.Width, iris$Sepal.Length))
5.
library(datasets)
#data("iris")
str(iris)
library(caret)
set.seed(100)
ind <- createDataPartition(iris$Species,p=0.80,list = F)
train <- iris[ind,]
test <- iris[-ind,]
dim(train)
dim(test)
library(psych)
pairs.panels(train[,-5],gap=0,bg=c("red","blue","yellow")[train$Species],
             pch=21)
pc <- prcomp(train[,-5],center = T,scale. = T)
pc
summary(pc)
pairs.panels(pc$x,gap=0,bg=c("red","blue","yellow")[train$Species],pch = 21)
6.
dat <- iris
dat$size <- ifelse(dat$Sepal.Length < median(dat$Sepal.Length), "small", "big")
table(dat$Species, dat$size)
library(ggplot2)
ggplot(dat) +
  aes(x = Species, fill = size) +
  geom_bar() +
  scale_fill_hue() +
  theme_minimal()
test <- chisq.test(table(dat$Species, dat$size))
test
test$statistic # test statistic
test$p.value # p-value
summary(table(dat$Species, dat$size))
library(vcd)
assocstats(table(dat$Species, dat$size))
test$observed
test$expected # test statistic
