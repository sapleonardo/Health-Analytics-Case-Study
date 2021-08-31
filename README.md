Health Analytics Case Study: The goal of this project is to debug SQL code for the GM of HealthCO and to answer their business questions as well

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

