# Churn Scoring 
## Customer Churn Analysis

**What is Customer Churn?**  
Customer churn refers to the scenario when customers cease their engagement with a service or product. The frequency of churn can be analyzed on different timelines - monthly, quarterly, or annually. This phenomenon can drastically impact a company's revenue streams, stymie growth, and escalate the costs related to customer acquisition.

**Importance of Customer Churn Scoring:**  
Customer Churn Scoring uses behavioral patterns to predict which customers are most likely to disengage. Such predictive insights empower businesses to intervene proactively, potentially preventing customers from severing their ties.

**Our Approach:**  
In our analysis, we employed a mix of analytical models and sampling techniques. The primary objective was to discern which combination of these methods yielded the most accurate predictions for our dataset.

**Case Study Insights:**  
The data foundation for our models is an e-commerce dataset comprised of 5,630 entries spread across 20 columns. The dataset encapsulates the following particulars:

## Dataset Overview
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/dd11bba8-93ce-4dd2-b115-51ec7ca0bcc8)

## Data Exploration
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/f5008e2e-de06-499a-a142-4e5a2bf4327e)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/39b43bc7-2f5c-4800-8fef-648fd833369e)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/8a90b472-6b29-4bd3-9189-71a7d982449a)

## Data Transformation
- Drop the missing value out.
- Transform the category variable to numeric variable by using one-hot encoding technique.
- Perform standard scaling 

<code>df = df.dropna()</code>

<code>#one-hot encoding
df_dummmies = pd.get_dummies(df[['PreferredLoginDevice','PreferredPaymentMode','Gender','PreferedOrderCat','MaritalStatus']])
df = df.merge(df_dummmies, left_index=True, right_index=True, how='inner')

X = df.drop(['CustomerID', 'Churn', 'PreferredLoginDevice','PreferredPaymentMode','Gender','PreferedOrderCat','MaritalStatus'], axis=1)
y = df['Churn']
#rescale X
scaler = StandardScaler()

x_scaled_array = scaler.fit_transform(X)
X_scaled = pd.DataFrame(x_scaled_array)
X_scaled.columns = X.columns

#Train-Test dataset
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=0)

</code>


## Model Creation and Evaluation

<code>
class ModelEvaluator:
    def __init__(self, models,X_train, X_test, y_train, y_test):
        self.models = models
        self.X_train = X_train
        self.X_test = X_test
        self.y_train = y_train
        self.y_test = y_test

    #Create Generic function to fit data and display results/predictions
    #clf = classifer
    def fit_evaluate(self):
        results = []

        for model_name, model in self.models.items():
          samplers = {'no_sampler':'none',
            'Under_Sampler': RandomUnderSampler(random_state=0),
            'Over_Sampler': RandomOverSampler(random_state=0),
            'SMOTE_Sampler': SMOTE(random_state=0),
          }


          for sampler_name, sampler in samplers.items():
            if sampler != 'none':
              sampler.fit(self.X_train, self.y_train)
              X_trainS, y_trainS = sampler.fit_resample(self.X_train, self.y_train)
            else:
              X_trainS = self.X_train
              y_trainS = self.y_train

            # fit model to training data
            model.fit(X_trainS, y_trainS)

            # make predictions for train data
            y_pred_train = model.predict(X_trainS)

            # make predictions for test data
            y_pred_test = model.predict(self.X_test)
            # print evaluation
            print(classification_report(self.y_test, y_pred_test))

            # Calculate F1 score
            f1 = f1_score(self.y_test, y_pred_test)
            precision = precision_score(self.y_test, y_pred_test)
            recall = recall_score(self.y_test, y_pred_test)

            results.append({'Model': model_name, 'Sampler': sampler_name, 'Precision': precision, 'Recall': recall, 'F1 Score': f1})
            print(f'Model: {model_name}, Sampler: {sampler_name}')
            print('\nConfusion Matrix: \n')
            s = sns.heatmap(confusion_matrix(self.y_test, y_pred_test), annot=True, fmt='g', cmap='YlGnBu');
            s.set(xlabel='Predicted class', ylabel='True class')
            plt.show()

            fpr_train, tpr_train, _ = roc_curve(y_pred_train,  y_trainS)
            auc_train = roc_auc_score(y_pred_train, y_trainS)
            plt.plot(fpr_train,tpr_train, color='Blue', label='train: auc='+f'{auc_train:.2f}')
            fpr_test, tpr_test, _ = roc_curve(y_pred_test,  self.y_test)
            auc_test = roc_auc_score(y_pred_test, self.y_test)
            plt.plot(fpr_test,tpr_test, color='Red', label='test: auc='+f'{auc_test:.2f}')
            plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
            plt.legend(loc=4)
            plt.show()
        return pd.DataFrame(results)

</code>
<code>
#Specify the models you want to evaluate
models = {
    'Logistic Regression': LogisticRegression(),
    'Random Forest': RandomForestClassifier(),
    'KNeighbors': KNeighborsClassifier(),
    'XGBClassifier': XGBClassifier()
}
evaluator = ModelEvaluator(models,X_train, X_test, y_train, y_test)
results = evaluator.fit_evaluate()
print(results)
      precision    recall  f1-score   support

           0       0.90      0.97      0.93       623
           1       0.77      0.48      0.60       132

    accuracy                           0.88       755
   macro avg       0.83      0.73      0.76       755
weighted avg       0.88      0.88      0.87       755

Model: Logistic Regression, Sampler: no_sampler

Confusion Matrix: 
</code>

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/a9b98e1d-ed9b-4062-b560-e941a90b61b2)
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/462c132e-f4e1-4e5f-8d35-7b9e1e8f2182)

<code>
              precision    recall  f1-score   support

           0       0.95      0.80      0.87       623
           1       0.46      0.80      0.58       132

    accuracy                           0.80       755
   macro avg       0.70      0.80      0.73       755
weighted avg       0.86      0.80      0.82       755

Model: Logistic Regression, Sampler: Under_Sampler

Confusion Matrix
</code>

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/5eb614bc-9b1c-4dca-95bd-82324bcecd40)
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/e899e6b5-44e5-4805-80a3-a1a3bfdc866a)

## Result all models
<code>
                  Model        Sampler  Precision    Recall  F1 Score
0   Logistic Regression     no_sampler   0.771084  0.484848  0.595349
1   Logistic Regression  Under_Sampler   0.460526  0.795455  0.583333
2   Logistic Regression   Over_Sampler   0.451327  0.772727  0.569832
3   Logistic Regression  SMOTE_Sampler   0.461538  0.772727  0.577904
4         Random Forest     no_sampler   0.981982  0.825758  0.897119
5         Random Forest  Under_Sampler   0.621212  0.931818  0.745455
6         Random Forest   Over_Sampler   0.975410  0.901515  0.937008
7         Random Forest  SMOTE_Sampler   0.982456  0.848485  0.910569
8            KNeighbors     no_sampler   0.710145  0.371212  0.487562
9            KNeighbors  Under_Sampler   0.374126  0.810606  0.511962
10           KNeighbors   Over_Sampler   0.606383  0.863636  0.712500
11           KNeighbors  SMOTE_Sampler   0.571429  0.878788  0.692537
12        XGBClassifier     no_sampler   0.954198  0.946970  0.950570
13        XGBClassifier  Under_Sampler   0.707865  0.954545  0.812903
14        XGBClassifier   Over_Sampler   0.927007  0.962121  0.944238
15        XGBClassifier  SMOTE_Sampler   0.944000  0.893939  0.918288

</code>

## Create Model fuction and Selected XGBClassifier Model

<code>
#Create Generic function to fit data and display results/predictions
def fit_evaluate(clf, X_train, X_test, y_train, y_test):
    ##เปลี่ยนเป็น clfs แทนของเดิม def fit_evaluate(clfs, X_train, X_test, y_train, y_test):
    #แล้วเปลี่ยนเป็น for clf in clfs:
    # fit model to training data
    clf.fit(X_train, y_train)


    # make predictions for train data
    y_pred_train = clf.predict(X_train)

    # make predictions for test data
    y_pred_test = clf.predict(X_test)
    # print evaluation
    print(classification_report(y_test, y_pred_test))
    print('\nConfusion Matrix: \n')
    s = sns.heatmap(confusion_matrix(y_test, y_pred_test), annot=True, fmt='g', cmap='YlGnBu');
    s.set(xlabel='Predicted class', ylabel='True class')
    plt.show()

    fpr_train, tpr_train, _ = roc_curve(y_pred_train,  y_train)
    auc_train = roc_auc_score(y_pred_train, y_train)
    plt.plot(fpr_train,tpr_train, color='Blue', label='train: auc='+f'{auc_train:.2f}')

    fpr_test, tpr_test, _ = roc_curve(y_pred_test,  y_test)
    auc_test = roc_auc_score(y_pred_test, y_test)
    plt.plot(fpr_test,tpr_test, color='Red', label='test: auc='+f'{auc_test:.2f}')
    plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
    plt.legend(loc=4)
    plt.show()


modelXGB = xgb.XGBClassifier(objective='binary:logistic', eval_metric="auc")
print('* XGBoost Classifier * \n')
fit_evaluate(modelXGB, X_train, X_test, y_train, y_test)
</code>

<code>

* XGBoost Classifier * 

              precision    recall  f1-score   support

           0       0.99      0.99      0.99       623
           1       0.95      0.95      0.95       132

    accuracy                           0.98       755
   macro avg       0.97      0.97      0.97       755
weighted avg       0.98      0.98      0.98       755


Confusion Matrix: 
</code>
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/43b1b4a3-7eb7-46fe-8009-677c372207da)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/2d8b0558-ebb8-4e04-85e9-0dace14331eb)

## Feature Importance 

<code>
featureImp = []

for feat, importance in zip(X.columns, modelXGB.feature_importances_):
    temp = [feat, importance*100]
    featureImp.append(temp)

fT_df = pd.DataFrame(featureImp, columns = ['Feature', 'Importance'])

sorted_features = fT_df.sort_values('Importance', ascending = False)
sorted_features = sorted_features.reset_index()
print(sorted_features)

    index                                Feature  Importance
0       0                                 Tenure   14.673425
1       7                               Complain    9.029687
2      33                   MaritalStatus_Single    5.421891
3      29          PreferedOrderCat_Mobile Phone    4.626490
4      27    PreferedOrderCat_Laptop & Accessory    4.608345
5      11                      DaySinceLastOrder    4.220677
6       6                        NumberOfAddress    3.954014
7      25               PreferedOrderCat_Fashion    3.588869
8       5                      SatisfactionScore    3.495441
9       1                               CityTier    3.413731
10     21          PreferredPaymentMode_E wallet    3.307106
11      4               NumberOfDeviceRegistered    3.281787
12     18  PreferredPaymentMode_Cash on Delivery    3.183587
13     31                 MaritalStatus_Divorced    2.675803
14      2                        WarehouseToHome    2.642813
15     13          PreferredLoginDevice_Computer    2.610558
16     23                          Gender_Female    2.305419
17     19       PreferredPaymentMode_Credit Card    2.292607
18     20        PreferredPaymentMode_Debit Card    2.265770
19     12                         CashbackAmount    2.240221
20     10                             OrderCount    2.192402
21      9                             CouponUsed    2.063237
22     22               PreferredPaymentMode_UPI    2.037321
23     32                  MaritalStatus_Married    1.984479
24      8            OrderAmountHikeFromlastYear    1.930459
25     17               PreferredPaymentMode_COD    1.888858
26     14      PreferredLoginDevice_Mobile Phone    1.728923
27      3                         HourSpendOnApp    1.355771
28     15             PreferredLoginDevice_Phone    0.980302
29     26               PreferedOrderCat_Grocery    0.000000
30     24                            Gender_Male    0.000000
31     28                PreferedOrderCat_Mobile    0.000000
32     16                PreferredPaymentMode_CC    0.000000
33     30                PreferedOrderCat_Others    0.000000
</code>


    
