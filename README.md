# Exercise Recognition
## Training set
The training set can be found on this repository as a train.csv file. More information is available on Kaggle Physical Exercise Recognition Dataset thought it may be slightly altered.
URL: https://www.kaggle.com/datasets/muhannadtuameh/exercise-recognition.
![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/pose_tracking_full_body_landmarks.png)
As the plot above shows the available data are `x`,`y`,`z` coordinate in the 3d space on various key bodyparts and the movements are labeled `jumping_jacks_down`, `jumping_jacks_up`, `pullups_down`, `pullups_up`, `pushups_down`, `pushups_up`, `situp_down`, `situp_up`, `squats_down`, `squats_up`. Also, it should be mentioned that (x,y,z)=(0,0,0) is in the middle of the pelvis and the reference plain (z=0) is different for every pose and does not always represends the ground. 
