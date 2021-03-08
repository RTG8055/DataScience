# Categorical Variables - Barcharts


```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```


```python
df = pd.read_csv('../Datasets/titanic.csv')
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pclass</th>
      <th>survived</th>
      <th>name</th>
      <th>sex</th>
      <th>age</th>
      <th>sibsp</th>
      <th>parch</th>
      <th>ticket</th>
      <th>fare</th>
      <th>cabin</th>
      <th>embarked</th>
      <th>boat</th>
      <th>body</th>
      <th>home.dest</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>Allen, Miss. Elisabeth Walton</td>
      <td>female</td>
      <td>29</td>
      <td>0</td>
      <td>0</td>
      <td>24160</td>
      <td>211.3375</td>
      <td>B5</td>
      <td>S</td>
      <td>2</td>
      <td>?</td>
      <td>St Louis, MO</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>Allison, Master. Hudson Trevor</td>
      <td>male</td>
      <td>0.9167</td>
      <td>1</td>
      <td>2</td>
      <td>113781</td>
      <td>151.55</td>
      <td>C22 C26</td>
      <td>S</td>
      <td>11</td>
      <td>?</td>
      <td>Montreal, PQ / Chesterville, ON</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>0</td>
      <td>Allison, Miss. Helen Loraine</td>
      <td>female</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>113781</td>
      <td>151.55</td>
      <td>C22 C26</td>
      <td>S</td>
      <td>?</td>
      <td>?</td>
      <td>Montreal, PQ / Chesterville, ON</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>Allison, Mr. Hudson Joshua Creighton</td>
      <td>male</td>
      <td>30</td>
      <td>1</td>
      <td>2</td>
      <td>113781</td>
      <td>151.55</td>
      <td>C22 C26</td>
      <td>S</td>
      <td>?</td>
      <td>135</td>
      <td>Montreal, PQ / Chesterville, ON</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0</td>
      <td>Allison, Mrs. Hudson J C (Bessie Waldo Daniels)</td>
      <td>female</td>
      <td>25</td>
      <td>1</td>
      <td>2</td>
      <td>113781</td>
      <td>151.55</td>
      <td>C22 C26</td>
      <td>S</td>
      <td>?</td>
      <td>?</td>
      <td>Montreal, PQ / Chesterville, ON</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_cop = df.groupby(['survived', 'sex', 'pclass']).count().reset_index()
df_cop = df_cop.loc[:,['survived', 'sex', 'pclass', 'name']]
df_cop.columns = ['survived', 'sex', 'pclass', 'count']
```


```python
df_cop.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>survived</th>
      <th>sex</th>
      <th>pclass</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>female</td>
      <td>1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>female</td>
      <td>2</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>female</td>
      <td>3</td>
      <td>110</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>male</td>
      <td>1</td>
      <td>118</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>male</td>
      <td>2</td>
      <td>146</td>
    </tr>
  </tbody>
</table>
</div>



## Faceted Bar Chart
##### seaborn


```python
g = sns.catplot(x="sex", y="count",
                hue="survived", col="pclass",
                data=df_cop, kind="bar",
                height=6, aspect=.7, palette="flare");
```


![png](output_7_0.png)


## Basic Bar Chart


```python
df_copy2  = df['sex'].value_counts().reset_index()
df_copy2.columns = ['gender', 'count']
```

##### seaborn


```python
sns.barplot(x='gender', y='count', data=df_copy2)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a29526cc0>




![png](output_11_1.png)


##### matplotlib


```python
plt.bar(x=df_copy2.gender, height=df_copy2['count'], color=['blue', 'red'])
```




    <BarContainer object of 2 artists>




![png](output_13_1.png)


## Horizontal Bar charts
##### seaborn


```python
# Flip the x and y variables
sns.barplot(x='count', y='gender', data=df_copy2)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a2599a7f0>




![png](output_15_1.png)


##### matplotlib


```python
# y and width are the passed params
plt.barh(y=df_copy2.gender, width=df_copy2['count'], color=['blue', 'red'])
```




    <BarContainer object of 2 artists>




![png](output_17_1.png)


## Reordering the bars

##### seaborn


```python
# notice the order parameter
sns.barplot(x='gender', y='count', data=df_copy2, order=['female', 'male'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a246b2da0>




![png](output_20_1.png)


##### matplotlib

Done by ordering the dataframe and then plotting
