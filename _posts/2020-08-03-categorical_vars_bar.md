---
layout: post
title: "Categorical Variables - Barcharts"
date: 2021-03-08 21:20:06 -0700
comments: false
---
# Categorical Variables - Barcharts



```python
sns.set_style('darkgrid')
```

## Faceted Bar Chart
##### seaborn


```python
g = sns.catplot(x="sex", y="count",
                hue="survived", col="pclass",
                data=df, kind="bar",
                height=6, aspect=.7, palette="flare");
```


![png]({{ site.baseurl}}/images/faceted_bar.png)


## Basic Bar Chart


```python
df_copy2  = df['sex'].value_counts().reset_index()
df_copy2.columns = ['gender', 'count']
df = df_copy2
```

##### seaborn


```python
plt.figure(figsize=(8,4))
plt.title('Titanic Gender Distribution')

sns.barplot(x='gender', y='count', data=df, palette='pastel', alpha=0.9)

```







![png]({{ site.baseurl}}/images/seaborn_basic.png)


##### matplotlib


```python
plt.title('Titanic Gender Distribution')
plt.bar(x=df['gender'], height=df['count'], color=['blue', 'red'], alpha=0.4, width=0.4)
plt.xlabel('Gender')
plt.ylabel('Count')
```





![png]({{ site.baseurl}}/images/matplotlib_basic.png)


## Horizontal Bar charts
##### seaborn


```python
# Flip the x and y variables
plt.figure(figsize=(8,4))
plt.title('Titanic Gender Distribution')
sns.barplot(x='count', y='gender', data=df, palette='pastel', alpha=0.5)
```





![png]({{ site.baseurl}}/images/seaborn_horizontal.png)


##### matplotlib


```python
# y and width are the passed params
plt.title('Titanic Gender Distribution')
plt.barh(y=df['gender'], width=df['count'], color=['blue', 'red'], alpha=0.4)
plt.xlabel('Gender')
plt.ylabel('Count')
```




    Text(0, 0.5, 'Count')




![png]({{ site.baseurl}}/images/matplotlib_horizontal.png)


## Reordering the bars

##### seaborn


```python
# notice the order parameter
plt.figure(figsize=(8,4))
plt.title('Titanic Gender Distribution')

sns.barplot(x='gender', y='count', data=df, palette='pastel', alpha=0.9, order=['male', 'female'])

```







![png]({{ site.baseurl}}/images/seaborn_reordering.png)


##### matplotlib

Done by ordering the dataframe and then plotting
