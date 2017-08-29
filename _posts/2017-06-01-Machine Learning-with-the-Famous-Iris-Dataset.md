---
layout: post
title: Machine Learning with the Famous Iris Dataset
subtitle: Simple KNN Classification with scikit-learn
author: Caesar F. Yang
featured-image:/images/2016-06-01/header_iris.jpg
tags: [machine learning, data analysis, python]
date-string: JUNE 01, 2017
---

# About the dataset
If you like to improve the code on this blog, you can find the dataset at: 
[https://www.kaggle.com/uciml/iris](https://www.kaggle.com/uciml/iris).


# Question

1. How can we generalize a model of the iris species? 
2. Can we make prediction for an out-of-sample observation?
3. How to evaluate the prediction accuracy of our model?
4. What's the relationship between K (number of neighbors) and testing accuracy?

# Variables in this dataset

* SepalLengthCm
* SepalWidthCm
* PetalLengthCm
* PetalWidthCm

![png](/images/2017-06-01/03_iris.png)

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
from sklearn.neighbors import KNeighborsClassifier
from sklearn.cross_validation import train_test_split
from warnings import filterwarnings
filterwarnings("ignore") # sklearn prints some unimportant warning messages
```


```python
#import the dataset from your working directory and view it
iris = pd.read_csv('iris.csv')
iris.info()
iris.head()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 150 entries, 0 to 149
    Data columns (total 6 columns):
    Id               150 non-null int64
    SepalLengthCm    150 non-null float64
    SepalWidthCm     150 non-null float64
    PetalLengthCm    150 non-null float64
    PetalWidthCm     150 non-null float64
    Species          150 non-null object
    dtypes: float64(4), int64(1), object(1)
    memory usage: 7.1+ KB





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>SepalLengthCm</th>
      <th>SepalWidthCm</th>
      <th>PetalLengthCm</th>
      <th>PetalWidthCm</th>
      <th>Species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
#since 'Id' column is insignificant, we can drop it and view the df again
iris = iris.drop("Id", axis=1) 
iris.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SepalLengthCm</th>
      <th>SepalWidthCm</th>
      <th>PetalLengthCm</th>
      <th>PetalWidthCm</th>
      <th>Species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
#let's do some visualization before we build our model
#the scatter plot and distribution
sns.pairplot(iris, hue="Species")
```




    <seaborn.axisgrid.PairGrid at 0x10db52b70>




![png](/images/2017-06-01/output_3_1.png)



```python
#the heatmap of correlations between features
sns.heatmap(iris.corr(), annot=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x112adb898>




![png](/images/2017-06-01/output_4_1.png)



```python
#store feature matrix in "X", in other words, the first 4 columns
X = iris.iloc[:,0:4]
#store label vector in "y"
y = iris['Species']
```


```python
#STEP 1: split X and y into training and testing sets
from sklearn.cross_validation import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=4)
```


```python
#print the shapes of the new X objects
print(X_train.shape)
print(X_test.shape)

#print the shapes of the new y objects
print(y_train.shape)
print(y_test.shape)
```

    (90, 4)
    (60, 4)
    (90,)
    (60,)



```python
#Setting the number of neighbors = 5
#Train the model on the training set
#Test the model on the testing set
n_neighbors=5
knn = KNeighborsClassifier(n_neighbors)
knn.fit(X, y)
y_pred = knn.predict(X)
```


```python
#Evaluate how well we did
from sklearn import metrics
print("The accuracy of KNN (K={n_neighbors}): {0:.2}".format(knn.score(X_test, y_test),
                                                             n_neighbors=n_neighbors))
```

    The accuracy of KNN (K=5): 0.97



```python
# make a prediction for an out-of-sample observation
knn.predict([6, 5, 2, 2])
```




    array(['Iris-setosa'], dtype=object)




```python
# try K=1 through K=10 and record testing accuracy
k_range = list(range(1, 11))
scores = []
for k in k_range:
    knn = KNeighborsClassifier(n_neighbors=k)
    knn.fit(X_train, y_train)
    y_pred = knn.predict(X_test)
    scores.append(metrics.accuracy_score(y_test, y_pred))
```


```python
# plot the relationship between K and testing accuracy
plt.plot(k_range, scores)
plt.xlabel('Value of K for KNN')
plt.ylabel('Testing Accuracy')
```




    <matplotlib.text.Text at 0x113275828>




![png](/images/2017-06-01/output_12_1.png)


