---
title: "Pandas DataFrames"
teaching: 15
exercises: 15
questions:
- "How can I do statistical analysis of tabular data?"
objectives:
- "Select individual values from a Pandas dataframe."
- "Select entire rows or entire columns from a dataframe."
- "Select a subset of both rows and columns from a dataframe in a single operation."
- "Select a subset of a dataframe by a single Boolean criterion."
keypoints:
- "Use `DataFrame.iloc[..., ...]` to select values by integer location."
- "Use `:` on its own to mean all columns or all rows."
- "Select multiple columns or rows using `DataFrame.loc` and a named slice."
- "Result of slicing can be used in further operations."
- "Use comparisons to select data based on value."
- "Select values or NaN using a Boolean mask."
---

## Note about Pandas DataFrames/Series

A [DataFrame][pandas-dataframe] is a collection of [Series][pandas-series];
The DataFrame is the way Pandas represents a table, and Series is the data-structure
Pandas use to represent a column.

Pandas is built on top of the [Numpy][numpy] library, which in practice means that
most of the methods defined for Numpy Arrays apply to Pandas Series/DataFrames.

What makes Pandas so attractive is the powerful interface to access individual records
of the table, proper handling of missing values, and relational-databases operations
between DataFrames.

## Selecting values

To access a value at the position `[i,j]` of a DataFrame, we have two options, depending on
what is the meaning of `i` in use.
Remember that a DataFrame provides an *index* as a way to identify the rows of the table;
a row, then, has a *position* inside the table as well as a *label*, which
uniquely identifies its *entry* in the DataFrame.

## Use `DataFrame.iloc[..., ...]` to select values by their (entry) position

*   Can specify location by numerical index analogously to 2D version of character selection in strings.

~~~
# import the Pandas library and read data from the insurance.csv file
import pandas as pd
data = pd.read_csv('insurance.csv')

# Output the first three rows of data
print(data.head(n=3))

# Output the data in row 0, column 0
print(data.iloc[0, 0])
~~~
{: .language-python}
~~~
19
~~~
{: .output}

* Remember that Pandas automatically uses the first row of a CSV file to determine the names for data columns
* Since Python counts rows and columns starting at 0, location `[0, 0]` is the data in the first column of the first row: 19

## Use `DataFrame.loc[..., ...]` to select values by their (entry) label.

*   You can also specify location by column and/or row name, in a similar way using dictionary keys.

~~~
print(data.loc[0, "smoker"])
~~~
{: .language-python}
~~~
yes
~~~
{: .output}

*   In our data set the rows are numbered instead of indexed, so we specify the row of interest with an index number, and specify the data point of interest by providing the column name.

## Use `:` on its own to mean all columns or all rows.

*   Just like Python's usual slicing notation.

~~~
print(data.loc[3, :])
~~~
{: .language-python}
~~~
age                  33
sex                male
bmi              22.705
children              0
smoker               no
region        northwest
charges     21984.47061
Name: 3, dtype: object
~~~
{: .output}

*   Would get the same result as printing `data.loc[3, :]` by printing `data.loc[3]` (without a second index).

~~~
print(data.loc[:, "smoker"])
~~~
{: .language-python}
~~~
0       yes
1        no
2        no
3        no
4        no
       ... 
1333     no
1334     no
1335     no
1336     no
1337    yes
Name: smoker, Length: 1338, dtype: object
~~~
{: .output}

*   You would get the same result printing `data["smoker"]`
*   You also get the same result printing `data.smoker` (but this is not recommended because it is easily confused with the `.` notation for methods)

## Select multiple columns or rows using `DataFrame.loc` and a named slice.

~~~
print(data.loc[0:5, 'smoker':'charges'])
~~~
{: .language-python}
~~~
  smoker     region      charges
2     no  southeast   4449.46200
3     no  northwest  21984.47061
4     no  northwest   3866.85520
5     no  southeast   3756.62160
~~~
{: .output}

In the above code, we discover that **slicing using `loc` is inclusive at both
ends**, which differs from **slicing using `iloc`**, where slicing indicates
everything up to but not including the final index. 


## Result of slicing can be used in further operations.

*   Usually, we want to do more than just print out the contents of a slice.
*   All the statistical operators that work on entire dataframes
    work the same way on slices.
*   For example, let's calculate maximum value within a slice.

~~~
print(data.loc[100:500, 'bmi':'children'].max())
~~~
{: .language-python}
~~~
bmi         49.06
children     5.00
dtype: float64
~~~
{: .output}

~~~
print(data.loc[100:500, 'bmi':'children'].min())
~~~
{: .language-python}
~~~
bmi         15.96
children     0.00
dtype: float64
~~~
{: .output}

## Use comparisons to select data based on value.

*   Comparison is applied element by element.
*   Returns a similarly-shaped dataframe of `True` and `False`.

~~~
# Use a subset of data to keep output readable.
subset = data.loc[0:20, 'bmi':'children']
print('Subset of data:\n', subset)

# Which values were greater than 30 ?
print('\nWhere are values large?\n', subset > 30)
~~~
{: .language-python}
~~~
Subset of data:
        bmi  children
0   27.900         0
1   33.770         1
2   33.000         3
3   22.705         0
4   28.880         0
5   25.740         0
6   33.440         1
7   27.740         3
8   29.830         2
9   25.840         0
10  26.220         0
11  26.290         0
12  34.400         0
13  39.820         0
14  42.130         0
15  24.600         1
16  30.780         1
17  23.845         0
18  40.300         0
19  35.300         0
20  36.005         0

Where are values large?
       bmi  children
0   False     False
1    True     False
2    True     False
3   False     False
4   False     False
5   False     False
6    True     False
7   False     False
8   False     False
9   False     False
10  False     False
11  False     False
12   True     False
13   True     False
14   True     False
15  False     False
16   True     False
17  False     False
18   True     False
19   True     False
20   True     False
~~~
{: .output}

## Select values or NaN using a Boolean mask.

*   A frame full of Booleans is sometimes called a *mask* because of how it can be used.

~~~
mask = subset > 30
print(subset[mask])
~~~
{: .language-python}
~~~
       bmi  children
0      NaN       NaN
1   33.770       NaN
2   33.000       NaN
3      NaN       NaN
4      NaN       NaN
5      NaN       NaN
6   33.440       NaN
7      NaN       NaN
8      NaN       NaN
9      NaN       NaN
10     NaN       NaN
11     NaN       NaN
12  34.400       NaN
13  39.820       NaN
14  42.130       NaN
15     NaN       NaN
16  30.780       NaN
17     NaN       NaN
18  40.300       NaN
19  35.300       NaN
20  36.005       NaN
~~~
{: .output}

*   In the output, the value appears where the mask is true, and NaN (Not a Number) apears where it is false.
*   This is useful because NaNs are ignored by operations like max, min, average, etc.

~~~
print(subset[subset > 30].describe())
~~~
{: .language-python}
~~~
             bmi  children
count  10.000000       0.0
mean   35.894500       NaN
std     3.672313       NaN
min    30.780000       NaN
25%    33.522500       NaN
50%    34.850000       NaN
75%    38.866250       NaN
max    42.130000       NaN["smoker"]
~~~
{: .output}

## Group By: split-apply-combine

Pandas vectorizing methods and grouping operations are features that provide users 
much flexibility to analyse their data. Often, we want to process data this way:
1. Split the data into groups by selecting elements that meet some criteria
2. Apply a function to each group (for example, calculate a sum; perform a count; do staistical operations)
3. Combine the results into a data structure

For instance, let's say we want to do a tally of how many patients in each region have more than 3 children.

First, we'll use the "mask" technique to filter out all of the data where the number of children is 3 or less:

~~~
mask_moreThanThreeChildren = data.loc[:,"children"] > 3
print(mask_moreThanThreeChildren)
~~~
{: .language-python}
~~~
0       False
1       False
2       False
3       False
4       False
        ...  
1333    False
1334    False
1335    False
1336    False
1337    False
Name: children, Length: 1338, dtype: bool
~~~
{: .output}

Not many people will have 4+ children, so the we expect to see a lot of "False" results in the data. 

Next, we want to group this data by region so we can count how many people in each region have at least 4 kids.

Pandas provides a powerful "groupby" function that will help us do this. If we pass it the "region" column name, it will create one group for each of the unique values in the column. We have a northeast, northwest, southeast, and southwest region, so we would expect to see four groups.

We can count how many elements have been placed into each group using a built-in size() function. 

Putting this all together, we will do as follows by chaining together commands:
1. use the mask to create a subset of our data, listing only those people with more than three children
2. use groupby() to create one group of records for each region
3. use size() to see how large each group is (i.e. how many records there are)

~~~
grouped = data[mask_moreThanThreeChildren].groupby("region").size()
print(grouped)

# We can also avoid using a variable and write print(data.groupby("region").size())
~~~
{: .language-python}

~~~
region
northeast    10
northwest     7
southeast    11
southwest    15
dtype: int64

~~~
{: .output}


> ## Selection of Individual Values
>
> Assume Pandas has been imported into your notebook
> and the insurance data has been loaded:
>
> ~~~
> import pandas as pd
>
> df = pd.read_csv('insurance.csv')
> ~~~
> {: .language-python}
>
> Write an expression to find the average charge for all smokers according to sex.
> > ## Solution
> > The selection can be done by "masking" for smokers, grouping by sex, and then calculating the mean. :
> > ~~~
> > mask_smokers = df.loc[:,"smoker"] == 'yes'
> > grouped = df[mask_smokers].groupby("sex")
> > averages = grouped.mean()
> > print(averages.loc[:,"charges"])
> > ~~~
> > {: .language-python}
> > The output is
> > ~~~
> > sex
> > female    30678.996276
> > male      33042.005975
> > Name: charges, dtype: float64
> > ~~~
> >{: .output}
> {: .solution}
{: .challenge}


> ## Many Ways of Access
>
> There are at least two ways of accessing a value or slice of a DataFrame: by name or index.
> However, there are many others. For example, a single column or row can be accessed either as a `DataFrame`
> or a `Series` object.
>
> Suggest different ways of doing the following operations on a DataFrame:
> 1. Access a single column
> 2. Access a single row
> 3. Access an individual DataFrame element
> 4. Access several columns
> 5. Access several rows
> 6. Access a subset of specific rows and columns
> 7. Access a subset of row and column ranges
>
{: .challenge}
>
> > ## Solution
> > 1\. Access a single column:
> > ~~~
> > # by name
> > data["col_name"]   # as a Series
> > data[["col_name"]] # as a DataFrame
> >
> > # by name using .loc
> > data.T.loc["col_name"]  # as a Series
> > data.T.loc[["col_name"]].T  # as a DataFrame
> >
> > # Dot notation (Series)
> > data.col_name
> >
> > # by index (iloc)
> > data.iloc[:, col_index]   # as a Series
> > data.iloc[:, [col_index]] # as a DataFrame
> >
> > # using a mask
> > data.T[data.T.index == "col_name"].T
> > ~~~
> > {: .language-python}
> >
> > 2\. Access a single row:
> > ~~~
> > # by name using .loc
> > data.loc["row_name"] # as a Series
> > data.loc[["row_name"]] # as a DataFrame
> >
> > # by name
> > data.T["row_name"] # as a Series
> > data.T[["row_name"]].T as a DataFrame
> >
> > # by index
> > data.iloc[row_index]   # as a Series
> > data.iloc[[row_index]]   # as a DataFrame
> >
> > # using mask
> > data[data.index == "row_name"]
> > ~~~
> > {: .language-python}
> >
> > 3\. Access an individual DataFrame element:
> > ~~~
> > # by column/row names
> > data["column_name"]["row_name"]         # as a Series
> >
> > data[["col_name"]].loc["row_name"]  # as a Series
> > data[["col_name"]].loc[["row_name"]]  # as a DataFrame
> >
> > data.loc["row_name"]["col_name"]  # as a value
> > data.loc[["row_name"]]["col_name"]  # as a Series
> > data.loc[["row_name"]][["col_name"]]  # as a DataFrame
> >
> > data.loc["row_name", "col_name"]  # as a value
> > data.loc[["row_name"], "col_name"]  # as a Series. Preserves index. Column name is moved to `.name`.
> > data.loc["row_name", ["col_name"]]  # as a Series. Index is moved to `.name.` Sets index to column name.
> > data.loc[["row_name"], ["col_name"]]  # as a DataFrame (preserves original index and column name)
> >
> > # by column/row names: Dot notation
> > data.col_name.row_name
> >
> > # by column/row indices
> > data.iloc[row_index, col_index] # as a value
> > data.iloc[[row_index], col_index] # as a Series. Preserves index. Column name is moved to `.name`
> > data.iloc[row_index, [col_index]] # as a Series. Index is moved to `.name.` Sets index to column name.
> > data.iloc[[row_index], [col_index]] # as a DataFrame (preserves original index and column name)
> >
> > # column name + row index
> > data["col_name"][row_index]
> > data.col_name[row_index]
> > data["col_name"].iloc[row_index]
> >
> > # column index + row name
> > data.iloc[:, [col_index]].loc["row_name"]  # as a Series
> > data.iloc[:, [col_index]].loc[["row_name"]]  # as a DataFrame
> >
> > # using masks
> > data[data.index == "row_name"].T[data.T.index == "col_name"].T
> > ~~~
> > {: .language-python}
> > 4\. Access several columns:
> > ~~~
> > # by name
> > data[["col1", "col2", "col3"]]
> > data.loc[:, ["col1", "col2", "col3"]]
> >
> > # by index
> > data.iloc[:, [col1_index, col2_index, col3_index]]
> > ~~~
> > {: .language-python}
> > 5\. Access several rows
> > ~~~
> > # by name
> > data.loc[["row1", "row2", "row3"]]
> >
> > # by index
> > data.iloc[[row1_index, row2_index, row3_index]]
> > ~~~
> > {: .language-python}
> > 6\. Access a subset of specific rows and columns
> > ~~~
> > # by names
> > data.loc[["row1", "row2", "row3"], ["col1", "col2", "col3"]]
> >
> > # by indices
> > data.iloc[[row1_index, row2_index, row3_index], [col1_index, col2_index, col3_index]]
> >
> > # column names + row indices
> > data[["col1", "col2", "col3"]].iloc[[row1_index, row2_index, row3_index]]
> >
> > # column indices + row names
> > data.iloc[:, [col1_index, col2_index, col3_index]].loc[["row1", "row2", "row3"]]
> > ~~~
> > {: .language-python}
> > 7\. Access a subset of row and column ranges
> > ~~~
> > # by name
> > data.loc["row1":"row2", "col1":"col2"]
> >
> > # by index
> > data.iloc[row1_index:row2_index, col1_index:col2_index]
> >
> > # column names + row indices
> > data.loc[:, "col1_name":"col2_name"].iloc[row1_index:row2_index]
> >
> > # column indices + row names
> > data.iloc[:, col1_index:col2_index].loc["row1":"row2"]
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

> ## Exploring available methods using the `dir()` function
>
> Python includes a `dir()` function that can be used to display all of the available methods (functions) that are built into a data object.  In Episode 4, we used some methods with a string. But we can see many more are available by using `dir()`:
>
> ~~~
> my_string = 'Hello world!'   # creation of a string object 
> dir(my_string)
> ~~~
> {: .language-python}
>
> This command returns:
>
> ~~~
> ['__add__',
> ...
> '__subclasshook__',
> 'capitalize',
> 'casefold',
> 'center',
> ...
> 'upper',
> 'zfill']
> ~~~
> {: .language-python}
>
> You can use `help()` or <kbd>Shift</kbd>+<kbd>Tab</kbd> to get more information about what these methods do.
>
> Assume Pandas has been imported and the insurance data has been loaded as `data`.  Then, use `dir()` 
> to find the function that prints out the median per-capita GDP across all European countries for each year that information is available.
>
> > ## Solution
> > Among many choices, `dir()` lists the `median()` function as a possibility.  Thus,
> > ~~~
> > data.median()
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Additional Resources

[pandas-dataframe]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/

## Your assignment

For next class, complete the following exercise:

Use what you've learned to write a script that uses the Pandas library to reads the insurance cost data from `insurance.csv` and outputs summary statistics for data in the 'BMI' column, grouped by smokers and non-smokers. Include the minimum value, maximum value, median, and average.

**Hint**: The Pandas library is probably more helpful than your code from the previous assignment. Look at the examples in this episode to piece your code together!