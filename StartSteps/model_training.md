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


## Submisstion file

          def make_submission(prediction, sub_name):
              my_submission = pd.DataFrame({'reservation_id':test_binary[index],'amount_spent_per_room_night_scaled':prediction})
              my_submission.to_csv('{}.csv'.format(sub_name),index=False)
              print('A submission file has been made')
          predictions = NN_model.predict(features_actual_scaled)

make_submission(predictions[:,0],'submission_NL6.csv')
