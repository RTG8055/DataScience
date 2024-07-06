## Exploratory Data Analysis

### common Functions

	-	df.info()
	-	df.describe()
	-	df.column.unique()
	-	df.column.value_counts()

### To be used code

#### Correlation Plots


   	plt.figure(figsize= (14,8))
   	# cmap = cmap=sns.diverging_palette(5, 250, as_cmap=True)
   	cmap = sns.diverging_palette(250, 10, as_cmap=True)
	ax = sns.heatmap(train.corr(),center = 0,annot= True,linewidth=0.5,cmap= cmap)



	corr = train.corr()
	plt.figure(figsize=(14,8))
	mask = np.zeros_like(corr)
	mask[np.triu_indices_from(mask)] = True
	with sns.axes_style("white"):
	    ax = sns.heatmap(corr, mask=mask, vmax=.3, square=True,cmap=cmap,center = 0)


#### Common Functions

	for col in categorical_columns: train[col] = train[col].astype('category')
	for col in boolean_columns: train[col] = train[col].apply(convert_to_boolean).astype(bool);test[col] = test[col].apply(convert_to_boolean).astype(bool)
	for col in numeric_columns:
	    df[col] = df[col].astype('float64')





	df.isnull().sum()





	df.apply(lambda x: len(x.unique()),axis=0)


#### Missing Values


	fig = plt.figure(figsize=(18,6))
	miss_train = pd.DataFrame((train.isnull().sum())*100/train.shape[0]).reset_index()
	miss_test = pd.DataFrame((application_test.isnull().sum())*100/application_test.shape[0]).reset_index()
	miss_train["type"] = "train"
	miss_test["type"]  =  "test"
	missing = pd.concat([miss_train,miss_test],axis=0)
	ax = sns.pointplot("index",0,data=missing,hue="type")
	plt.xticks(rotation =90,fontsize =7)
	plt.title("Percentage of Missing values in application train and test data")
	plt.ylabel("PERCENTAGE")
	plt.xlabel("COLUMNS")
	ax.set_facecolor("k")
	fig.set_facecolor("lightgrey")


	lms[categorical_columns]=lms[categorical_columns].apply(lambda x: x.cat.add_categories('Missing').fillna('Missing'))


	 

#### Visualizations
https://wellsr.com/python/seaborn-barplot-tutorial-for-python/#:~:text=If%20you%20want%20to%20display,have%20to%20do%20work%20around.&text=You%20can%20see%20that%20the,be%20stored%20in%20a%20variable.

	for col in categorical_columns:
	    val = lms[col].value_counts(dropna=False)
	    if(len(val.index)>100):
	        print("Too many Categories in "+col)
	        continue
	    sns.barplot(val.index,val.values,label=True)
	    plt.title(col)
	    plt.show()
	 

	 for col in numeric_columns:
	    lms[col].hist()
	    plt.title(col)
	    plt.show()
	 





	plt.figure(figsize=(15,20))

	plt.subplot(231)
	sns.heatmap(pd.DataFrame(bureau.isnull().sum()/bureau.shape[0]*100),annot=True,
	            cmap=sns.color_palette("cool"),linewidth=1,linecolor="white")
	plt.title("bureau")

	plt.subplot(232)
	sns.heatmap(pd.DataFrame(bureau_balance.isnull().sum()/bureau_balance.shape[0]*100),annot=True,
	            cmap=sns.color_palette("cool"),linewidth=1,linecolor="white")
	plt.title("bureau_balance")

	plt.subplot(233)
	sns.heatmap(pd.DataFrame(train.isnull().sum()/train.shape[0]*100),annot=True,
	            cmap=sns.color_palette("cool"),linewidth=1,linecolor="white")
	plt.title("train")

	plt.subplot(234)
	sns.heatmap(pd.DataFrame(installments_payments.isnull().sum()/installments_payments.shape[0]*100),annot=True,
	            cmap=sns.color_palette("cool"),linewidth=1,linecolor="white")
	plt.title("installments_payments")

	plt.subplot(235)
	sns.heatmap(pd.DataFrame(pos_cash_balance.isnull().sum()/pos_cash_balance.shape[0]*100),annot=True,
	            cmap=sns.color_palette("cool"),linewidth=1,linecolor="white")
	plt.title("pos_cash_balance")

	plt.subplot(236)
	sns.heatmap(pd.DataFrame(previous_application.isnull().sum()/previous_application.shape[0]*100),annot=True,
	            cmap=sns.color_palette("cool"),linewidth=1,linecolor="white")
	plt.title("previous_application")

	plt.subplots_adjust(wspace = 1.6)


#### Alluvial plots
```
def get_sanky_frame(df, categories, do_not_plot=[], color_dict={}):
    newDf = pd.DataFrame()
    for cat_col in categories:
        df[cat_col] = df[cat_col].map(str)
    
    cols = ['count']
    if color_dict != {}:
        for col in do_not_plot:
            df['color'] = df[col].map(lambda x: x if color_dict.get(x) is None else color_dict.get(x))
        cols.append('color')
    
    for i in range(len(categories)-1):
        if categories[i] in do_not_plot or categories[i+1] in do_not_plot:
            continue
        tempDf = df[[categories[i],categories[i+1]]+cols]
        tempDf.columns = ['source','target']+cols
        newDf = pd.concat([newDf,tempDf])
        if do_not_plot == [] and len(categories)>2:
            newDf = newDf.groupby(['source','target']).agg({'count':'sum'}).reset_index()
    newDf['source'] = newDf['source'].map(str)

    label_list = list(np.unique(df[categories].values))
    source = newDf['source'].apply(lambda x: label_list.index(x))
    target = newDf['target'].apply(lambda x: label_list.index(x))
    count = newDf['count']
    if color_dict != {}:
        link_color = newDf['color']
    else:
        link_color = []
    return label_list, source, target,count, link_color
```

```
def generate_parallel_plot(df_temp, categories, do_not_plot=[], link_dict = {}, title=""):
    df_v = df_temp[categories].value_counts().reset_index().rename(columns={0:'count'}).copy(deep=True)
    l,s,t,c,color_link = get_sanky_frame(df_v, categories, do_not_plot, link_dict)
    link_values = {"source": s, "target": t, "value": c,"hovertemplate": "Source: %{source.label} <br /> Target: %{target.label} <br /> Count: %{value}"}
    if len(color_link) != 0:
        link_values["color"]= color_link
    fig = go.Figure(data=[go.Sankey(
        node = {"label": l, "pad":5, "thickness":5, "hovertemplate": "%{label} count: %{value}"},
        link = link_values
        )])
    fig.update_layout(title_text=title,
                  font_size=10, legend_title='Colors')
    display(fig)
```

```
categories = ['Hole Finish Value','prev_Result', 'Result']
plot_categories = ['prev_Result', 'Result']
df_v = df_r_h[categories].value_counts().reset_index().rename(columns={0:'count'}).copy(deep=True)

link_values = list(np.unique(df_v[['Hole Finish Value']].values))
print(link_values)
link_colors = ['#FEF3C7', '#A6E3D7', '#CBB4D5', '#721506', '#EBBAB5','black' ]
link_names = ['blue','green','purple','red','pink','black']
link_dict = {}
for v,c in zip(link_values, link_colors):
    link_dict[v]= c
print(link_dict)

pd.DataFrame([link_dict.keys(), link_names, link_dict.values()])

df_r_h = new_df[(new_df['Round'] == 3) & (new_df['Hole'] == 1) & (new_df['Year'] == 2020)]
generate_parallel_plot(df_r_h, ['Hole Finish Value','prev_Result', 'Result'], ['Hole Finish Value'], link_dict, "Round 3, Hole 1, Year 2020")
```




#### boolean columns graphs category wise


	default = train[train[target]==1][boolean_columns]
	non_default = train[train[target]==0][boolean_columns]

	d_cols = boolean_columns
	d_length = len(d_cols)

	fig = plt.figure(figsize=(16,4))
	for i,j in itertools.zip_longest(d_cols,range(d_length)):
	    plt.subplot(1,4,j+1)
	    default[i].value_counts().plot.pie(autopct = "%1.0f%%",colors = sns.color_palette("prism"),startangle = 90,
	                                        wedgeprops={"linewidth":1,"edgecolor":"white"},shadow =True)
	    circ = plt.Circle((0,0),.7,color="white")
	    plt.gca().add_artist(circ)
	    plt.ylabel("")
	    plt.title(i+"-Defaulter")


	fig = plt.figure(figsize=(16,4))
	for i,j in itertools.zip_longest(d_cols,range(d_length)):
	    plt.subplot(1,4,j+1)
	    non_default[i].value_counts().plot.pie(autopct = "%1.0f%%",colors = sns.color_palette("prism",3),startangle = 90,
	                                           wedgeprops={"linewidth":1,"edgecolor":"white"},shadow =True)
	    circ = plt.Circle((0,0),.7,color="white")
	    plt.gca().add_artist(circ)
	    plt.ylabel("")
	    plt.title(i+"-Repayer")




#### percentage of defaulters in category


	for col in categorical_columns:
		plt.figure(figsize=(16,8))
		plt.subplot(121)
		train[train[target]==0][col].value_counts().plot.pie(fontsize=9,autopct = "%1.0f%%",colors = sns.color_palette("Set1"),
		wedgeprops={"linewidth":2,"edgecolor":"white"},shadow =True)
		circ = plt.Circle((0,0),.7,color="white")
		plt.gca().add_artist(circ)
		plt.title("Distribution of "+col+" type for target==0",color="b")

		plt.subplot(122)
		train[train[target]==1][col].value_counts().plot.pie(fontsize=9,autopct = "%1.0f%%", colors = sns.color_palette("Set1"),
		wedgeprops={"linewidth":2,"edgecolor":"white"},shadow =True)
		circ = plt.Circle((0,0),.7,color="white")
		plt.gca().add_artist(circ)
		plt.title("Distribution of "+col+" type for target==1",color="b")
		plt.ylabel("")
		plt.show()




#### numeric cols distribution


	cols = ['AMT_BALANCE','AMT_CREDIT_LIMIT_ACTUAL', 'AMT_INST_MIN_REGULARITY',
	       'AMT_RECEIVABLE_PRINCIPAL', 'AMT_RECIVABLE', 'AMT_TOTAL_RECEIVABLE']

	length = len(cols)
	cs = ["r","g","b","c","m","y"]

	fig = plt.figure(figsize=(13,14))
	fig.set_facecolor("lightgrey")

	for i,j,k in itertools.zip_longest(cols,range(length),cs):
	    plt.subplot(3,2,j+1)
	    ax = sns.distplot(train[train[i].notnull()][i],color=k)
	    ax.set_facecolor("k")
	    plt.xlabel("")
	    plt.title(i)




#### violin plots


	plt.figure(figsize=(13,6))
	sns.violinplot(y= train["DAYS_DECISION"],
	               x = train["NAME_CONTRACT_STATUS"],palette=["r","g","b","y"])
	plt.axhline(train[train["NAME_CONTRACT_STATUS"] == "Approved"]["DAYS_DECISION"].mean(),
	            color="r",linestyle="dashed",label="accepted_average")
	plt.axhline(train[train["NAME_CONTRACT_STATUS"] == "Refused"]["DAYS_DECISION"].mean(),
	            color="g",linestyle="dashed",label="refused_average")
	plt.axhline(train[train["NAME_CONTRACT_STATUS"] == "Cancelled"]["DAYS_DECISION"].mean(),color="b",
	            linestyle="dashed",label="cancelled_average")
	plt.axhline(train[train["NAME_CONTRACT_STATUS"] == "Unused offer"]["DAYS_DECISION"].mean(),color="y",
	            linestyle="dashed",label="un used_average")
	plt.legend(loc="best")

	plt.title("Contract status relative to decision made about previous application.")
	plt.show()




### box plot


plt.figure(figsize=(13,7))
sns.boxplot(y=bureau_balance["MONTHS_BALANCE"],x=bureau_balance["STATUS"],palette="husl")
plt.title("Months balance for status types")
plt.show()


