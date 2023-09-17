# Exercise Recognition
## Training set
The training set can be found in this repository as a train.csv file. More information is available on Kaggle Physical Exercise Recognition Dataset, though it may be slightly altered.
URL: https://www.kaggle.com/datasets/muhannadtuameh/exercise-recognition.

![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/Body%20landmarks.png)

The data are `x`,`y`,`z` coordinates in the 3D space on various key body parts as the plot above shows and the labels are the movement positions `jumping_jacks_down`, `jumping_jacks_up`, `pullups_down`, `pullups_up`, `pushups_down`, `pushups_up`, `situp_down`, `situp_up`, `squats_down`, `squats_up`. Also, it should be mentioned that (x,y,z)=(0,0,0) is in the middle of the pelvis and the reference plain (z=0) is different for every pose and does not always represent the ground. 

## Visualisation
Most of the points, even if they are important for the identification of the exercise, they clatter the plot making it impossible to tell positions apart. Keeping only points 0, 11, 12, 13, 14, 15, 16, 23, 24, 25, 26, 27, 28 as seen on the plot above ensures better visualization.

![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/Data%20points%20plot.png)

In this example, index 157 is clearly depicted the sit-up down position.

The plot below further helps us recognize the difference between the nose points of two different movements.

![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/nose%20point%20comparison.png)

Understanding the data is vital for tackling the problem. The plot below is the correlation matrix of all the data. All the different points are compared with each other and depicted in a color coded symmetrical matrix. The three lighter-colored squares (stronger correlation) in the diagonal are the points in the head and shoulders, hands, pelvis and legs.

![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/corrplot.png)

The frequency of examples in each class is displayed below. Unbalanced classes may cause issues when predicting the output.

![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/Classes.png)

 ## Preprocessing

The unbalance dataset make this problem harder. One way to fix this using the Synthetic Minority Oversampling Technique (SMOTE) method from the library imbalanced learn.

![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/test.png)

This produces mostly good examples like the ones below:

![alt-text-1](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/After%20smote.png) ![alt-text-2](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/After%20smote.png)

The StandardScaler was use on the data. In scikit-learn is a preprocessing technique used to standardize a dataset, which means scaling features to have a mean of 0 and a standard deviation of 1.


Mathematically, it is represented as:


$$X_{\text{std}} = \frac{X - \mu}{\sigma}$$


Where:
- $X_{\text{std}}$ : Standardized dataset
- $X$ : Original dataset
- $\mu$ : Mean of the original dataset
- $\sigma$ : Standard deviation of the original dataset

## Feature Selection

Multiple way of feature selection were tested on this dataset and are leasted below:
- Select K best (33 features)
- LassoCV (91 features)
- LassoCV (33 features fixed)
- Logistic Regresion estimator (33 features)
- My Selected (39 features based on logic)

For more specific information about the features selected please head to the code.

## Classification

### The functions use for easier classification:
- Monte Carlo Classification(`def monte_carlo_classification_report(X, y, clf,
n_simulations)`) and prints a report. This split the data randomly with the sklearn libraty (`train_test_split()`) and then trains and tests the model. Finally, it prints a matrix with the precision, recall, f1 scores and accurasy with their standard deviation and mean values. This was made posible using a helper function (`def print_classification_results(X, y, clf, report, n_simulations, class_names)`)  and `precision_recall_fscore_support` and `accuracy _score` from `sklearn.metrics`
- Monte Carlo Classification Report with Stratified Shuffle Split (`def monte_carlo_stratified_shuffle_split(X, y, clf, n_splits)`). Now the data are split using Stratified Shuffle Split (`StratifiedShuffleSplit(n_splits=n_splits, test_size=0.2)`) from the `sklearn.model_selection` model selection library.

### Models and results: 
- `LogisticRegression(penalty='l2', solver='lbfgs',
max_iter=10000,multi_class="multinomial" {,
class_weight=’balanced’})`

with **0.838** accuracy and **0.835** with the shuffle split, achived on the SMOTE dataset

- `LogisticRegression(penalty='l1', solver='saga',
max_iter=10000, multi_class="multinomial"{,
class_weight='balanced'})`

with **0.825** accuracy and **0.829** with the shuffle split, achived on the SMOTE dataset

- `LogisticRegression(C = 1000.0, penalty = 'l2', solver =
'lbfgs', max_iter=10000, {class_weight = 'balanced'})` in this model the choise of the hyperparameters was made using Grid Search (`GridSearchCV(LogisticRegression(max_iter=10000), grid,
n_jobs=-1, cv=3)` with `grid = {"C": np.logspace(-7,3,4), "solver":
['lbfgs','saga','liblinear'], 'penalty': ['l1', 'l2',
'elasticnet', 'none']}`)

achived on the SMOTE dataset

- `LogisticRegression(C=1000, multi_class='multinomial',
solver='saga', penalty='l1', max_iter=10000)`
with the 33 features dataset, selected by `SelectFromModel(estimator=LogisticRegression())`, to try grid search for the polynomial hyperparameter `GridSearchCV(pipeline, param_grid, n_jobs=-1, cv=3,
verbose=1)` with `param_grid = {'polynomialfeatures__degree': [1, 2, 3]}` and
`make_pipeline(PolynomialFeatures(),LogisticRegression(C=1000,
multi_class='multinomial', solver='saga', penalty='l1',
max_iter=10000))`

accuracy was not great with a mean of **0.782**

- `LogisticRegressionCV(cv=5, max_iter=10000, random_state=0{,
class_weight='balanced')}`

with **0.864** accuracy, achived on the SMOTE dataset

- `LogisticRegression(penalty='l2', solver='lbfgs', max_iter=10000,multi_class="ovr"{, class_weight='balanced'})`

with **0.821** accuracy, achived on the SMOTE dataset

- `OneVsOneClassifier(LinearSVC(max_iter=15000,
{class_weight='balanced')})`

with **0.859** accuracy, achived on the SMOTE dataset

- `OneVsOneClassifier(LogReg_l2{LogReg_bal})`

with **0.854** accuracy, achived on the SMOTE dataset

- `LinearDiscriminantAnalysis({priors=uniform_priors})`

with **0.812** accuracy, achived on the SMOTE dataset

- `LinearDiscriminantAnalysis(solver='svd', n_components=None,
shrinkage=None, {priors=uniform_priors})` hyperparameter choise made with `GridSearchCV(lda, param_grid=param_grid, cv=5, n_jobs=-1)` and `param_grid = {'solver': ['svd', 'lsqr', 'eigen'],'shrinkage':
[None, 'auto', 0, 0.5, 1],'n_components': [None, 1, 2]}`

with **0.809** accuracy, achived on the SMOTE dataset

- `QuadraticDiscriminantAnalysis({priors=uniform_priors})`

with **0.789** accuracy, achived on 33 LassoCV selected features

- `SVC({class_weight='balanced'})`

with **0.814** accuracy, achived on the SMOTE dataset

- `SVC(kernel = 'linear'{, class_weight='balanced'})`

with **0.858** accuracy, achived on the SMOTE dataset

-`SVC(C = 1000, class_weight = None, gamma = 0.01, kernel =
'rbf')` hyperparameter choise made with `GridSearchCV(estimator=svm, param_grid=param_grid, cv=5,
n_jobs=-1)` with `param_grid = {'C': np.logspace(-5, 5, 11),'kernel':['linear',
'rbf', 'poly', 'sigmoid'],degree': [2, 3, 4],'gamma':
np.logspace(-5, 5, 11),'class_weight': [None, 'balanced']}`

with **0.922** accuracy and **0.0.930** with the shuffle split, achived on the SMOTE dataset

- `DecisionTreeClassifier(criterion='gini'{,
class_weight='balanced'})`

with **0.792** accuracy, achived on the SMOTE dataset

- `DecisionTreeClassifier(class_weight=None,
criterion='entropy', max_depth=None, min_samples_leaf=2,
min_samples_split=5)` hyperparameter choise made with `GridSearchCV(tree, param_grid, cv=5, scoring='accuracy')` and `param_grid = {'criterion': ['gini', 'entropy'],'max_depth':
[None, 5, 10],'min_samples_split': [2, 5, 10],
'min_samples_leaf': [1, 2, 4], 'class_weight': ['balanced',
None]}`

with **0.793** accuracy, achived on the SMOTE dataset

- `DecisionTreeClassifier(class_weight=None,
criterion='entropy', max_depth=10, min_samples_leaf=1,
min_samples_split=2)`

with **0.793** accuracy, achived on the SMOTE dataset

### Tree plot:
![image](https://github.com/Kalatz/exercise_recognition/assets/113215517/a752625e-bc06-47e4-833d-ac7ec3da59d1)

## Conclusions
The support vector machine model with rbf(`SVC(C = 1000, class_weight = None, gamma = 0.01, kernel = 'rbf')`) in which the hyper-parameters were selected with GridSearchCV. Trained with Train-test split  with the Stratified Shuffle Split method. More specifically, the artificial data set features from the SMOTE technique had the highest accuracy with the above model with **93%**.

On the other hand, the quadratic discriminant analysis (QDA) model had the worst performance,
especially on the datasets with all features, with an accuracy of 25%. This can
be due to the fact that the number of parameters to be estimated increases exponentially with
the number of features, which can lead to over-fitting and thus reduced accuracy.
