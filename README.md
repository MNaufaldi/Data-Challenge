# Data Challenge
 The purpose of this python code is to get data from [This link](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) and make it usable for further purposes and properly label them to make it easier to read. Do keep in mind that this code is not efficient because it didn't use any external libraries to get the data (takes about 40 seconds to finish).
 
## Libraries used
1. os

# 1. Merges the training and the test sets to create one data set.

## Initializing
```
test_folder = 'test/'
test_folder_Inertial = 'test/Inertial Signals/'
train_folder = 'train/'
train_folder_Inertial = 'train/Inertial Signals/'
All_data = {}
test_data = {}
train_data = {}
All_data_mean = {}
All_data_std = {}
```
Create the necessary variables to store the data and defining the paths to open the .txt files

## Defining the function and getting the data
```
def AddData(filepath,All_data):
    for file in os.listdir(filepath):
        if ".txt" in file:
            with open(filepath + file) as data:
                All_data[str(file.replace(".txt",""))] = []

                for numbers in data.read().split():
                    All_data[str(file.replace(".txt",""))].append(float(numbers))

AddData(test_folder,test_data)
AddData(test_folder_Inertial,test_data)
AddData(train_folder,train_data)
AddData(train_folder_Inertial,train_data)
All_data['Test'] = test_data
All_data['Train'] = train_data

```
Function to get the data from the .txt files to dictionaries that are previously created

# 2. Extracts only the measurements on the mean and standard deviation for each measurement.
```
for activity in All_data.keys():
    All_data_mean[activity] = {}
    All_data_std[activity] = {}
    for subject in All_data[activity].keys():
        All_data_mean[activity][subject] = np.mean(All_data[activity][subject])
        All_data_std[activity][subject] = np.std(All_data[activity][subject])
```
Nested for loops to get the mean and standard deviation for each subjects in activities

# 3. Uses descriptive activity names to name the activities in the data set and
# 4. Appropriately labels the data set with descriptive variable names.
```
for activity in list(All_data.keys()):
    for subject in list(All_data[activity].keys()):
        new_key = ""
        if "acc" in subject and "accelerometer" not in subject:
            new_key = subject.replace("acc","accelerometer")
        elif "gyro" in subject and "gyroscope" not in subject:
            new_key = subject.replace("gyro","gyroscope")
        else:
            continue
        All_data[activity][new_key] = All_data[activity].pop(subject)
        All_data_mean[activity][new_key] = All_data_mean[activity].pop(subject)
        All_data_std[activity][new_key] = All_data_std[activity].pop(subject)
```
Replacing the old variable names with the new and desciptive variable names

# 5.From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
```
AverageData = All_data.copy()
for activity in (AverageData.keys()):
    All_average = []
    for subject in (AverageData[activity].keys()):
        Average = np.mean(AverageData[activity][subject])
        AverageData[activity][subject] = Average
        All_average.append(Average)
    AverageData[activity]['All_Average'] = np.mean(All_average)      
```
Copying the all data dictionary (unnecessary) to create a variable for mean of every subjects and activities
