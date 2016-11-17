# Morning Lecture

```
cur.fetchone()
```
 - fetches one line
 - line gets returned as a tuple

# Morning Breakout
 - PREVENT SQL INJECTION
  * using dict with psycopg2 will have psycopg2 check for you
```SQL
query = '''
        SELECT date, balance
        FROM transactions
        WHERE acct_num = %(account);
        '''
numbers = [12345, 23456]
for number in numbers:
    cur.execute(query, {'account':number})
```

 - What is the connection? What is the cursor?
  * Connection encapsulates a database session.
  * Cursor is method of connection and is bound to it.
  * It allows you to execute SQL commands in the session.
  * It points to a particular record somewhere.
  * It knows where that record is and how to find next one.
  * That's all it is!

# Afternoon Lecture

## Series

```python
import pandas as pd
prices = pd.Series([1,1,2,3,5],
    index = ['apple', 'pear', 'banana', 'mango', 'jackfruit'])

prices['pear']

#loc is actually a property - not a method
prices.loc['banana':]
```

### Differences between indexing methods:

 - `loc` works on labels in the index.
 - `iloc` works on the positions in the index (so it only takes integers).
 - `ix` usually tries to behave like loc but falls back to behaving like iloc if the label is not in the index. It's important to note some subtleties that can make ix slightly tricky to use:
  * if the index is of integer type, ix will only use label-based indexing and not fall back to position-based indexing. If the label is not in the index, an error is raised.
  * if the index does not contain only integers, then given an integer, ix will immediately use position-based indexing rather than label-based indexing. If however ix is given another type (e.g. a string), it can use label-based indexing.

### Various examples:

```python
inventory = pd.Series([10,50,41,22],
    index=['pear', 'banana', 'mango', 'apple'])

#will automatically align two DFs based on labels
#the one that is not in the other (jackfruit) gets NaN
inventory * prices

nonunique_inventory = pd.Series([10,50,41,22,50],
    index=['pear', 'banana', 'mango', 'apple', 'apple'])

#multiples both apple inventories by the price of apples
nonunique_inventory * prices

#returns boolean series
inventory > 20
#we can use this to index
inventory.loc[inventory > 20]

#there are many methods you can use
prices.mean()
prices.median()
prices.std()
prices.describe()

#this is stupid
for price in prices:
    price * 2

#to avoid this, use apply instead
#similar to map() but more efficient to use on Series
#discount anything over $3 by 10%
discount_prices = prices.apply(lambda x: 0.9*x if x>3 else x)
```

## Dataframes

Series are pandas analog to vector. Dataframe is pandas analog for matrix.

__A Dataframe is a dict of Series.__

```python
produce = pd.DataFrame({
    'price':prices,
    'discount_price':discount_prices,
    'inventory':inventory})

#now that we have two dimensions we specify what to do with both
#[what do to with ROWS, what to do with COLUMNS]
produce.loc['pear':,:]

produce.iloc[[0,2,3],:]

#.ix allows mixed integer and label indexing
produce.ix[[0,2,3],['discount_price']]
#OR
#this doesn't give exact same result
#'discount_price' appears as a name attribute here
#  rather than printing out as a column name
produce.iloc[[0,2,3],:].loc[:, 'discount_price']

#selecting a SINGLE column OR row --> Series
#selecting multiple columns --> Dataframe
## single  column
type(produce.discount_price) #pandas.core.series.Series
## single row
type(produce.ix[0]) #pandas.core.series.Series

#BOTH row and column names are indices
produce.columns
produce.index

#select all rows and only columns where max value is > 5
produce.loc[:, produce.max() > 5]
# basically using the following boolean index for columns
# returns us a Series of max for each column
produce.max()
produce.max() > 5

produce['inventory_value'] = produce['inventory'] * produce['price']
#this does nothing to the original DF
produce.drop('inventory_value', axis = 1)
#this will actually change it
produce.drop('inventory_value', axis = 1, inplace = True)
```

## Group By

__Split, Apply, Combine__

 - __Split__ data into grouped by groups
 - __Apply__ same function to all subgroups
 - __Combine__ recombine the individual outputs of functions into DataFrame

    ```python
     grocery = pd.DataFrame({
        'category':['produce', 'produce', 'meat', 'meat',
                'meat', 'cheese', 'cheese'],
        'item':['celery', 'apple', 'ham', 'turkey', 'lamb',
                'cheddar', 'brie'],
        'price':[.99, .49, 1.89, 4.34, 9.50, 6.25, 8.0]})

    #returns a groupby object that handles the split, apply, combina operations
    g = grocery.groupby('category')

    #computes means based on groups
    g.mean()
    g['price'].mean()
    ```

### Methods: take function as argument and apply to each split

  - __Group by__: method to group
  - __Apply__: returns anything
  - __Aggregate__: returns one value per group (e.g. mean, sum)
  - __Filter__: filter entire groups based on T/F
  - __Transform__: return the same shame as we started with that have been transformed in some way

    ```python
    import numpy as np
    grouped = grocery.groupby('category')

    #AGGREGATE
    grouped.aggregate(np.mean)

    #TRANSFORM
    #difference from mean
    grouped.transform(lambda x: x - x.mean())

    #FILTER
    #for each category if there are more than 2 items (i.e. rows) then keep
    #it can ONLY keep or lose ENTIRE split
    #x is a split group
    grouped.filter(lambda x: len(x) > 2)
    ```

# Afternoon Breakout

```python
# keep all of the items in each category that have a price
#   above average for that category

#can't use filter because we don't want to remove entire groups
grocery[grocery.groupby('category')['price'].apply(lambda x: x > x.mean())]

#another way
grocery.groupby('category')['price'].apply(
    lambda x: x if x > x.mean())

# because x is a Series (normally a Dataframe)
grocery.groupby('category').apply(keep_above_avg)

def keep_above_avg(x):
    return x[x.price > x.price.mean()]

# not the same as without grouping
keep_above_avg(grocery)
```

 - Pandas vs SQL
  * Pandas is probably faster than SQL because pandas is in memory while SQL is on disk and accessing memory is typically faster than accessing disk
  * Pandas has better analytical capabilities
  * There's nothing you can do in SQL that you can't do in pandas
