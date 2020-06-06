from sklearn.feature_extraction import FeatureHasher

fh = FeatureHasher(n_features=6, input_type='string')
hashed_features = fh.fit_transform(vg_df['Genre'])
hashed_features = hashed_features.toarray()
pd.concat([vg_df[['Name', 'Genre']], pd.DataFrame(hashed_features)], 
          axis=1).iloc[1:7]																																																	

    for column in categorical_columns:
        test[column] = test[column].astype('category')
--

    for col in numeric_columns:
        test[col] = test[col].astype('float64')
--

    for col in date_columns:
        test[col] = pd.to_datetime(test[col])
        test[col] = test[col].apply(pd.datetime.toordinal)
--

    train_cols = list(train.columns)
    train_cols.remove('FORECLOSURE')
    train_cols = [col for col in train_cols if col not in indexes]

--

'''
#one hot encoding for categorical variables
data_new = pd.get_dummies(data=data_new,columns=obj_dtypes)

'''

'''
# Up-sample Minority Class
from sklearn.utils import resample

#separating majority and minority classes
df_majority = train[train["TARGET"] == 0]
df_minority = train[train["TARGET"] == 1]

#upsample minority data
df_minority_upsampled = resample(df_minority,
                                 replace=True,
                                 n_samples =197969,
                                 random_state=123)

df_upsampled = pd.concat([df_majority,df_minority_upsampled],axis=0)

#splitting dependent and independent variables
df_upsampled_X = df_upsampled[[i for i in df_upsampled.columns if i not in ['SK_ID_CURR'] + [ 'TARGET']]]
df_upsampled_Y = df_upsampled[["TARGET"]]
'''

'''
# Down-sample Majority Class
from sklearn.utils import resample

#separating majority and minority classes
df_majority = train_hashed[train_hashed[target] == 0]
df_minority = train_hashed[train_hashed[target] == 1]

df_majority_downsampled = resample(df_majority,
                                   replace=False,
                                   n_samples=17288,
                                   random_state=123)

df_downsampled = pd.concat([df_minority,df_majority_downsampled],axis=0)

#splitting dependent and independent variables

df_downsampled_X = df_downsampled[[i for i in df_downsampled.columns if i not in ['SK_ID_CURR'] + [ 'TARGET']]]
df_downsampled_Y = df_downsampled[[target]]
'''


### Box-Cox Transform
'''
from scipy.stats import boxcox
numerical = df_LC.columns[df_LC.dtypes == 'float64']
for i in numerical:
    if df_LC[i].min() > 0:
        transformed, lamb = boxcox(df_LC.loc[df[i].notnull(), i])
        if np.abs(1 - lamb) > 0.02:
            df_LC.loc[df[i].notnull(), i] = transformed
'''

'''
#Label encoding
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()

for i in obj_dtypes:
    data_new[i] = le.fit_transform(data_new[i])
'''

    from sklearn.model_selection import train_test_split
    # implementing train-test-split
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=66)


### RandomForest Classifier

https://medium.com/@hjhuney/implementing-a-random-forest-classification-model-in-python-583891c99652

    from sklearn import model_selection
    # random forest model creation
    rfc = RandomForestClassifier()
    rfc.fit(X_train,y_train)
    # predictions
    rfc_predict = rfc.predict(X_test)
--

    from sklearn.model_selection import cross_val_score
    from sklearn.metrics import classification_report, confusion_matrix
    rfc_cv_score = cross_val_score(rfc, X, y, cv=10, scoring=’roc_auc’)
    
    print("=== Confusion Matrix ===")
    print(confusion_matrix(y_test, rfc_predict))
    print('\n')
    print("=== Classification Report ===")
    print(classification_report(y_test, rfc_predict))
    print('\n')
    print("=== All AUC Scores ===")
    print(rfc_cv_score)
    print('\n')
    print("=== Mean AUC Score ===")
    print("Mean AUC Score - Random Forest: ", rfc_cv_score.mean())
### XGBoost
'''
def modelfit(alg, dtrain, predictors,useTrainCV=True, cv_folds=5, early_stopping_rounds=50):
    from sklearn import metrics
    if useTrainCV:
        xgb_param = alg.get_xgb_params()
        xgtrain = xgb.DMatrix(dtrain[predictors].values, label=dtrain[target].values)
        cvresult = xgb.cv(xgb_param, xgtrain, num_boost_round=alg.get_params()['n_estimators'], nfold=cv_folds,
            metrics='mlogloss', early_stopping_rounds=early_stopping_rounds)
        alg.set_params(n_estimators=cvresult.shape[0])
    
    #Fit the algorithm on the data
    alg.fit(dtrain[predictors], dtrain[target],eval_metric='auc')
        
    #Predict training set:
    dtrain_predictions = alg.predict(dtrain[predictors])
    dtrain_predprob = alg.predict_proba(dtrain[predictors])[:,1]
        
    #Print model report:
    print "\nModel Report"
    print "Accuracy : %.4g" % metrics.accuracy_score(dtrain[target].values, dtrain_predictions)

    feat_imp = pd.Series(alg.feature_importances_, index=predictors)
    feat_imp.plot(kind='bar', title='Feature Importances')
    plt.ylabel('Feature Importance Score')
    return alg
'''

'''
target = 'price_range'
IDcol = 'id'
predictors = [x for x in train.columns if x not in [target, IDcol]]
xgb1 = XGBClassifier(
 learning_rate =0.1,
 n_estimators=1000,
 max_depth=5,
 min_child_weight=1,
 gamma=0,
 subsample=0.8,
 colsample_bytree=0.8,
 objective= 'multi:softmax',
 num_class = 4,
 nthread=4,
 scale_pos_weight=1,
eval_metric = 'mlogloss',
 seed=27)

clf=modelfit(xgb1, train, predictors)
'''
