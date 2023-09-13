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
q

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


