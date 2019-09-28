[1] [summarized mainly from this kaggle kernel] (https://www.kaggle.com/kashnitsky/topic-1-exploratory-data-analysis-with-pandas)

[2] [more examples about group by and agg] (https://www.shanelynn.ie/summarising-aggregation-and-grouping-data-in-python-pandas/)

main data structures - Series (1D) and Dataframe (2D)

 ### 1. display options
```python
pd.set_option("display.precision", 2)

```
 ### 2. loading
```python
df = pd.read_csv('____.csv')
```

 ### 3. displaying 
```python
df.head()    #display first 5 rows of dataframe
df[-1:]      #indexing the last row
df[:1] ]     #indexing the first row 
data['network'].nunique()  # Number of non-null unique network entries [2]

```

  * #### sorting by 1 column
  ```python
  df.sort_values(by='col_name', ascending=False).head()
  ```
  
  * #### sorting by multiple columns
  ```python
  df.sort_values(by=['col_name1','col_name2'], ascending=[True,False]).head()
  ```
  
   * #### indexing by name (loc)
  ```python
  df.loc[0:5, 'state': 'area_Code'] # first 6 rows (5 inclusive) and columns labelled from state to area code
  ```
   * #### indexed by number (iloc)
  ```python
  df.iloc[0:5, 0:3] #first 5 rows (5 not inclusive) and first 3 columns.
  ```
  
  * #### apply function to cols
  ```python
  df.apply(np.max) #displays max of each column - can add axis=1 for rows but lambda functions are more convenient
  ```
  
  * #### apply function to rows - lambda function
  ```python
  df[df['col'].apply(lambda s: s[0] == 'W')].head() #displays rows filtered by that column where value starts with W
  ```
  
   * #### map function to replace values
   replace values in a column - by passing a dictionary as its argument- {old value: new value}
  ```python
  d= {'No': False, 'Yes': True}
  df['col'] = df['col'].map(d)
  ```
   * #### replace function
   also replace values in a column - {name of column: dictionary itself}
  ```python
  df = df.replace({'col': d})
  ```


  * #### grouping
  ```python
  #divides data by values of grouping columns and then selects the columns to show or all columns if not specified
  #then function or several is applied to the obtained group
  df.groupby(by=grouping_columns)[columns_to_show].function()
  
  columns_to_show = ['Total day minutes', 'Total eve minutes', 
                   'Total night minutes']
  df.groupby(['Churn'])[columns_to_show].describe(percentiles=[])
  
  ```
  examples from [2]
```python
# Get the sum of the durations per month
data.groupby('month')['duration'].sum()

# Get the number of dates / entries in each month
data.groupby('month')['date'].count()

# What is the sum of durations, for calls only, to each network
data[data['item'] == 'call'].groupby('network')['duration'].sum()

# grouping by more than one variable
# How many calls, sms, and data entries are in each month?
data.groupby(['month', 'item'])['date'].count()

```

  * #### grouping + agg
  agg() - pass a function or list of functions
  
  ```python
   df.groupby(['Churn'])[columns_to_show].agg([np.mean, np.std, np.min, 
                                            np.max])
                                            
  grouped = data.groupby('month').agg("duration": [min, max, mean]) 
# Using ravel, and a string join, we can create better names for the columns:
grouped.columns = ["_".join(x) for x in grouped.columns.ravel()]

```

from [2]
  ```python
aggregations = {
    'duration':'sum',
    'date': lambda x: max(x) - 1
}
data.groupby('month').agg(aggregations)


# Group the data frame by month and item and extract a number of stats from each group
data.groupby(
    ['month', 'item']
).agg(
    {
        # find the min, max, and sum of the duration column
        'duration': [min, max, sum],
         # find the number of network type entries
        'network_type': "count",
        # min, first, and number of unique dates per group
        'date': [min, 'first', 'nunique']
    }

  ```
  
  
  

### 4. stats about dataframe

* #### shape
```python
df.shape     # (rows, column) ex: (3333,20)
```
* #### column names
```python
df.columns   # column names
```

```python
df.info      # column names + no. of rows + type (bool, int64, float64, object) - CAN FIND IF MISSING ENTRIES (don't match up to no. of rows in shape)
```

* #### stats of numerical features
```python
df.describe() #stats of all numerical features - no. of non missing values(Count), mean, std dev, range, median, 0.25 and 0.75 quartiles
```


* #### stats of non-numerical features
```python
df.describe(include=['object', 'bool'])
df['col_name'].value_counts()  # in case of bool - distribution of how many rows have 0, and how many are 1
df['col_name'].value_counts(normalize=True) #for fractions

```
  * #### col specific indexing +  stats 
    
```python
df['col_name'].mean() #df['Churn'].mean()
```

  * ####  boolean indexing + stats
```python
df[df['col_name'] == 1].mean() #stats of all the rows which have for ex. churn ==1

#getting mean of a specific column where ex. churn ==1
df[df['col_name'] == 1]['col_name2'].mean()

#getting max number for a specific column based off 2 column filters  (max of col 3 if col 1 and col 3 are satisfied)
df[(df['col_name'] == 1) & (df['col_name2'] == 'No') ]['col_name3'].max()


```
### 5. manipulation
```python
df['col_name'] = df['col_name'].astype('int64')  #converting bool to int64

```
