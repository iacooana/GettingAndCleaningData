####Getting and Cleaning Data Course Project####

===================================================================================================================================================
## Data Dictionary - Human Activity Recognition Using Smartphones 
### **_Average of Mean and Standard Deviation Measurements per Activity - Tidy Data Set_**
===================================================================================================================================================



Note:
=====
This data set is derived from the Human Activity Recognition Using Smartphones Dataset, Version 1.0 prepared by Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto and available here for download: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
Information about the underlying data and the cleanup activities and transformations undertaken are described below.

The underlying data:
=====================
The underlying data sets used to obtain the _Average of Mean and Standard Deviation Measurement per Activity Tidy Data Set_ are descbribed as follows:
- The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. 
- Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. 
- Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. 
- The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 
- The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). 
- The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

- For each record it is provided:
	- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
	- Triaxial Angular velocity from the gyroscope. 
	- A 561-feature vector with time and frequency domain variables. 
	- Its activity label. 
	- An identifier of the subject who carried out the experiment.

- The dataset includes the following files:
	- **'README.txt'**
	- **'features_info.txt'**: Shows information about the variables used on the feature vector.
	- **'features.txt'**: List of all features.
	- **'activity_labels.txt'**: Links the class labels with their activity name.
	- **'train/X_train.txt'**: Training set.
	- **'train/y_train.txt'**: Training labels.
	- **'test/X_test.txt'**: Test set.
	- **'test/y_test.txt'**: Test labels.

**Note**: The files listed above contained valuable information used in the data transformations needed to obtain the _Average of Mean and Standard Deviation Measurement per Activity Tidy Data Set_.

- The following files were also available for the train and test data. While these files were helpful they were not used for obtain the Tidy Data Set.
	- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
	- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 
	- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 
	- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 

Data Transfomations: 
======================
- **run_analysis.r** contains the R script that performs the data transformations needed to obtain the tidy data set. The comments within the code, together with **README.md** provides step by step explanations of the work.
- **Data Input**: 
	- _**'activity_labels.txt'**_
	- _**'train/X_train.txt'**_
	- _**'train/y_train.txt'**_
	- _**'test/X_test.txt'**_
	- _**'test/y_test.txt'**_

- **Data Transformations**:
	- After reading in _activity_labels.txt_, the labels were cleaned up of the number in front of each of the descriptive label: E.g. "1 WALKING" became "WALKING"
	- After reading in the _features.txt_, the variable names were also cleaned up of the number in front of each of the descriptive label: E.g. "1 tBodyAcc-mean()-X" will now be "tBodyAcc-mean()-X"
	- The test and train data sets and their coresponding labels were read in and combined. The output of this transformation was:
		- 1 train data set with a new Activity column containing the coresponding activites IDs 
		- 1 test data set with a new Activity column containing the coresponding activities IDs 
	- The next step was to merge the train and test data sets into one overall data set
	- The resulted data set had one Activity column that contained the Activity IDs rather than the descriptive activities. A lookup was done on the previously read in activity labels to replace the IDs with the descriptive names
	- The resulted data set had auto-generated variable names (V1, V2,V3...). The next transformation was to apply the variable names read in from features.txt
	- At this point the data set contained both the test and train data sets with their coresponding activity labels and descriptive variable names. None of the units or measures or number of variables have been affected up to this point. 
	- The next step was to select only the measurements on mean and standard deviation. Criteria used for selecting these measures was:
		- Any feature that contained the word _mean_ or _std_ in its name regardless of the position together with an evaluation of the meaning of the features as per _features_info.txt_ (in the original data set - this remains unchanged). Based on this criteria it was determined that there are 8 variations on variables related to mean and standard deviations:
			- mean(): Mean value
			- meanFreq(): Weighted average of the frequency components to obtain a mean frequency
			- Additional vectors obtained by averaging the signals in a signal window sample:
				- gravityMean
				- tBodyAccMean
				- tBodyAccJerkMean
				- BodyGyroMean
				- tBodyGyroJerkMean
			- std(): Standard deviation
	- Using the above criteria, the variable names were scanned for any of the 8 variations above; a total of 86 coresponding variables containing information on mean or standard deviation were found.
	- The 86 variables containing mean or standard deviations together with the respective activity were selected as the basis for the new data set, leaving the other measurements out:
		
		**"Activity"			       "BodyAcc-mean()-X"		     "tBodyAcc-mean()-Y"              
		"tBodyAcc-mean()-Z"                    "tBodyAcc-std()-X"                    "tBodyAcc-std()-Y"                    
		"tBodyAcc-std()-Z"                     "tGravityAcc-mean()-X"                 "tGravityAcc-mean()-Y"                
		"tGravityAcc-mean()-Z"                 "tGravityAcc-std()-X"                  "tGravityAcc-std()-Y"                 
		"tGravityAcc-std()-Z"                  "tBodyAccJerk-mean()-X"                "tBodyAccJerk-mean()-Y"               
		"tBodyAccJerk-mean()-Z"                "tBodyAccJerk-std()-X"                 "tBodyAccJerk-std()-Y"                
		"tBodyAccJerk-std()-Z"                 "tBodyGyro-mean()-X"                   "tBodyGyro-mean()-Y"                  
		"tBodyGyro-mean()-Z"                   "tBodyGyro-std()-X"                    "tBodyGyro-std()-Y"                   
		"tBodyGyro-std()-Z"                    "tBodyGyroJerk-mean()-X"               "tBodyGyroJerk-mean()-Y"              
		"tBodyGyroJerk-mean()-Z"               "tBodyGyroJerk-std()-X"                "tBodyGyroJerk-std()-Y"               
		"tBodyGyroJerk-std()-Z"                "tBodyAccMag-mean()"                   "tBodyAccMag-std()"                   
		"tGravityAccMag-mean()"                "tGravityAccMag-std()"                 "tBodyAccJerkMag-mean()"              
		"tBodyAccJerkMag-std()"                "tBodyGyroMag-mean()"                  "tBodyGyroMag-std()"                  
		"tBodyGyroJerkMag-mean()"              "tBodyGyroJerkMag-std()"               "fBodyAcc-mean()-X"                   
		"fBodyAcc-mean()-Y"                    "fBodyAcc-mean()-Z"                    "fBodyAcc-std()-X"                    
		"fBodyAcc-std()-Y"                     "fBodyAcc-std()-Z"                     "fBodyAcc-meanFreq()-X"               
		"fBodyAcc-meanFreq()-Y"                "fBodyAcc-meanFreq()-Z"                "fBodyAccJerk-mean()-X"               
		"fBodyAccJerk-mean()-Y"                "fBodyAccJerk-mean()-Z"                "fBodyAccJerk-std()-X"                
		"fBodyAccJerk-std()-Y"                 "fBodyAccJerk-std()-Z"                 "fBodyAccJerk-meanFreq()-X"           
		"fBodyAccJerk-meanFreq()-Y"            "fBodyAccJerk-meanFreq()-Z"            "fBodyGyro-mean()-X"                  
		"fBodyGyro-mean()-Y"                   "fBodyGyro-mean()-Z"                   "fBodyGyro-std()-X"                   
		"fBodyGyro-std()-Y"                    "fBodyGyro-std()-Z"                    "fBodyGyro-meanFreq()-X"              
		"fBodyGyro-meanFreq()-Y"               "fBodyGyro-meanFreq()-Z"               "fBodyAccMag-mean()"                  
		"fBodyAccMag-std()"                    "fBodyAccMag-meanFreq()"               "fBodyBodyAccJerkMag-mean()"          
		"fBodyBodyAccJerkMag-std()"            "fBodyBodyAccJerkMag-meanFreq()"       "fBodyBodyGyroMag-mean()"             
		"fBodyBodyGyroMag-std()"               "fBodyBodyGyroMag-meanFreq()"          "fBodyBodyGyroJerkMag-mean()"         
		"fBodyBodyGyroJerkMag-std()"           "fBodyBodyGyroJerkMag-meanFreq()"      "angle(tBodyAccMean,gravity)"         
		"angle(tBodyAccJerkMean),gravityMean)" "angle(tBodyGyroMean,gravityMean)"     "angle(tBodyGyroJerkMean,gravityMean)"
		"angle(X,gravityMean)"                 "angle(Y,gravityMean)"                 "angle(Z,gravityMean)"**
	- The mean and standard deviation measurements identified above were estimated based on the folowing feature selection and signals:
		- The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. 
		- Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

		- Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

		- Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

		- These signals were used to estimate variables of the feature vector for each pattern: '-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.
			- tBodyAcc-XYZ
			- tGravityAcc-XYZ
			- tBodyAccJerk-XYZ
			- tBodyGyro-XYZ
			- tBodyGyroJerk-XYZ
			- tBodyAccMag
			- tGravityAccMag
			- tBodyAccJerkMag
			- tBodyGyroMag
			- tBodyGyroJerkMag
			- fBodyAcc-XYZ
			- fBodyAccJerk-XYZ
			- fBodyGyro-XYZ
			- fBodyAccMag
			- fBodyAccJerkMag
			- fBodyGyroMag
			- fBodyGyroJerkMag
	- To make a tidy data set, the data was then grouped based on the Activity name and a mean function was applied to the 86 measurements; this created the final tidy data set with each of the variables forming a column and each observation forming a row
- **Data Output**: 
	- _**NewTidyData.txt**_ dataset



Additional Notes: 
==================
- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.


Underlying Data License:
========================
Use of this dataset in publications must be acknowledged by referencing the following publication [1] 

[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz.

Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

This dataset is distributed AS-IS and no responsibility implied or explicit can be addressed to the authors or their institutions for its use or misuse. Any commercial use is prohibited.

Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012. For more information about the original dataset contact: activityrecognition@smartlab.ws

