# Ways Of handling categorical variables

<details>
  <summary>Hashing</summary>
  
  ```from sklearn.feature_extraction import FeatureHasher

    fh = FeatureHasher(n_features=6, input_type='string')
    hashed_features = fh.fit_transform(vg_df['Genre'])
    hashed_features = hashed_features.toarray()
    pd.concat([vg_df[['Name', 'Genre']], pd.DataFrame(hashed_features)], 
              axis=1).iloc[1:7]
  ```
</details>

<details>
  <summary>type conversion</summary>
  
  ```for column in categorical_columns:
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
    df_total = pd.concat([train.drop([target,'X_12'],axis=1),test.drop('X_12',axis=1)])
    
    convert_type(df_total)
    
    from sklearn.preprocessing import LabelEncoder
    le = LabelEncoder()

    for i in categorical_columns:
        df_total[i]=le.fit_transform(df_total[i])

    le = LabelEncoder()
    for i in boolean_columns:
        df_total[i] = le.fit_transform(df_total[i])

  ```
</details>

<details>
  <summary>Binary Encoding</summary>
    
    ```import category_encoders as ce

    ce_bin=ce.BinaryEncoder(cols=categorical_columns)
    df_total_binary = ce_bin.fit_transform(df_total)
    ```
</details>

<details>
  <summary>Dummy / One-Hot Encoding</summary>
    
    ```
    def create_one_hot_columns(df, one_hot_cols, extra_cols):
      from sklearn.preprocessing import OneHotEncoder
      
      enc = OneHotEncoder(sparse_output=False)
      enc.fit(df[one_hot_cols])
      
      df_one_hot = enc.transform(df[one_hot_cols])
      
      df_encoded = pd.concat([df[extra_cols], pd.DataFrame(df_one_hot, columns=enc.get_feature_names_out())],axis=1)
      
      return df_encoded
    ```
</details>

---

# Get Train dataset

<details>
  <summary>Get Train Cols</summary>
    
    ```train_cols_binary = list(df_total_binary.drop(indexes,axis=1).columns)
    train_cols_dummy = list(df_total_dummy.drop(indexes,axis=1).columns)

    train_cols = list(train.columns) 
    train_cols.remove(target) 
    train_cols.remove('X_12')
    train_cols = [col for col in train_cols if col not in indexes]
    ```
</details>


<details>
  <summary>Get Train Rows</summary>
    
    ```
    train_rows_all = set(train['INCIDENT_ID'])
    ```
</details>

<details>
  <summary>contruct Final Train</summary>
    
    ```train_binary = df_total_binary[df_total_binary[indexes[0]].apply(lambda x: True if (x in train_rows_all) else False)]
    train_binary = pd.concat([train_binary,train[target]],axis=1)
    test_binary = df_total_binary[df_total_binary[indexes[0]].apply(lambda x: False if (x in train_rows_all) else True)]
    ```
</details>

---

# Convert to Date
 - today = datetime.datetime(2017, 10, 20)
 - datetime.strptime(date_string, format)
 - train['cnv_date'] = pd.to_datetime(train.DATE)
 - train['day_of_week'] = train.cnv_date.apply(lambda x: x.dayofweek)
 - train['weekend_or_not'] = train.day_of_week.apply(lambda x: 1 if x==6 or x==0 else 0)
 
 https://www.journaldev.com/23365/python-string-to-datetime-strptime
 
 ---
 
 # Resampling

<details>
  <summary>Down Sampling</summary>
    
    ```from sklearn.utils import resample

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
    ```
</details>

<details>
  <summary>Up Sampling</summary>
    
    ```
    def up_sample_df(df, target,majority_cat = 'Discharge'):
      from sklearn.utils import resample
      
      all_categories = df[target].unique()
      #separating majority and minority classes
      df_majority_binary = df[df[target] == majority_cat]
      df_upsampled = df_majority_binary
      for minority_cat in all_categories:
          if minority_cat == majority_cat:
              continue
          
          df_minority_binary = df[df[target] == minority_cat]
  
          df_minority_binary_upsampled = resample(df_minority_binary,
                                             replace=True,
                                             n_samples=df_majority_binary.shape[0],
                                             random_state=66)
  
          df_upsampled = pd.concat([df_upsampled,df_minority_binary_upsampled],axis=0,ignore_index=True)
      
      return df_upsampled
    ```
</details>

# Biblography
- https://www.kaggle.com/vkrahul/fork-of-hackerearth-novartis-malicious-upsampling/notebook?select=CBC_predict_dummy_up.csv
- https://www.journaldev.com/23365/python-string-to-datetime-strptime
