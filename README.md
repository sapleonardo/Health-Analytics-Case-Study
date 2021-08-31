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
