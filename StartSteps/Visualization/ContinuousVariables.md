#### Visualizations

#### Violin Plots

```markdown
plt.figure(figsize=(12,6))
sns.violinplot(x = df['SaleCondition'], y = df['SalePrice'])
plt.axhline(df[df['SaleCondition'] == 'Normal']['SalePrice'].mean(),\
            color='r',linestyle='dashed',label='normal_avg')
plt.legend()

![Image]('violin.png')
```

### box plot


plt.figure(figsize=(13,7))
sns.boxplot(y=bureau_balance["MONTHS_BALANCE"],x=bureau_balance["STATUS"],palette="husl")
plt.title("Months balance for status types")
plt.show()


