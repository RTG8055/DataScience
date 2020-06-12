# Ways Of handling categorical variables

## Hashing

    from sklearn.feature_extraction import FeatureHasher

    fh = FeatureHasher(n_features=6, input_type='string')
    hashed_features = fh.fit_transform(vg_df['Genre'])
    hashed_features = hashed_features.toarray()
    pd.concat([vg_df[['Name', 'Genre']], pd.DataFrame(hashed_features)], 
              axis=1).iloc[1:7]																																																	
## type conversion
    for column in categorical_columns:
        test[column] = test[column].astype('category')
---

    for col in numeric_columns:
        test[col] = test[col].astype('float64')
---

    for col in date_columns:
        test[col] = pd.to_datetime(test[col])
        test[col] = test[col].apply(pd.datetime.toordinal)
---

    def convert_type(df):
        for column in categorical_columns:
            df[column] = df[column].astype('category')

        for col in date_columns: 
            df[col] = pd.to_datetime(df[col]) 
            df[col] = df[col].apply(pd.datetime.toordinal)

---

    train_rows_all = set(train['INCIDENT_ID'])
    train.drop(target,axis=1).shape
    test.shape
    df_total = pd.concat([train.drop([target,'X_12'],axis=1),test.drop('X_12',axis=1)])
    
    convert_type(df_total)
    
    from sklearn.preprocessing import LabelEncoder
    le = LabelEncoder()

    for i in categorical_columns:
        df_total[i]=le.fit_transform(df_total[i])

    le = LabelEncoder()
    for i in boolean_columns:
        df_total[i] = le.fit_transform(df_total[i])

---

## Binary Encoding

    import category_encoders as ce

    ce_bin=ce.BinaryEncoder(cols=categorical_columns)
    df_total_binary = ce_bin.fit_transform(df_total)

---

## Dummy / One-Hot Encoding

    categorical_columns2 = ['X_1', 'X_2', 'X_3', 'X_4', 'X_5', 'X_6', 'X_7', 'X_8', 'X_9', 'X_10', 'X_13',
    'X_14', 'X_15', 'day_of_week']
    df_total_dummy = pd.get_dummies(df_total,columns=categorical_columns2)
    ce_bin2=ce.BinaryEncoder(cols=['X_11'])
    df_total_dummy = ce_bin2.fit_transform(df_total_dummy)
        
    #one hot encoding for categorical variables
    data_new = pd.get_dummies(data=data_new,columns=obj_dtypes)

---

# Get Train dataset

## Get Train Cols
    train_cols_binary = list(df_total_binary.drop(indexes,axis=1).columns)
    train_cols_dummy = list(df_total_dummy.drop(indexes,axis=1).columns)

    train_cols = list(train.columns) 
    train_cols.remove(target) 
    train_cols.remove('X_12')
    train_cols = [col for col in train_cols if col not in indexes]

---

## Get Train Rows

    train_rows_all = set(train['INCIDENT_ID'])
---
## Construct final train

    train_binary = df_total_binary[df_total_binary[indexes[0]].apply(lambda x: True if (x in train_rows_all) else False)]
    train_binary = pd.concat([train_binary,train[target]],axis=1)
    test_binary = df_total_binary[df_total_binary[indexes[0]].apply(lambda x: False if (x in train_rows_all) else True)]

---

# Convert to Date
 - today = datetime.datetime(2017, 10, 20)
 - datetime.strptime(date_string, format)
 - train['cnv_date'] = pd.to_datetime(train.DATE)
 - train['day_of_week'] = train.cnv_date.apply(lambda x: x.dayofweek)
 - train['weekend_or_not'] = train.day_of_week.apply(lambda x: 1 if x==6 or x==0 else 0)
 
 ---
 
 # Resampling
 
## Down Sampling

    from sklearn.utils import resample

    #separating majority and minority classes
    df_majority_binary = train_binary[train_binary[target] == 1]
    df_minority_binary = train_binary[train_binary[target] == 0]

    df_majority_binary_downsampled = resample(df_majority_binary,
                                       replace=False,
                                       n_samples=1068,
                                       random_state=66)

    df_downsampled_binary = pd.concat([df_minority_binary,df_majority_binary_downsampled],axis=0)

    #splitting dependent and independent variables

    df_downsampled_binary_X = df_downsampled_binary[train_cols_binary]
    df_downsampled_binary_Y = df_downsampled_binary[[target]]

    print(df_downsampled_binary_X.shape)
    print(df_downsampled_binary_Y.shape)
---
## Up Sampling

    from sklearn.utils import resample

    #separating majority and minority classes
    df_majority_binary = train_binary[train_binary[target] == 1]
    df_minority_binary = train_binary[train_binary[target] == 0]

    df_minority_binary_upsampled = resample(df_minority_binary,
                                       replace=True,
                                       n_samples=22788,
                                       random_state=66)

    df_upsampled_binary = pd.concat([df_majority_binary,df_minority_binary_upsampled],axis=0)

    #splitting dependent and independent variables

    df_upsampled_binary_X = df_upsampled_binary[train_cols_binary]
    df_upsampled_binary_Y = df_upsampled_binary[[target]]

    print(df_upsampled_binary_X.shape)
    print(df_upsampled_binary_Y.shape)
 

# Biblography
- https://www.kaggle.com/vkrahul/fork-of-hackerearth-novartis-malicious-upsampling/notebook?select=CBC_predict_dummy_up.csv
- https://www.journaldev.com/23365/python-string-to-datetime-strptime
