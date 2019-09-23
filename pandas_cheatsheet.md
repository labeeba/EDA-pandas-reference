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
```

  * #### sorting by 1 column
  ```python
  df.sort_values(by='col_name', ascending=False).head()
  ```
  
  * #### sorting by multiple columns
  ```python
  df.sort_values(by=['col_name1','col_name2'], ascending=[True,False]).head()
  ```
    

### 4. stats about dataframe
```python
df.shape     # (rows, column) ex: (3333,20)
df.columns   # column names
df.info      # column names + no. of rows + type (bool, int64, float64, object) - CAN FIND IF MISSING ENTRIES (don't match up to no. of rows in shape)
df.describe() #stats of all numerical features - no. of non missing values(Count), mean, std dev, range, median, 0.25 and 0.75 quartiles

#stats of non-numerical features
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


```
### 5. manipulation
```python
df['col_name'] = df['col_name'].astype('int64')  #converting bool to int64

```
