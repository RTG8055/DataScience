---
layout: post
title: "Continuous Variables"
date: 2021-03-09 18:44:06 -0700
comments: false
---

<a id='toc'></a>
## Contents:
- <a href="#Histogram">Histogram</a>
- <a href="#LinePlot">Line Plot</a>
- <a href="#ViolinPlot">Violin Plot</a>
- <a href="#BoxPlot">Box Plot</a>
- <a href="#RidgeLinePlot">Ridge Line Plot</a>
- <a href="#QQPlot">QQ Plot</a>


<a id='Histogram'></a>
# Histogram
<a href="#toc">[Back to top]</a>


```python
plt.figure(figsize=(12,6))
plt.hist(df['SalePrice'])
```




    (array([148., 723., 373., 135.,  51.,  19.,   4.,   3.,   2.,   2.]),
     array([ 34900., 106910., 178920., 250930., 322940., 394950., 466960.,
            538970., 610980., 682990., 755000.]),
     <a list of 10 Patch objects>)




![png]({{site.baseurl}}/images/Histo.png)


<a id='LinePlot'></a>
# Line Plot 
<a href="#toc">[Back to top]</a>


```python
plt.figure(figsize=(15,8))
sns.lineplot(data=df[:100]['SalePrice'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1910852fd30>




![png]({{site.baseurl}}/images/LinePlot.png)


<a id="ViolinPlot"></a>
# Violin Plot
<a href="#toc">[Back to top]</a>


```python
plt.figure(figsize=(12,6))
sns.violinplot(x = df['SaleCondition'], y = df['SalePrice'])
plt.axhline(df[df['SaleCondition'] == 'Normal']['SalePrice'].mean(),\
            color='r',linestyle='dashed',label='normal_avg')
plt.legend()
```




    <matplotlib.legend.Legend at 0x1910b632f40>




![png]({{site.baseurl}}/images/ViolinPlot.png)


<a id="BoxPlot"></a>
# Box Plot
<a href="#toc">[Back to top]</a>

These plots are useful for outlier detection.

 - Horizontal



```python
plt.figure(figsize=(12,6))
sns.boxplot(data=df, y='SaleCondition', x='SalePrice', orient='h')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1910dcf7250>




![png]({{site.baseurl}}/images/BoxP_H.png)


- Vertical


```python
plt.figure(figsize=(12,6))
sns.boxplot(data=df, x='SaleCondition', y='SalePrice', orient='v')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1910dcf7070>




![png]({{site.baseurl}}/images/BoxP_V.png)





<a id="RidgeLinePlot"></a>
# Ridge Line Plot
<a href="#toc">[Back to top]</a>


```python
ridge_plot = sns.FacetGrid(df, row="SaleCondition", hue="SaleCondition", aspect=5, height=1.25)  
ridge_plot.map(sns.kdeplot, 'SalePrice', shade=True)
ridge_plot.map(plt.axhline)
ridge_plot.fig.subplots_adjust(hspace=0.35)
```


![png]({{site.baseurl}}/images/RidgeLinePlot.png)



<a id="QQPlot"></a>
## QQ Plots
<a href="#toc">[Back to top]</a>

Source: https://seaborn-qqplot.readthedocs.io/en/latest/


```python
from seaborn_qqplot import pplot
```


```python
pplot(df.iloc[:250,:], x='YearBuilt', y='SalePrice', kind='qq', height=4, aspect=2)
```




    <seaborn.axisgrid.PairGrid at 0x191132283d0>




![png]({{site.baseurl}}/images/QQPlot.png)


