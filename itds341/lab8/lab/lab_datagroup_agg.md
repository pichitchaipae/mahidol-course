
# Lab: Data Grouping and Aggregation

**General Instructions (Read Carefully):**

- For each task, store your answer in the variable name specified in the task (e.g., `result2_1`, `df2_3`, etc.).
- Do **not** change the name of the variables. Your code will be autograded based on these names.
- Unless a task explicitly asks you to modify a DataFrame (e.g., add a column), do **not** mutate or overwrite it. If you need to change a DataFrame for a later task, save the original as a new variable (e.g., `df_task1 = df.copy()`) before making changes.
- Each task should be completed independently, unless otherwise stated. Do **not** rely on the output of previous tasks unless the instructions say so.
- You may print your variable to check your answer with the expected output provided, but it is not required for grading.
- Use only the required columns or rows as specified in each task.
- You may use any valid pandas method unless a specific method is requested.
- Your code should be reproducible and not rely on previous outputs unless stated.

**Tips for Autograder-Friendly Code:**

- Always use the exact variable names specified.
- If you need to mutate a DataFrame (e.g., add a column), save a copy of the original DataFrame in a new variable before making changes, so earlier tasks can still be checked.
- If you are unsure, ask yourself: "If the autograder checks my variable, will it match the expected output exactly?"

---

## Task 1: Load Titanic Dataset

Read the Titanic dataset from the CSV file, named `titanic-data-clean.csv` into a DataFrame named `df`.

---


## Task 2: GroupBy


### Task 2.1: Compute the mean of the `Fare` of passengers from each `Pclass`

Store your answer in `result2_1`.

Expected output:

```
Pclass
1    84.154687
2    20.662183
3    13.675550
Name: Fare, dtype: float64
```


### Task 2.2: Compute the standard deviation of the `Fare` of passengers from each `Pclass` and `Sex`

Store your answer in `result2_2`.

Expected output:

```
Pclass  Sex   
1       female    74.259988
        male      77.548021
2       female    10.891796
        male      14.922235
3       female    11.690314
        male      11.681696
Name: Fare, dtype: float64
```


### Task 2.3: Compute the mean of the `Fare` of passengers from each `Pclass` and `Age_group`

Hint: please refer to the binning technique in the previous lecture to group passengers in the following bins:

```python
bins = [0, 25, 35, 60, 100]
group_names = ["Youth", "YoungAdult", "MiddleAged", "Senior"]
```

Expected output:

```python
print(df['Age_group'])

# 0           Youth
# 1      MiddleAged
# 2      YoungAdult
# 3      YoungAdult
# 4      YoungAdult
#           ...    
# 886    YoungAdult
# 887         Youth
# 888    YoungAdult
# 889    YoungAdult
# 890    YoungAdult
# Name: Age_group, Length: 891, dtype: category
# Categories (4, object): ['Youth' < 'YoungAdult' < 'MiddleAged' < 'Senior']
```

Once you have added the `Age_group` column into the dataframe, `df`, write the code to compute the mean of the `Fare` of passengers from each `Pclass` and `Age_group`.

Store your answer in `result2_3`.

Expected output:

```
                         Fare
Pclass Age_group             
1      Youth       116.987200
       YoungAdult   78.512619
       MiddleAged   76.983334
       Senior       59.969050
2      Youth        25.102309
       YoungAdult   17.535386
       MiddleAged   19.760638
       Senior       10.500000
3      Youth        13.865335
       YoungAdult   13.727935
       MiddleAged   13.334195
       Senior        7.820000
```


### Task 2.4: Create a custom aggregation function

Define a function named `gt_mean` that counts the number of passengers whose `Fare` is greater than the mean of their group. Use this function in a groupby aggregation on `Embarked` and `Sex`.

Hint: you may find this code np.sum(df>df.mean()) useful for counting.

Store your answer in `result2_4`.

Expected output:

```
                 Fare
Embarked Sex         
C        female    30
         male      25
Q        female     8
         male      12
S        female    53
         male     137
UNK      female     0
```


## Task 3: Multiple Aggregation Functions


### Task 3.1: Compute the mean and standard deviation of the `Fare` of passengers from each `Pclass` and `Sex`

Store your answer in `result3_1`.

Expected output:

```
                     Fare           
                     mean        std
Pclass Sex                          
1      female  106.125798  74.259988
       male     67.226127  77.548021
2      female   21.970121  10.891796
       male     19.741782  14.922235
3      female   16.118810  11.690314
       male     12.661633  11.681696
```


### Task 3.2: Use different aggregation functions on different columns

- `Fare`: `min`, `max`, `mean`, `std`
- `Age`: `gt_mean` (from Task 2.4)
- `Parch`: `count`

Store your answer in `result3_2`.

Expected output:

```
                  Fare                                      Age Parch
                   min       max        mean        std gt_mean count
Pclass Sex                                                           
1      female  25.9292  512.3292  106.125798  74.259988      44    94
       male     0.0000  512.3292   67.226127  77.548021      53   122
2      female  10.5000   65.0000   21.970121  10.891796      38    76
       male     0.0000   73.5000   19.741782  14.922235      47   108
3      female   6.7500   69.5500   16.118810  11.690314      81   144
       male     0.0000   69.5500   12.661633  11.681696     202   347
```


## Task 4: Apply on Group


Define a function named `top` that returns the top three passengers (by `Fare`) in each group. Use this function with groupby on `Pclass`.

Hint: You may find `df.sort_values` with `ascending=False` useful.

Store your answer in `result4_1`.

Expected output:

```
            PassengerId  Survived  Pclass                                Name  \
Pclass                                                                          
1      258          259         1       1                    Ward, Miss. Anna   
       737          738         1       1              Lesurer, Mr. Gustave J   
       679          680         1       1  Cardeza, Mr. Thomas Drake Martinez   
2      665          666         0       2                  Hickman, Mr. Lewis   
       72            73         0       2                Hood, Mr. Ambrose Jr   
       385          386         0       2           Davies, Mr. Charles Henry   
3      324          325         0       3            Sage, Mr. George John Jr   
       792          793         0       3             Sage, Miss. Stella Anna   
       180          181         0       3        Sage, Miss. Constance Gladys   

               Sex        Age  SibSp  Parch        Ticket      Fare  \
Pclass                                                                
1      258  female  35.000000      0      0      PC 17755  512.3292   
       737    male  35.000000      0      0      PC 17755  512.3292   
       679    male  36.000000      0      1      PC 17755  512.3292   
2      665    male  32.000000      2      0  S.O.C. 14879   73.5000   
       72     male  21.000000      0      0  S.O.C. 14879   73.5000   
       385    male  18.000000      0      0  S.O.C. 14879   73.5000   
3      324    male  29.699118      8      2      CA. 2343   69.5500   
       792  female  29.699118      8      2      CA. 2343   69.5500   
       180  female  29.699118      8      2      CA. 2343   69.5500   

                  Cabin Embarked   Age_group  
Pclass                                        
1      258          UNK        C  YoungAdult  
       737         B101        C  YoungAdult  
       679  B51 B53 B55        C  MiddleAged  
2      665          UNK        S  YoungAdult  
       72           UNK        S       Youth  
       385          UNK        S       Youth  
3      324          UNK        S  YoungAdult  
       792          UNK        S  YoungAdult  
       180          UNK        S  YoungAdult
```


## Task 5: Pivot Table


### Task 5.1: Create a pivot table to aggregate the `Fare`, `Parch`, and `SibSp` of passengers in each `Pclass` group using mean

Store your answer in `result5_1`.

Expected output:

```
             Fare     Parch     SibSp
Pclass                               
1       84.154687  0.356481  0.416667
2       20.662183  0.380435  0.402174
3       13.675550  0.393075  0.615071
```


### Task 5.2: Extend the code from Task 5.1 to aggregate using `count` and `mean`

Store your answer in `result5_2`.

Hint: you may find the `aggfunc` argument of the `df.pivot_table` useful.

Expected output:

```
       count                   mean                    
        Fare Parch SibSp       Fare     Parch     SibSp
Pclass                                                 
1        216   216   216  84.154687  0.356481  0.416667
2        184   184   184  20.662183  0.380435  0.402174
3        491   491   491  13.675550  0.393075  0.615071
```


### Task 5.3: Extend the code from Task 5.1 to further compute the mean of each sex for `Fare`, `Parch`, `SibSp`

Store your answer in `result5_3`.

Hint: you may find the `columns` argument of the `df.pivot_table` useful.

Expected output:

```
              Fare                Parch               SibSp          
Sex         female       male    female      male    female      male
Pclass                                                               
1       106.125798  67.226127  0.457447  0.278689  0.553191  0.311475
2        21.970121  19.741782  0.605263  0.222222  0.486842  0.342593
3        16.118810  12.661633  0.798611  0.224784  0.895833  0.498559
```


### Task 5.4: Extend the code from Task 5.3 to also compute the mean from all passengers (use `margins=True`)

Store your answer in `result5_4`.

Hint: you may find the `margin` argument of the `df.pivot_table` useful.

```
              Fare                           Parch                      \
Sex         female       male        All    female      male       All   
Pclass                                                                   
1       106.125798  67.226127  84.154687  0.457447  0.278689  0.356481   
2        21.970121  19.741782  20.662183  0.605263  0.222222  0.380435   
3        16.118810  12.661633  13.675550  0.798611  0.224784  0.393075   
All      44.479818  25.523893  32.204208  0.649682  0.235702  0.381594   

           SibSp                      
Sex       female      male       All  
Pclass                                
1       0.553191  0.311475  0.416667  
2       0.486842  0.342593  0.402174  
3       0.895833  0.498559  0.615071  
All     0.694268  0.429809  0.523008
```


## Task 6: Fill in the Missing Fares


Fill in the missing `Fare` of passengers from each `Pclass` using the mean of their `Pclass` group.

Store your answer in `df2` (the DataFrame with missing `Fare` filled in).

```python
# Please use the following code to fake the missing age
df2 = df.copy()
drop_idx = df.sample(frac=0.1).index
df2.loc[drop_idx, 'Fare'] = np.nan
```

Once you have randomly removed the `Fare` of some passengers, the following code should print out the details of the passengers whose `Fare` are `NaN`.

Note: Due to the randomness, your passengers may be different from the following expected outputs. Nevertheless, their `Fare` column should all be `NaN`.

Expected output:

```python
print(df2.loc[drop_idx])

#      PassengerId  Survived  Pclass                           Name     Sex  \
# 356          357         1       1    Bowerman, Miss. Elsie Edith  female   
# 176          177         0       3  Lefebre, Master. Henry Forbes    male   
# 842          843         1       1        Serepeca, Miss. Augusta  female   
# 219          220         0       2             Harris, Mr. Walter    male   
# 73            74         0       3    Chronopoulos, Mr. Apostolos    male   
# ..           ...       ...     ...                            ...     ...   
# 45            46         0       3       Rogers, Mr. William John    male   
# 396          397         0       3            Olsson, Miss. Elina  female   
# 377          378         0       1      Widener, Mr. Harry Elkins    male   
# 784          785         0       3               Ali, Mr. William    male   
# 144          145         0       2     Andrew, Mr. Edgardo Samuel    male   

#            Age  SibSp  Parch              Ticket  Fare Cabin Embarked  \
# 356  22.000000      0      1              113505   NaN   E33        S   
# 176  29.699118      3      1                4133   NaN   UNK        S   
# 842  30.000000      0      0              113798   NaN   UNK        C   
# 219  30.000000      0      0           W/C 14208   NaN   UNK        S   
# 73   26.000000      1      0                2680   NaN   UNK        C   
# ..         ...    ...    ...                 ...   ...   ...      ...   
# 45   29.699118      0      0     S.C./A.4. 23567   NaN   UNK        S   
# 396  31.000000      0      0              350407   NaN   UNK        S   
# 377  27.000000      0      2              113503   NaN   C82        C   
# 784  25.000000      0      0  SOTON/O.Q. 3101312   NaN   UNK        S   
# 144  18.000000      0      0              231945   NaN   UNK        S   

#       Age_group  
# 356       Youth  
# 176  YoungAdult  
# 842  YoungAdult  
# 219  YoungAdult  
# 73   YoungAdult  
# ..          ...  
# 45   YoungAdult  
# 396  YoungAdult  
# 377  YoungAdult  
# 784       Youth  
# 144       Youth  

# [89 rows x 13 columns]
```

Once you have filled in the NaN values, the following code should print out the same passengers with their `Fare` column filled in with the group mean.

Note: Due to the randomness, your passengers may be filled in with diffent `Fare` mean values.

```python
print(df2.loc[drop_idx])

#      PassengerId  Survived  Pclass                           Name     Sex  \
# 356          357         1       1    Bowerman, Miss. Elsie Edith  female   
# 176          177         0       3  Lefebre, Master. Henry Forbes    male   
# 842          843         1       1        Serepeca, Miss. Augusta  female   
# 219          220         0       2             Harris, Mr. Walter    male   
# 73            74         0       3    Chronopoulos, Mr. Apostolos    male   
# ..           ...       ...     ...                            ...     ...   
# 45            46         0       3       Rogers, Mr. William John    male   
# 396          397         0       3            Olsson, Miss. Elina  female   
# 377          378         0       1      Widener, Mr. Harry Elkins    male   
# 784          785         0       3               Ali, Mr. William    male   
# 144          145         0       2     Andrew, Mr. Edgardo Samuel    male   

#            Age  SibSp  Parch              Ticket       Fare Cabin Embarked  \
# 356  22.000000      0      1              113505  83.889660   E33        S   
# 176  29.699118      3      1                4133  13.489582   UNK        S   
# 842  30.000000      0      0              113798  83.889660   UNK        C   
# 219  30.000000      0      0           W/C 14208  21.219711   UNK        S   
# 73   26.000000      1      0                2680  13.489582   UNK        C   
# ..         ...    ...    ...                 ...        ...   ...      ...   
# 45   29.699118      0      0     S.C./A.4. 23567  13.489582   UNK        S   
# 396  31.000000      0      0              350407  13.489582   UNK        S   
# 377  27.000000      0      2              113503  83.889660   C82        C   
# 784  25.000000      0      0  SOTON/O.Q. 3101312  13.489582   UNK        S   
# 144  18.000000      0      0              231945  21.219711   UNK        S   

#       Age_group  
# 356       Youth  
# 176  YoungAdult  
# 842  YoungAdult  
# 219  YoungAdult  
# 73   YoungAdult  
# ..          ...  
# 45   YoungAdult  
# 396  YoungAdult  
# 377  YoungAdult  
# 784       Youth  
# 144       Youth  

# [89 rows x 13 columns]
```
