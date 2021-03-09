---
layout: post
title: "Categorical Variables - Barcharts"
date: 2021-03-08 21:20:06 -0700
comments: false
---
# Categorical Variables - Barcharts


## Faceted Bar Chart
##### seaborn


```python
g = sns.catplot(x="sex", y="count",
                hue="survived", col="pclass",
                data=df_cop, kind="bar",
                height=6, aspect=.7, palette="flare");
```


![png]({{ site.baseurl}}/images/{{ site.baseurl}}/images/faceted_bar.png)


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




![png]({{ site.baseurl}}/images/seaborn_basic.png)


##### matplotlib


```python
plt.bar(x=df_copy2.gender, height=df_copy2['count'], color=['blue', 'red'])
```




    <BarContainer object of 2 artists>




![png]({{ site.baseurl}}/images/matplotlib_basic.png)


## Horizontal Bar charts
##### seaborn


```python
# Flip the x and y variables
sns.barplot(x='count', y='gender', data=df_copy2)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a2599a7f0>




![png]({{ site.baseurl}}/images/seaborn_horizontal.png)


##### matplotlib


```python
# y and width are the passed params
plt.barh(y=df_copy2.gender, width=df_copy2['count'], color=['blue', 'red'])
```




    <BarContainer object of 2 artists>




![png]({{ site.baseurl}}/images/matplotlib_horizontal.png)


## Reordering the bars

##### seaborn


```python
# notice the order parameter
sns.barplot(x='gender', y='count', data=df_copy2, order=['female', 'male'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1a246b2da0>




![png]({{ site.baseurl}}/images/seaborn_reorder_bar.png)


##### matplotlib

Done by ordering the dataframe and then plotting
