---
layout: post
title: PUBG Finish Placement Prediction
author: Chenlu Wang
date: 2019-08-02
---




## Abstract
-----

<p style="text-align:justify">
The project is mainly based on the dataset of game Player Unknown’s Battle Ground. PUBG is a player versus player shooter game in which up to 100 players start in each match and they can be on teams which get ranked at the end of the game based on how many other teams are still alive when they are eliminated. In game, players can pick up different munitions, revive downed-but-not-out (knocked) teammates, drive vehicles, swim, run, shoot, and experience all of the consequences -- such as falling too far or running themselves over and eliminating themselves.
</p>

<p style="text-align:justify">
This project uses ideas of machine learning based on logistic regression, random forest and xgboost for fitting and prediction. The goal is to use dataset given through Kaggle to predict the target variable and have ideal error based on mean squared error loss function. The result of this project is provided by xgboost model with 0.02 mse.
</p>

<!-- more -->

## 1.Introduction
-----

<p style="text-align:justify">
PlayerUnknown's  BattleGrounds  (PUBG)  is  an  online  multiplayer  battle  game developed and published by PUBG Corporation, a subsidiary of South Korean video game  company  Bluehole.  The gameis  based  on  previous  mods  that  were  created  by Brendan "PlayerUnknown" Greene for other games, inspired by the 2000 Japanese film Battle Royale, and expanded into a standalone game under Greene's creative direction. In  the  game,  up  to  one  hundred  players parachute  onto  an  island  and  scavenge  for weapons  and  equipment  to  kill  others  while  avoiding  getting  killed  themselves.  The available safe area of the game's map decreases in size over time, directing surviving players into tighter areas to force encounters. The last player or team standing wins the round.
</p>

## 2. Exploratory Data Analysis
-----

### 2.1 Summary
-----

<p style="text-align:justify">
The dataset is made up of 29 variables with size 29*4446966. All variables can be grouped into three different classes. First class consists variables showing the information of matching and type of games, like groupID which shows members in different groups and matchType showing whether player played the game alone or with others. Second class consists variables about performance of the players, like headshotKills which shows how many enemies player eliminated by directly shooting
the head and teamkills which is the number of times this player killed a teammate. The last class are variables describing the rank of players, like winPlacePerc which is the percentile winning placement and rankPoints, which shows Elo-like ranking of players.
</p>

<center>
<img src="/wclnarry.github.io/picture/12.png" alt="drawing" width="500"/>
</center>

<center>
<img src="/wclnarry.github.io/picture/1.png" alt="drawing" width="500"/>
</center>

### 2.2 Visualization
-----

<p style="text-align:justify">
Different matching types indicates different modes of games that players joined in. By plotting the target variable with different types of matching, densities of different matching types seem to be the same, which indicates that matching types does not have significant influence on target variable as they have the same pattern in the plot. The project will mainly sibe based on the variables about gaming behaviors.
</p>

<center>
<img src="/wclnarry.github.io/picture/2.png" alt="drawing" width="500"/>
</center>

<p style="text-align:justify">
Considering the variables about gaming behaviors, the correlation between all 17 variables with target “winPlacePerc” is shown in the picture below. Variables “weapons acquired”, “walk distance” and “boost” are highly correlated to the target variable compared to others.
</p>

<center>
<img src="/wclnarry.github.io/picture/3.png" alt="drawing" width="500"/>
</center>

<p style="text-align:justify">
“Weapons acquired” have the most value below 6 and the range of it is from 0 to 236. Reclassify the variable into level 0,2,3,4,5 and talent and draw the histogram with target variable, the number of weapons acquired show opposite relationship with “winPlacePerc” as more weapons players acquired, higher probability they win the game. The pattern of acquiring more than 6 weapons is the same to that acquiring 5 weapons. Similarly, the pattern of 3 weapons is the same to that of 4 weapons.
</p>

<center>
<img src="/wclnarry.github.io/picture/4.png" alt="drawing" width="500"/>
</center>

<p style="text-align:justify">
“Walk distance” is the variable describing the total distance that players traveled on foot measured in meters. Considering the other two variables, “swim distance” and “ride distance” are also based on the distance. Adding these three into a new variable, “total distance” and it has high correlation with target variable. Compared to the correlation between total distance and target variable, walk distance has higher correlation with target. As the walk distance has higher weight over other two variables and higher correlation with winPlacePerc, the project will mainly focus on the walk distance instead of swim and ride distance. The higher walk distance, the more likely to win the game.
</p>

<center>
<img src="/wclnarry.github.io/picture/5.png" alt="drawing" width="500"/>
</center>

<p style="text-align:justify">
“Boost” shows the number of boost items used. The most of the “boosts” value are less than 3,but the range of it is from 0 to 33. Reclassify the boost data into four classes with 0,1,2,3 and talent. The probability of players who use less than one boost winning the game is probably less than 0.75 and of players who use more than three boosts is more than 0.65.
</p>

<center>
<img src="/wclnarry.github.io/picture/6.png" alt="drawing" width="500"/>
</center>

<p style="text-align:justify">
Considering to see the relationship between variables about killing behaviors and how they affect the target variables, we calculate the head shot rate with headshot kills and total kills. The plot shows there is high correlation between head shot rate and probability of winning the game. The higher headshot rate, the higher probability of winning the game. Another variable about killing behaviors is kill streak which shows max number of enemy that players killed in a short amount of time. By calculating the kill streak rate with number of streak kills and total kills, the pattern of kill streak rate is similar to the head shot rate, indicating the kill streak rate has similar relationship with probability of winning to head shot rate. Lastly, calculating the damage per kill with “damageDealt” and total kills. The pattern of damage per kill and winPlacePerc is similar to a straight line which indicates the high relationship between damage per kill and target variable. The higher damage per kill, the fairer shooting and higher probability of winning the game. Damage per kill tends to have more significant effects on target variables than other data on killing behaviors and this project will focus on this variable when fitting the model.
</p>

<center>
<img src="/wclnarry.github.io/picture/7.png" alt="drawing" width="500"/>
</center>

<center>
<img src="/wclnarry.github.io/picture/8.png" alt="drawing" width="500"/>
</center>

## 3. Modeling prediction and Evaluation
-----

### 3.1 Featuring
-----

<p style="text-align:justify">
Since the data is very integral and we do not need too much preprocessing work such as filling the missing value, and many variables are highly correlated with the target variables, we can obtain a MAE score of 0.06181 using XGBoost model without doing anything to the dataset, which is acceptable. We can even use a simplest linear model and obtain a 0.08915 MAE score with the original data. Therefore, generating effective features would be very important in this competition. Our main efforts in the featuring are as following.
</p>

<p style="text-align:justify">
First, make full use of the exist useful variables, such as walking distance, damage dealt, heal dealt and so on. Those variables are really significant in predicting intuitively and statistically (as we shown before), it makes sense that player who survives in the end is more likely to walk further and kill more other players. To make full use of such variables, we transform them more specifically, such as the head shot rate we mentioned before, it shows how skillful a player is, and it is highly correlated with win percentage as well. Besides, we generated damage dealt per minutes, damage dealt per kill and damage dealt rank for damage dealt variable.
</p>

<p style="text-align:justify">
Second, we divide the data according to different match types. Since there are six kinds of match type, solo, duo, squad, solo-first-person-perspective (solo-fpp), duo-fpp and squad-spp, and players in different kind of match would have different performance. For example, solo players will have less damage/heal dealt, because in solo games players will be eliminated immediately after they get shot, while in duo/squad games they can be saved by teammates. Therefore, we need to generate features and analysis them accordingly.
</p>

<p style="text-align:justify">
Third, there are always some outliers in the data. For example, some players are lagged or off-line after entering the game, they might survive in the game for a while but have 0 score in any of the record such as damage dealt and walking distance. They can still win or have good places if they get a super teammate. Also we detect cheaters in the games. If a player walks faster than vehicle (use walking distance per minute), it is definitely a cheater. We find out those outliers and label them.
</p>

<p style="text-align:justify">
Finally, we normalize the data by ranking the observations in each variables and transform them into percentile, then times 100. Now we have 74 variables in total.
</p>

### 3.2 Modeling
-----

#### 3.2.1 LDA
-----

<p style="text-align:justify">
Linear Discriminant Analysis, or LDA for short, is a classic current learning method. In the second classification problem, it was first called "Fisher discriminant analysis" by Fisher.
</p>

<p style="text-align:justify">
The idea of LDA is elementary. First, give a training sample. Then try to project the sample onto a straight line. Then, we can find that similar samples are as close as possible, while different samples are as far away as possible. Finally, when predicting a new sample, the new sample is also projected onto this line. The category of the sample is then determined based on the position of the projected point. We apply LDA to our dataset. We found that LDA performed best when target set was divided into two categories. Its mae value can go to 0.1.
</p>

-----
#### 3.2.2 Logistic Regression
-----
<p style="text-align:justify">
Logistic Regression is the appropriate regression analysis to conduct when the dependent variable is dichotomous (binary). It is usually used to describe data and to explain the relationship between one dependent binary variable and one or more nominal, ordinal, interval or ratio-level independent variables.
</p>

<p style="text-align:justify">
In our dataset, although our prediction value (win place percentage) is not a binary value, logistic regression can provide us the prediction of probability on test set and we use it to calculate the mean absolute error. Besides, some of the features (especially after normalization), such as the head shot rate we plot before, have similar curves with sigmoid function, which most of the values lie at 0 and 1. Therefore, we can use the logistic regression in this dataset. We derive a 0.05331387 MAE score on validation set (local), and a 0.08915 MAE score on test set (on Kaggle).
</p>

-----
#### 3.2.3 Random Forest
-----

<p style="text-align:justify">
Random Forest is an ensemble learning method mainly for classification and regression, which operate by constructing a multitude of decision trees at training time and producing the class that is the mode of the classification. Random Forest basically correct for decision trees’ over-fitting problem.
</p>

<p style="text-align:justify">
The random forest model provides a 0.05331419 MAE score on validation set. Besides, we can check the ranking of the importance of features, the results are as following. The rank of statistics of walk distance, kill place and items collected play most important roles in our random forest model.
</p>

<center>
<img src="/wclnarry.github.io/picture/9.png" alt="drawing" width="500"/>
</center>

-----
#### 3.2.4 XGBoost
-----

<p style="text-align:justify">
eXtreme Gradient Boosting (XGBoost) is an enhanced version of gradient boosted decision trees (GBDT). Both XGBoost and GBDT are based on decision tree and boosting method. First, we use boosting method to generate several decision tree classifiers (or regression models) and do the prediction, then we calculate the residual errors and use the models and features that minimized the residual errors. After that we use the better features to generate new models, do the iteration work until the residual error converges. When calculating the residual errors, the XGBoost algorithm uses regularization and Taylor’ formula to improve the calculating speed, and uses greedy algorithm to improve the efficiency in generating new decision tree models. Therefore, it maintains the accuracy of GBDT but improves the speed and efficiency, and it dose not require high calculating capacity (easy to fit).
</p>

<p style="text-align:justify">
In our XGBoost model, after several parameters tuning, we set iteration rounds = 120, learning rate = 0.09, depth of tree = 20, subsample proportion = 0.5, loss function of the residual error = linear regression, evaluation method = MAE, and so on. And we obtain our best result with XGBoost model, they are 0.03513282 on validation set, and 0.02827 on test set (Kaggle).
</p>

<center>
<img src="/wclnarry.github.io/picture/10.png" alt="drawing" width="500"/>
</center>


-------
#### 3.2.5 KNN Model
---------

<p style="text-align:justify">
k-Nearest Neighbor, referred to as KNN, is a commonly used supervised learning method. Its working mechanism is straightforward. First, give a test sample. Then based on some distance metric, find the k training samples closest to the training set. Finally, the results are predicted based on the information of the k "neighbors." In the classification task, the "voting method" is generally used. In other words, the category with the most occurrences among the k samples is selected as the prediction result. Given a test sample x, if the sample closest to it is z, the probability of error in the KNN model is the probability that the x and z categories are different, i.e.，
</p>

$$P(error)=1-\sum_{c\in \Gamma}P(c|x)P(c|z)$$

<p style="text-align:justify">
In the regression problem, the average method is generally used. In other words, the average of the values of the k samples is selected as the prediction result. Finally, we can choose to perform weighted averaging or weighted voting based on distance, and the closer the sample is, the higher the weight. After applying the KNN model to our dataset, we noticed that the KNN model performed well and the MAE value could go to 0.07 after adjusting the parameters.
</p>

-----
#### 3.2.6 MLP Model
-----
<p style="text-align:justify">
Multilayer Perceptron (MLP) is also called Artificial Neural Network (ANN). Perception is the basic processing element. It has input and output. Each input is associated with a connection weight, and then the output is the weighted sum of the inputs. In addition to the input and output layers, there can be multiple hidden layers in between. The simplest MLP contains only one hidden layer, that is, three layers.
</p>

<p style="text-align:justify">
An MLP thinks as a directed graph consisting of multiple node layers, each connected to the next layer. In addition to the input nodes, each node is a neuron with a nonlinear activation function. MLP is a standard supervised learning algorithm. For our dataset, we use the MLPRegressor function in sklearn. In the most ideal state, our mae can reach 0.02. But for our program, our mae value is 0.07. The hidden layer we chose is 40, the training method is Adam, and the activation function is logical.
</p>


### 3.3 Ensemble Model
-----
<p style="text-align:justify">
We use linear model, XGBoost and neural network to ensemble our prediction result. However, the prediction of the ensemble models did not beat our XGBoost model. The XGBoost ensemble model and neural network ensemble model both have MAE around 0.036-0.04, which are closed but not as good as the result of single XGBoost model.
</p>

## 4. Discussion
-----

### 4.1 Summary
-----
<p style="text-align:justify">
The overall performance of our models are as following table. Since we use sub sample to train the model on validation set, the performance of XGBoost model on validation set is not as good as the result on test set. Besides, due to the limit of calculating capacity, we cannot use the full dataset to train the MLP model. We will try to use AWS or sparkr to solve the problem and fit the model in the further work.
</p>

<center>
<img src="/wclnarry.github.io/picture/13.png" alt="drawing" width="500"/>
</center>


### 4.2 Furture work
-----
<p style="text-align:justify">
Firstly, based on the complexity of models fitted, the further work we need to do is to stack all the results of models that we used, and train the new model based on stack results. The goal is to improve the accuracy and efficiency of the final model. Secondly, as the accuracy of our MLP model is less than 0.02 which is ideal, we can use the parameters fitted in original MP as initial value to renew the MLP model. Thirdly, we need to put further effort on fitting the parameters in random forest to get peak value.
</p>

<p style="text-align:justify">
Last but not least, more efforts are needed to identify the abnormal data in our data set. For example, the 100 percent head shoot rate may cause by using some of unfair methods such as hacker or auto-lock-head. Establishing outlier detection mechanism will have practical meaning of predicting the probability of winning the game.
</p>


## 5. Code
------

### 5.1 KNN Model
```
import numpy as np
import pandas as pd
df_train = pd.read_csv('/Users/wangchenlu/Downloads/data.csv')
train=df_train.reset_index()
target="winPlacePerc"
train_columns=list(train.columns)
train_columns.remove("matchId")
train_columns.remove("groupId")
train_columns.remove("index")
train_columns.remove("Unnamed: 0")
from numpy import NaN
from pandas import Series, DataFrame
import numpy as np
x_train=pd.DataFrame(x_train)
x_test=pd.DataFrame(x_test)
x_train = x_train.dropna()
x_test=x_test.dropna()

y_train=x_train[target]
y_test=x_test[target]
x_train=x_train.drop([target],axis=1)
x_test=x_test.drop([target],axis=1)
import pandas as pd
from sklearn.preprocessing import LabelEncoder
from sklearn import svm
from sklearn.neighbors import KNeighborsRegressor
LC=LabelEncoder()
KNN=KNeighborsRegressor()
KNN.fit(x_train,y_train)
result_KNN=KNN.predict(x_test)
import matplotlib.pyplot as plt
plt.plot(result_KNN,color='r') plt.show()
a=abs(result_KNN-y_test)
a=np.mean(a)
print a
```

### 5.2 LDA Model
-----

```
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn import preprocessing
from ultimate.mlp import MLP
import gc
df_train = pd.read_csv('/Users/wangchenlu/Downloads/data.csv')
train=df_train.reset_index()
target="winPlacePerc"
train_columns=list(train.columns)
train_columns.remove("matchId")
train_columns.remove("groupId")
train_columns.remove("index")
train_columns.remove("Unnamed: 0")
new_train=train[0:20000]
new_y=new_train[target]
x_train=new_train[train_columns]

new_test=train[20000:30000]
x_test=new_test[train_columns]
#missing data
from numpy import NaN
from pandas import Series, DataFrame
import numpy as np
from sklearn.preprocessing import Imputer
x_train=pd.DataFrame(x_train)
x_test=pd.DataFrame(x_test)
x_train = x_train.dropna()
x_test=x_test.dropna()
y_train=x_train[target]
y_test=x_test[target]
x_train=x_train.drop([target],axis=1)
x_test=x_test.drop([target],axis=1)
y_train=10*y_train
y_test=10*y_test
import numpy as np
import pandas as pd
from sklearn.preprocessing import StandardScaler
from sklearn import model_selection
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
from sklearn.linear_model import LogisticRegression
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
from sklearn.metrics import mean_squared_error
import warnings

import matplotlib.pyplot as plt
import seaborn as sns
import plotly.figure_factory as ff
import plotly.offline as py

import plotly.graph_objs as go
import plotly.tools as tls

from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
from plotly import tools
y_train=y_train.astype(int)
y_test=y_test.astype(int)
#Running LDA Model
LDA = LinearDiscriminantAnalysis()
LDA.fit(x_train, y_train)
#Predicting values for test data
pred=LDA.predict(x_test)
```

### 5.3 MLP Model
-----

```
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn import preprocessing
import gc
df_train = pd.read_csv('/Users/wangchenlu/Downloads/data.csv')
train=df_train.reset_index()
target="winPlacePerc"
train_columns=list(train.columns)
train_columns.remove("matchId")
train_columns.remove("groupId")
train_columns.remove("index")
train_columns.remove("Unnamed: 0")
new_train=train[0:20000]
new_y=new_train[target]
x_train=new_train[train_columns]
new_test=train[20000:30000]
x_test=new_test[train_columns]

#missing data
from numpy import NaN
from pandas import Series, DataFrame import numpy as np
from sklearn.preprocessing import Imputer
x_train=pd.DataFrame(x_train)
x_test=pd.DataFrame(x_test)
x_train = x_train.dropna()
x_test=x_test.dropna()
#
y_train=x_train[target]
y_test=x_test[target]
x_train=x_train.drop([target],axis=1)
x_test=x_test.drop([target],axis=1)
import numpy as np
import pandas as pd
from sklearn import preprocessing
from ultimate.mlp import MLP import gc, sys
gc.enable()
x_train=np.array(x_train,dtype=np.float64)
x_test=np.array(x_test,dtype=np.float64)
y_train=np.array(y_train,dtype=np.float64)
y_test=np.array(y_test,dtype=np.float64)
#y_train = y_train * 2 - 1
from sklearn.neural_network import MLPRegressor

mlp = MLPRegressor(solver='adam',activation='logistic', batch_size=1000,hidden_layer_sizes=(x_train.shape[1], 40,40,40,40,1),alpha=1e-5, random_state=1,max_iter=300,verbose=True,learning_rate_init=0.001,early_stopping =True,tol=0.00001)

mlp.fit(x_train,y_train)
pred = mlp.predict(x_test)
print pred
a=abs(pred-y_test)
a=np.mean(a)
print a
```
