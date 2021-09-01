Health Analytics Case Study: The goal of this project was to debug SQL code written by the HealthCo Analytics Team as well as answer business questions for the GM

![Health_Data_Info](https://user-images.githubusercontent.com/85455439/131554524-e6bca2e3-3a25-4c75-9b08-2ad712a2b368.png)
user_logs table within the health schema is comprised of six total columns


![HEALTH_Data_COUNT](https://user-images.githubusercontent.com/85455439/131554804-29d4810a-7be1-4993-80f7-f8b055314e99.png)
There are approximately 43,000 records within this table

#1 Question: How many DISTINCT id's in the user_logs table?
Original Code:
![DISTINCT_USERS_TAKE_ONE](https://user-images.githubusercontent.com/85455439/131556802-6758f44d-b3ba-4b72-aacf-e21248aadaf8.png)
This code does not run because this query is attempting to elicit a return from a field which does not exist within the user_log table


![UNIQUE_USERS](https://user-images.githubusercontent.com/85455439/131555994-98029cd4-fdeb-4a3c-9d71-b48f4a348c81.png)
#1 Question: The Analytics Team at HealthCO wanted to know how many DISTINCT users they had in the user_logs table
There are approximately 554 DISTINCT users. 


#2: How many total measurements do we have per user on average?
![AVG_MEASURES_PER_USER_PT1](https://user-images.githubusercontent.com/85455439/131559782-20a447d8-c214-497f-85ed-4976eeba07d5.png)
For questions 2-6, we had to construct a temporary table user_measure_count



#2: How many total measurements do we have per user on average?
![AVG_MEASURES_PER_USER_PT2](https://user-images.githubusercontent.com/85455439/131560226-a1b86933-04d7-4953-b159-8dbb803be92b.png)
This piece of code does not work because, they are trying to aggregate by a function which does not exist. They also failed to add a GROUP BY. 


#2: How many total measurements do we have per user on average?
![AVG_MEASURES_PER_USER_PT3](https://user-images.githubusercontent.com/85455439/131561088-7603143a-d970-4937-98b6-80aa8abcb8c6.png)
The above code will return two columns: The first column, has each user_id. The second column has the total number of measurements per that id

#3: What is the median number of measurements per user?
![Median_Values_PT1](https://user-images.githubusercontent.com/85455439/131561931-68f107d1-d13f-4697-80fa-492a26fbac32.png)
This code will not run because it has not been CAST to the appropriate datatype

#3: What is the median number of measurements per user?
![median_values_peruserPT2 ](https://user-images.githubusercontent.com/85455439/131563338-ebd19128-97d8-41e0-ba33-ec4575958e02.png)
The code above reveals that the median values of measurements per user is 2 


#4: How many users have three or more measurements?
![Measurements_Greaterthan_three_PT1](https://user-images.githubusercontent.com/85455439/131564047-cc5eb421-fc5a-42da-9cb9-702c14ea7ca1.png)
This code will not run because you cannot have a HAVING clause without including any fields in your GROUP BY first


#4: How many users have three or more measurements?
![MEASURECOUNT_GREATERTHAN_3_PT2](https://user-images.githubusercontent.com/85455439/131564454-2977a780-40c4-4f07-9d31-62294cbe0111.png)
The code above reveals that there are 209 users which have 3 or more measurements


#5: How many users have 1000 or more measurements?
![1000_ORmore_measurements_PTUNO](https://user-images.githubusercontent.com/85455439/131565347-89759ba2-19ce-4931-a65f-379252a1ed7e.png)
The above code doesn't work becuase it should be return a COUNT rather than a SUM


#5: How many users have 1000 or more measurements?
![1000_ORMORE_Measurements_PTDos](https://user-images.githubusercontent.com/85455439/131565889-cde83745-b3bb-4cb9-9037-aa62e63a21d9.png)
Only five users have 1000 or more measurements

#6: How many people and what percentage of people have logged glucose measurements?
![Glucose_COUNTS_PTUNO](https://user-images.githubusercontent.com/85455439/131567435-44723695-7487-4f6f-9360-9e675e2e612a.png)
This code will not run because there is a syntax error in the SELECT statement and there is no percentage column


![Glucose_COUNT_PTDOS](https://user-images.githubusercontent.com/85455439/131723691-47d8758f-95a9-436c-97d7-e4c3435a0850.png)
Code above shows that there were 325 users who had blood_glucose measurements


#7: How many users have 2 or more measurements?
![TWO_ORMORE_MEASUREMENTS_PTUNO](https://user-images.githubusercontent.com/85455439/131724860-ac1abae0-80d9-49ba-899c-e235c1913a8f.png)
The code above will not run successfully because you cannot put an aggregate function in a WHERE clause


#7: How many users have 2 or more measurements?
![TWOORMEAUREMENTS_PTDOS](https://user-images.githubusercontent.com/85455439/131725433-8c267504-9c26-46e5-9be5-742cd70d26cb.png)
The above code shows that there were 295 users who had 2 or more measurements


#8: How many users have all three unique measures?
![ALL_THREE_UNIQUE_MEASURESPTUNO](https://user-images.githubusercontent.com/85455439/131726167-3a793379-e106-4ea0-bf38-2a97e7cd9695.png)
The above code will not run because the name of the table is mispelled. Also, the COUNT in the SELECT statement isn't specific enough


#8: How many users have all three unique measures?
![THREE_UNIQUE_MEASURES_PTDOS](https://user-images.githubusercontent.com/85455439/131726625-22d204f4-7f0a-4492-b518-21a6c7e2ccbb.png)
The above code shows that there are 50 users who have all three unique measures


#9:  What is the median systolic/diastolic blood pressure values?
![BLOODPRESSURE_MEDIAN_VALUES_PTUNO](https://user-images.githubusercontent.com/85455439/131727491-d2693010-9c68-4645-93be-3f6519622cdb.png)
The above code will not run because the appropriate datatypes have not been CAST to one which fits the query 
Above code will also not run because the WHERE statement has been designed incorrectly


#9: What is the median systolic/diastolic blood pressure values?
![BLOODPRESSURE_MEDIAN_VALUES_PTDOS](https://user-images.githubusercontent.com/85455439/131729457-b6b03b83-5230-4122-bbbc-2d05cd23fba8.png)
The above code shows that the median blood_pressure values for diastolic and systolic pressure are 126 for systolic and 79 for diastolic

