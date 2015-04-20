####Getting and Cleaning Data Course Project####

**run_analysis.r** contains the script used to satisfy  requirements #1 through #5 of the course project.The output is a tidy data set.

In order to be able to run this script you must download and unzip the data for the project from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip so that 
you have the UCI HAR Dataset folder unzipped in your working directory. You should not modify the folder structure of the UCI HAR Dataset folder.


While the code is highly commented, here is a breakdown of what the code does:

1. Clears the environment variables if any exist and loads the r packages used, specifically base and dplyr

2. Reads in and performs initial clean-up on the test and train data sets and on the activity and variable names as follows:
	
	a) Read in activity_labels.txt and remove numbers in front to retain only the descriptive activity names (E.g. "1 WALKING" will now be "WALKING")
    
	b) Read in features.txt and remove numbers in front to retain only the descriptive variable names (E.g. "1 tBodyAcc-mean()-X" will now be "tBodyAcc-mean()-X")
	
	c) Read in X_test.txt (the Test Dataset) & y_test.txt (the coresponding activities' IDs) and combine the two: add a new Activity column to the Test Data set and assign this new column the activity IDs previously read in
		
	d) Read in X_train.txt (the Train Dataset) & y_test.txt (the coresponding activities' IDs) and combine the two: add a new Activity column to the Train Data set and assign this new column the activity IDs previously read in
		
3. Merge the test and train datasets obtained in steps 2.c) and 2.d) to obtain one data set (**requirement #1**). _Next steps in the code are done using the merged data set._
4. Modify Activity column to use descriptive activity names read in at step 2.a) instead of the activity IDs (**requirement #3**)

5. Modify the auto-generated variable names to use descriptive variable names previously read in at step 2.b) (**requirement #4**)

6. Extract only the measurements on the mean and standard deviation (**requirement #2**)
 (The logic used for selecting the measurements is explained in the Codebook.)

7. Using the functionality of the dplyr package, build the NewTidyData tidy dataset by taking the mean of the measurements selected in step 6 for each of the observations (**requirement #5**)

8. Write out the NewTidyData tidy data set to a text file (NewTidyData is a tidy data set because: each of the variables forms a column and each observation forms a row)
	

For information about the initial data sets, the variables, summaries, units and transformations performed on the data to obtain the tidy data set please see **CodeBook.md** in the same repository.
		

