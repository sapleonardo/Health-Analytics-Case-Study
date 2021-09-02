**Health Analytics Case Study: The goal of this project was to debug SQL code written by the HealthCo Analytics Team as well as answer business questions for the GM**
```sql
SELECT * 
FROM health.user_logs; 
```
**Results**
| id                                       | log\_date                | measure        | measure\_value | systolic | diastolic |
| ---------------------------------------- | ------------------------ | -------------- | -------------- | -------- | --------- |
| fa28f948a740320ad56b81a24744c8b81df119fa | 2020-11-15T00:00:00.000Z | weight         | 46.03959       |          |           |
| 1a7366eef15512d8f38133e7ce9778bce5b4a21e | 2020-10-10T00:00:00.000Z | blood\_glucose | 97             | 0        | 0         |
| bd7eece38fb4ec71b3282d60080d296c4cf6ad5e | 2020-10-18T00:00:00.000Z | blood\_glucose | 120            | 0        | 0         |

**user_logs table within the health schema is comprised of six total columns**

**Follwoing code returns a total count of every record in this table**
```sql
SELECT COUNT(*) AS totalrecord_count 
FROM health.user_logs; 
```

**Results**
| totalrecord\_count |
| ------------------ |
| 43891              |

**The table health.user_logs has a total COUNT of over 43000 records**

#1 Question: How many DISTINCT id's in the user_logs table?

**Original Code:**

```sql
SELECT
  COUNT DISTINCT user_id
FROM health.user_logs;
```

**This code does not run because this query is attempting to elicit a return from a field which does not exist within the user_log table**

#1 Question: How many DISTINCT id's in the user_logs table?

```sql
SELECT COUNT(DISTINCT id) as uniqueid_count
FROM health.user_logs; 
```

**Results**
| uniqueid\_count |
| --------------- |
| 554             |

**There are exactly 554 unique id's in the user_log table**

**For questions 2 through 6, a temporary table was created**
```sql
DROP TABLE IF EXISTS user_measure_count; 
CREATE TEMP TABLE user_measure_count AS 
SELECT id, COUNT(*) AS measure_count, COUNT(DISTINCT measure) AS unique_measures
FROM health.user_logs
GROUP BY id; 
```

```sql
SELECT *
FROM user_measure_count; 
```

**Temporary Table**
| id                                       | measure\_count | unique\_measures |
| ---------------------------------------- | -------------- | ---------------- |
| 004beb6711843b214e80d73df57a3680fdf9700a | 3              | 2                |
| 007fe1259a4283a991e1f2835ddcdedacf78dde9 | 1              | 1                |
| 008dd1dc1728bb0b420188963905d259c5533149 | 1              | 1                |
| 00ae4fa0241952312d518cee728a387bf156f514 | 4              | 1                |
**Copy of temporary table: user_measure_count**

**#2: How many total measurements do we have per user on average?**

```sql
SELECT
  ROUND(MEAN(measure_count))
FROM user_measure_count;
```
**This piece of code does not work because, they are trying to aggregate by a function which does not exist. They also failed to add a GROUP BY. 

**#2: How many total measurements do we have per user on average?**
```sql
SELECT id, AVG(measure_count) AS measurements_peruser 
FROM user_measure_count 
GROUP BY id 
ORDER BY measurements_peruser DESC; 
```
**The above code will return two columns: The first column, has each user_id. The second column has the total number of measurements per that id**

**Results**
| id                                       | measurements\_peruser |
| ---------------------------------------- | --------------------- |
| 054250c692e07a9fa9e62e345231df4b54ff435d | 22325                 |
| 0f7b13f3f0512e6546b8d2c0d56e564a2408536a | 1589                  |
| ee653a96022cc3878e76d196b1667d95beca2db6 | 1235                  |
| abc634a555bbba7d6d6584171fdfa206ebf6c9a0 | 1212                  |


#3: What is the median number of measurements per user?
```sql
SELECT
  PERCENTILE_CONTINUOUS(0.5) WITHIN GROUP (ORDER BY id) AS median_value
FROM user_measure_count;
```
This code will not run because it has not been CAST to the appropriate datatype

#3: What is the median number of measurements per user?
```sql
SELECT ROUND(
CAST(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_count) AS NUMERIC), 2
) AS medianmeasurements_peruser 
FROM user_measure_count; 
```
**Results**
| medianmeasurements\_peruser |
| --------------------------- |
| 2                           |

**The code above reveals that the median values of measurements per user is 2**

#4: How many users have three or more measurements?
```sql
SELECT
  COUNT(*)
FROM user_measure_count
HAVING measure >= 3;
```This code will not run because you cannot have a HAVING clause without including any fields in your GROUP BY first

#4: How many users have three or more measurements?
```sql
SELECT COUNT(*) AS user_count 
FROM user_measure_count 
WHERE measure_count >= 3; 
```
**Results**
| user\_count |
| ----------- |
| 209         |

**The code above reveals that there are exactly 209 users with 3 or more measurements**

#5: How many users have 1000 or more measurements?
```sql
SELECT
  SUM(id)
FROM user_measure_count
WHERE measure_count >= 1000;
```
**The above code will not work because the query should return a COUNT rather than a SUM**

#5: How many users have 1000 or more measurements?
```sql
SELECT COUNT(*) AS users 
  FROM user_measure_count 
  WHERE measure_count >= 1000;
```
**Results**
| users |
| ----- |
| 5     |

**The above code reveals that there are exactly 5 users which have 1000 or more measurements**

#6: How many people have logged glucose measurements
```sql
SELECT
  COUNT DISTINCT id
FROM health.user_logs
WHERE measure is 'blood_sugar';
```
**Above code will not run due to syntax errors in SELECT statement and in the WHERE clause**

#6: How many people have logged glucose measurements
```sql
  SELECT COUNT(DISTINCT id) as glucose_users 
  FROM health.user_logs 
  WHERE measure = 'blood_glucose'; 
```
**Results**
| glucose\_users |
| -------------- |
| 325            |
|                |

**Above code reveals that there are exactly 325 users with blood_glucose measurements**

#7: How many users have 2 or more measurements?
```sql
SELECT
  COUNT(*)
FROM user_measure_count
WHERE COUNT(DISTINCT measures) >= 2;
```
**The code above will not run successfully because you cannot put an aggregate function in a WHERE clause**

#7: How many users have 2 or more measurements?
```sql
SELECT COUNT(*) AS user_count 
FROM user_measure_count 
WHERE measure_count >= 2;
```
**Results**
| user\_count |
| ----------- |
| 295         |

**The above code shows that there were 295 users who had 2 or more measurements**

#8: How many users have all three unique measures?
```sql
SELECT
  COUNT(*)
FROM usr_measure_count
WHERE unique_measures = 3;
```
**The above code will not run because the name of the table is mispelled. Also, the COUNT in the SELECT statement isn't specific enough**

#8: How many users have all three unique measures?
```sql
SELECT COUNT(DISTINCT id) AS unique_usercount
FROM user_measure_count 
WHERE unique_measures = 3; 
```
**Results**
| unique\_usercount |
| ----------------- |
| 50                |


#9:  What is the median systolic/diastolic blood pressure values?
```sql
SELECT
  PERCENTILE_CONT(0.5) WITHIN (ORDER BY systolic) AS median_systolic
  PERCENTILE_CONT(0.5) WITHIN (ORDER BY diastolic) AS median_diastolic
FROM health.user_logs
WHERE measure is blood_pressure;
```
**The above code will not run because the appropriate datatypes have not been CAST to one which fits the query**
**Above code will also not run because the WHERE statement has been designed incorrectly**

#9: What is the median systolic/diastolic blood pressure values?
```sql
SELECT 
  ROUND (
    CAST(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY systolic) AS NUMERIC),
    2
    ) AS median_systolic,
    ROUND (
      CAST(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY diastolic) AS NUMERIC),  
      2
      ) AS median_diastolic 
    FROM health.user_logs
    WHERE measure = 'blood_pressure';
```
**Results**
| median\_systolic | median\_diastolic |
| ---------------- | ----------------- |
| 126              | 79                |

**The above code shows that the median blood_pressure values for diastolic and systolic pressure are 126 for systolic and 79 for diastolic


