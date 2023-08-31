# Exercise Recognition
## Training set
The training set can be found on this repository as a train.csv file. More information is available on Kaggle Physical Exercise Recognition Dataset thought it may be slightly altered.
URL: https://www.kaggle.com/datasets/muhannadtuameh/exercise-recognition.
![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/Body%20landmarks.png)
The data are `x`,`y`,`z` coordinate in the 3d space on various key bodyparts as the plot above shows and the labeles are the movement possitions `jumping_jacks_down`, `jumping_jacks_up`, `pullups_down`, `pullups_up`, `pushups_down`, `pushups_up`, `situp_down`, `situp_up`, `squats_down`, `squats_up`. Also, it should be mentioned that (x,y,z)=(0,0,0) is in the middle of the pelvis and the reference plain (z=0) is different for every pose and does not always represend the ground. 
## visualisation
Most of the points, even if they are important for the indentification of the exersice, they clatter the plot making it imposible to tell possitions appart. Keeping only points 0, 11, 12, 13, 14, 15, 16, 23, 24, 25, 26, 27, 28 as seen on the plot above ensures better visualisation.
![Alt text](https://github.com/Kalatz/exercise_recognition/blob/main/Plots/Data%20points%20plot.png)
