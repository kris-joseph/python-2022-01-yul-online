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
print(data.loc[0 "smoker"])
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

# Which values were greater than 20000 ?
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
much flexibility to analyse their data.

For instance, let's say we want to have a clearer view on how the European countries 
split themselves according to their GDP.

1.  We may have a glance by splitting the countries in two groups during the years surveyed,
    those who presented a GDP *higher* than the European average and those with a *lower* GDP.
2.  We then estimate a *wealthy score* based on the historical (from 1962 to 2007) values,
    where we account how many times a country has participated in the groups of *lower* or *higher* GDP

~~~
mask_higher = data > data.mean()
wealth_score = mask_higher.aggregate('sum', axis=1) / len(data.columns)
wealth_score
~~~
{: .language-python}
~~~
country
Albania                   0.000000
Austria                   1.000000
Belgium                   1.000000
Bosnia and Herzegovina    0.000000
Bulgaria                  0.000000
Croatia                   0.000000
Czech Republic            0.500000
Denmark                   1.000000
Finland                   1.000000
France                    1.000000
Germany                   1.000000
Greece                    0.333333
Hungary                   0.000000
Iceland                   1.000000
Ireland                   0.333333
Italy                     0.500000
Montenegro                0.000000
Netherlands               1.000000
Norway                    1.000000
Poland                    0.000000
Portugal                  0.000000
Romania                   0.000000
Serbia                    0.000000
Slovak Republic           0.000000
Slovenia                  0.333333
Spain                     0.333333
Sweden                    1.000000
Switzerland               1.000000
Turkey                    0.000000
United Kingdom            1.000000
dtype: float64
~~~
{: .output}

Finally, for each group in the `wealth_score` table, we sum their (financial) contribution
across the years surveyed using chained methods:

~~~
data.groupby(wealth_score).sum()
~~~
{: .language-python}
~~~
          gdpPercap_1952  gdpPercap_1957  gdpPercap_1962  gdpPercap_1967  \
0.000000    36916.854200    46110.918793    56850.065437    71324.848786   
0.333333    16790.046878    20942.456800    25744.935321    33567.667670   
0.500000    11807.544405    14505.000150    18380.449470    21421.846200   
1.000000   104317.277560   127332.008735   149989.154201   178000.350040   

          gdpPercap_1972  gdpPercap_1977  gdpPercap_1982  gdpPercap_1987  \
0.000000    88569.346898   104459.358438   113553.768507   119649.599409   
0.333333    45277.839976    53860.456750    59679.634020    64436.912960   
0.500000    25377.727380    29056.145370    31914.712050    35517.678220   
1.000000   215162.343140   241143.412730   263388.781960   296825.131210   

          gdpPercap_1992  gdpPercap_1997  gdpPercap_2002  gdpPercap_2007  
0.000000    92380.047256   103772.937598   118590.929863   149577.357928  
0.333333    67918.093220    80876.051580   102086.795210   122803.729520  
0.500000    36310.666080    40723.538700    45564.308390    51403.028210  
1.000000   315238.235970   346930.926170   385109.939210   427850.333420
~~~
{: .output}


> ## Selection of Individual Values
>
> Assume Pandas has been imported into your notebook
> and the Gapminder GDP data for Europe has been loaded:
>
> ~~~
> import pandas as pd
>
> df = pd.read_csv('data/gapminder_gdp_europe.csv', index_col='country')
> ~~~
> {: .language-python}
>
> Write an expression to find the Per Capita GDP of Serbia in 2007.
> > ## Solution
> > The selection can be done by using the labels for both the row ("Serbia") and the column ("gdpPercap_2007"):
> > ~~~
> > print(df.loc['Serbia', 'gdpPercap_2007'])
> > ~~~
> > {: .language-python}
> > The output is
> > ~~~
> > 9786.534714
> > ~~~
> >{: .output}
> {: .solution}
{: .challenge}

> ## Extent of Slicing
>
> 1.  Do the two statements below produce the same output?
> 2.  Based on this,
>     what rule governs what is included (or not) in numerical slices and named slices in Pandas?
> 
> ~~~
> print(df.iloc[0:2, 0:2])
> print(df.loc['Albania':'Belgium', 'gdpPercap_1952':'gdpPercap_1962'])
> ~~~
> {: .language-python}
> 
> > ## Solution
> > No, they do not produce the same output! The output of the first statement is:
> > ~~~
> >         gdpPercap_1952  gdpPercap_1957
> > country                                
> > Albania     1601.056136     1942.284244
> > Austria     6137.076492     8842.598030
> > ~~~
> >{: .output}
> > The second statement gives:
> > ~~~
> >         gdpPercap_1952  gdpPercap_1957  gdpPercap_1962
> > country                                                
> > Albania     1601.056136     1942.284244     2312.888958
> > Austria     6137.076492     8842.598030    10750.721110
> > Belgium     8343.105127     9714.960623    10991.206760
> > ~~~
> >{: .output}
> > Clearly, the second statement produces an additional column and an additional row compared to the first statement.  
> > What conclusion can we draw? We see that a numerical slice, 0:2, *omits* the final index (i.e. index 2)
> > in the range provided,
> > while a named slice, 'gdpPercap_1952':'gdpPercap_1962', *includes* the final element.
> {: .solution}
{: .challenge}

> ## Reconstructing Data
>
> Explain what each line in the following short program does:
> what is in `first`, `second`, etc.?
>
> ~~~
> first = pd.read_csv('data/gapminder_all.csv', index_col='country')
> second = first[first['continent'] == 'Americas']
> third = second.drop('Puerto Rico')
> fourth = third.drop('continent', axis = 1)
> fourth.to_csv('result.csv')
> ~~~
> {: .language-python}
>
> > ## Solution
> > Let's go through this piece of code line by line.
> > ~~~
> > first = pd.read_csv('data/gapminder_all.csv', index_col='country')
> > ~~~
> > {: .language-python}
> > This line loads the dataset containing the GDP data from all countries into a dataframe called 
> > `first`. The `index_col='country'` parameter selects which column to use as the 
> > row labels in the dataframe.  
> > ~~~
> > second = first[first['continent'] == 'Americas']
> > ~~~
> > {: .language-python}
> > This line makes a selection: only those rows of `first` for which the 'continent' column matches 
> > 'Americas' are extracted. Notice how the Boolean expression inside the brackets, 
> > `first['continent'] == 'Americas'`, is used to select only those rows where the expression is true. 
> > Try printing this expression! Can you print also its individual True/False elements? 
> > (hint: first assign the expression to a variable)
> > ~~~
> > third = second.drop('Puerto Rico')
> > ~~~
> > {: .language-python}
> > As the syntax suggests, this line drops the row from `second` where the label is 'Puerto Rico'. The 
> > resulting dataframe `third` has one row less than the original dataframe `second`.
> > ~~~
> > fourth = third.drop('continent', axis = 1)
> > ~~~
> > {: .language-python}
> > Again we apply the drop function, but in this case we are dropping not a row but a whole column. 
> > To accomplish this, we need to specify also the `axis` parameter (we want to drop the second column 
> > which has index 1).
> > ~~~
> > fourth.to_csv('result.csv')
> > ~~~
> > {: .language-python}
> > The final step is to write the data that we have been working on to a csv file. Pandas makes this easy 
> > with the `to_csv()` function. The only required argument to the function is the filename. Note that the 
> > file will be written in the directory from which you started the Jupyter or Python session.
> {: .solution}
{: .challenge}

> ## Selecting Indices
>
> Explain in simple terms what `idxmin` and `idxmax` do in the short program below.
> When would you use these methods?
>
> ~~~
> data = pd.read_csv('data/gapminder_gdp_europe.csv', index_col='country')
> print(data.idxmin())
> print(data.idxmax())
> ~~~
> {: .language-python}
>
> > ## Solution
> > For each column in `data`, `idxmin` will return the index value corresponding to each column's minimum;
> > `idxmax` will do accordingly the same for each column's maximum value.
> >
> > You can use these functions whenever you want to get the row index of the minimum/maximum value and not the actual minimum/maximum value.
> {: .solution}
{: .challenge}

> ## Practice with Selection
>
> Assume Pandas has been imported and the Gapminder GDP data for Europe has been loaded.
> Write an expression to select each of the following:
>
> 1.  GDP per capita for all countries in 1982.
> 2.  GDP per capita for Denmark for all years.
> 3.  GDP per capita for all countries for years *after* 1985.
> 4.  GDP per capita for each country in 2007 as a multiple of 
>     GDP per capita for that country in 1952.
>
> > ## Solution
> > 1:
> > ~~~
> > data['gdpPercap_1982']
> > ~~~
> > {: .language-python}
> >
> > 2:
> > ~~~
> > data.loc['Denmark',:]
> > ~~~
> > {: .language-python}
> >
> > 3:
> > ~~~
> > data.loc[:,'gdpPercap_1985':]
> > ~~~
> > {: .language-python}
> > Pandas is smart enough to recognize the number at the end of the column label and does not give you an error, although no column named `gdpPercap_1985` actually exists. This is useful if new columns are added to the CSV file later.
> >
> > 4:
> > ~~~
> > data['gdpPercap_2007']/data['gdpPercap_1952']
> > ~~~
> > {: .language-python}
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
> Assume Pandas has been imported and the Gapminder GDP data for Europe has been loaded as `data`.  Then, use `dir()` 
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


> ## Interpretation
>
> Poland's borders have been stable since 1945,
> but changed several times in the years before then.
> How would you handle this if you were creating a table of GDP per capita for Poland
> for the entire twentieth century?
{: .challenge}


[pandas-dataframe]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/