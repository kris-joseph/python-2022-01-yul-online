---
title: "Reading Tabular Data into DataFrames"
teaching: 10
exercises: 10
questions:
- "How can I read tabular data?"
objectives:
- "Import the Pandas library."
- "Use Pandas to load a simple CSV data set."
- "Get some basic information about a Pandas DataFrame."
keypoints:
- "Use the Pandas library to get basic statistics out of tabular data."
- "Use `index_col` to specify that a column's values should be used as row headings."
- "Use `DataFrame.info` to find out more about a dataframe."
- "The `DataFrame.columns` variable stores information about the dataframe's columns."
- "Use `DataFrame.T` to transpose a dataframe."
- "Use `DataFrame.describe` to get summary statistics about data."
---
## Use the Pandas library to do statistics on tabular data.

*   Pandas is a widely-used Python library for statistics, particularly on tabular data.
*   Borrows many features from R's dataframes.
    *   A 2-dimensional table whose columns have names
        and potentially have different data types.
*   Load it with `import pandas as pd`. The alias pd is commonly used for Pandas.
*   Read a Comma Separated Values (CSV) data file with `pd.read_csv`.
    *   Argument is the name of the file to be read.
    *   Assign result to a variable to store the data that was read.

~~~
import pandas as pd

data = pd.read_csv('insurance.csv')
print(data)
~~~
{: .language-python}
~~~
      age     sex     bmi  children smoker     region      charges
0      19  female  27.900         0    yes  southwest  16884.92400
1      18    male  33.770         1     no  southeast   1725.55230
2      28    male  33.000         3     no  southeast   4449.46200
3      33    male  22.705         0     no  northwest  21984.47061
4      32    male  28.880         0     no  northwest   3866.85520
...   ...     ...     ...       ...    ...        ...          ...
1333   50    male  30.970         3     no  northwest  10600.54830
1334   18  female  31.920         0     no  northeast   2205.98080
1335   18  female  36.850         0     no  southeast   1629.83350
1336   21  female  25.800         0     no  southwest   2007.94500
1337   61  female  29.070         0    yes  northwest  29141.36030

[1338 rows x 7 columns]
~~~
{: .output}

*   The columns in a dataframe are the observed variables, and the rows are the observations.
*   In a case where the output is too wide to fit the screen (due to the number of columns), Pandas will use backslashes `\` to show wrapped lines.

> ## File Not Found
>
> Our lessons store their data files in the same directory as the Python notebook, but this is not always the best practice.
> Often, coders will keep data files in a separate subdirectory -- often called something like `data` -- in which case the path
> to the file must indicate that Python needs to look in the subdirectory for the file.
> If we were using a `data` subdirectory, we would have to use  `data/insurance.csv` as the argument for the `read_csv` method.
> If you do not supply accurate information for Python to locate the data file,
> you will get a [runtime error]({{ page.root }}/05-built-in/#runtime-error)
> that ends with a line like this:
>
> ~~~
> FileNotFoundError: [Errno 2] No such file or directory: 'data/insurance.csv'
> ~~~
> {: .error}
{: .callout}

## Use `index_col` to specify that a column's values should be used as row headings.

*   Row headings are numbers (0 and 1 in this case).
*   Let's say we want to index our data by the `bmi` column instead of relying on the row numbers from the first example. We are assuming here that all of our BMI values are unique -- ideally, the column we use as an index should be able to uniquely-identify a record. For example, for personal data you might use a "unique" observation like a phone number or membership number as an index.
*   To do this, pass the name of the column to `read_csv` as its `index_col` parameter.

~~~
data = pd.read_csv('insurance.csv', index_col='bmi')
print(data)
~~~
{: .language-python}
~~~
        age     sex  children smoker     region      charges
bmi                                                         
27.900   19  female         0    yes  southwest  16884.92400
33.770   18    male         1     no  southeast   1725.55230
33.000   28    male         3     no  southeast   4449.46200
22.705   33    male         0     no  northwest  21984.47061
28.880   32    male         0     no  northwest   3866.85520
...     ...     ...       ...    ...        ...          ...
30.970   50    male         3     no  northwest  10600.54830
31.920   18  female         0     no  northeast   2205.98080
36.850   18  female         0     no  southeast   1629.83350
25.800   21  female         0     no  southwest   2007.94500
29.070   61  female         0    yes  northwest  29141.36030

[1338 rows x 6 columns]
~~~
{: .output}

## Use the `DataFrame.info()` method to find out more about a dataframe.

~~~
data.info()
~~~
{: .language-python}
~~~
<class 'pandas.core.frame.DataFrame'>
Float64Index: 1338 entries, 27.9 to 29.07
Data columns (total 6 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   age       1338 non-null   int64  
 1   sex       1338 non-null   object 
 2   children  1338 non-null   int64  
 3   smoker    1338 non-null   object 
 4   region    1338 non-null   object 
 5   charges   1338 non-null   float64
dtypes: float64(1), int64(2), object(3)
memory usage: 73.2+ KB
~~~
{: .output}

*   This is a `DataFrame`
*   Our index colum is BMI
*   Six columns, containing float, int, and object values.
    *   Why isn't it 7 columns?
    *   We will talk later about null values, which are used to represent missing observations.
    *   Why are only six columns? This is because we are using the one of the columns (bmi) as the index
*   Uses 73.2 kilobytes of memory.

## The `DataFrame.columns` variable stores information about the dataframe's columns.

*   Note that this is a method, *not* a function (it doesn't have parentheses) -- it is a property of the DataFrame object. 
    *   This is similar to `math.pi`.
    *   No need to use `()` to try to call it.
*   Called a *member variable*, or just *member*.

~~~
data = pd.read_csv('insurance.csv')  # Reload the data WITHOUT using the bmi column as the index
print(data.columns)
~~~
{: .language-python}
~~~
Index(['age', 'sex', 'bmi', 'children', 'smoker', 'charges'], dtype='object')
~~~
{: .output}

## Use `DataFrame.T` to transpose a dataframe.

*   Sometimes want to treat columns as rows and vice versa.
*   Transpose (written `.T`) doesn't copy the data, just changes the program's view of it.
*   Like `columns`, it is a member variable.

~~~
print(data.T)
~~~
{: .language-python}
~~~
bmi          27.900     33.770     33.000       22.705     28.880     25.740  \
age              19         18         28           33         32         31   
sex          female       male       male         male       male     female   
children          0          1          3            0          0          0   
smoker          yes         no         no           no         no         no   
region    southwest  southeast  southeast    northwest  northwest  southeast   
charges   16884.924  1725.5523   4449.462  21984.47061  3866.8552  3756.6216   

bmi          33.440     27.740     29.830       25.840  ...       24.225  \
age              46         37         37           60  ...           23   
sex          female     female       male       female  ...       female   
children          1          3          2            0  ...            2   
smoker           no         no         no           no  ...           no   
region    southeast  northwest  northeast    northwest  ...    northeast   
charges   8240.5896  7281.5056  6406.4107  28923.13692  ...  22395.74424   

bmi          38.600      25.740       33.400     44.700      30.970  \
age              52          57           23         52          50   
sex            male      female       female     female        male   
children          2           2            0          3           3   
smoker           no          no           no         no          no   
region    southwest   southeast    southwest  southwest   northwest   
charges   10325.206  12629.1656  10795.93733  11411.685  10600.5483   

bmi          31.920     36.850     25.800      29.070  
age              18         18         21          61  
sex          female     female     female      female  
children          0          0          0           0  
smoker           no         no         no         yes  
region    northeast  southeast  southwest   northwest  
charges   2205.9808  1629.8335   2007.945  29141.3603  

[6 rows x 1338 columns] 
~~~
{: .output}

* Note that Pandas is now using the backslash `\` to display columns across multiple rows

## Use `DataFrame.describe()` to get summary statistics about data.

`DataFrame.describe()` gets the summary statistics of only the columns that have numerical data. 
All other columns are ignored, unless you use the argument `include='all'`.
~~~
print(data.describe())
~~~
{: .language-python}
~~~
               age     children       charges
count  1338.000000  1338.000000   1338.000000
mean     39.207025     1.094918  13270.422265
std      14.049960     1.205493  12110.011237
min      18.000000     0.000000   1121.873900
25%      27.000000     0.000000   4740.287150
50%      39.000000     1.000000   9382.033000
75%      51.000000     2.000000  16639.912515
max      64.000000     5.000000  63770.428010
~~~
{: .output}

*   Note that Pandas uses floating point numbers for all of these statistics

> ## Inspecting Data
>
> Use `help(data.head)` and `help(data.tail)`
> to find out what `DataFrame.head` and `DataFrame.tail` do.
>
> 1.  What method call will display the first three rows of this data?
> 2.  What method call will display the last three columns of this data?
>     (Hint: you may need to change your view of the data.)
>
> > ## Solution
> > 1. We can check out the first five rows of `data` by executing `data.head()`
> >    (allowing us to view the head of the DataFrame). We can specify the number of rows we wish
> >    to see by specifying the parameter `n` in our call
> >    to `data.head()`. To view the first three rows, execute:
> >
> >    ~~~
> >    data.head(n=3)
> >    ~~~
> >    {: .language-python}
> >    ~~~
> >     age     sex     children    smoker  region  charges
> > bmi                         
> > 27.90   19  female  0   yes     southwest   16884.9240
> > 33.77   18  male    1   no  southeast   1725.5523
> > 33.00   28  male    3   no  southeast   4449.4620
> >    ~~~
> >    {: .output}
> > 2. To check out the last three rows of `data`, we would use the command,
> >    `data.tail(n=3)`, analogous to `head()` used above. However, here we want to look at
> >    the last three columns so we need to change our view and then use `tail()`. To do so, we
> >     create a new DataFrame in which rows and columns are switched:
> >
> >    ~~~
> >    data_flipped = data.T
> >    ~~~
> >    {: .language-python}
> >
> >    We can then view the last three columns of `americas` by viewing the last three rows
> >    of `americas_flipped`:
> >    ~~~
> >    data_flipped.tail(n=3)
> >    ~~~
> >    {: .language-python}
> >    ~~~
> > bmi     27.900  33.770  33.000  22.705  28.880  25.740  33.440  27.740  29.830  25.840  ...     24.225  38.600  25.740  33.400  44.700  30.970  31.920  36.850  25.800  29.070
> > smoker  yes     no  no  no  no  no  no  no  no  no  ...     no  no  no  no  no  no  no  no  no  yes
> > region  southwest   southeast   southeast   northwest   northwest   southeast   southeast   northwest   northeast   northwest   ...     northeast   southwest   southeast   southwest   southwest   northwest   northeast   southeast   southwest   northwest
> > charges     16884.924   1725.5523   4449.462    21984.47061     3866.8552   3756.6216   8240.5896   7281.5056   6406.4107   28923.13692     ...     22395.74424     10325.206   12629.1656  10795.93733     11411.685   10600.5483  2205.9808   1629.8335   2007.945    29141.3603
> > 
> > 3 rows ?? 1338 columns
> >    ~~~
> >    {: .output}
> >    
> >    This shows the data that we want, but we may prefer to display three columns instead of three rows,
> >    so we can flip it back:
> >    ~~~
> >    data_flipped.tail(n=3).T    
> >    ~~~
> >    {: .language-python}    
> >    __Note:__ we could have done the above in a single line of code by 'chaining' the commands:
> >    ~~~
> >    data.T.tail(n=3).T
> >    ~~~
> >    {: .language-python}
> {: .solution}
{: .challenge}


> ## Writing Data
> 
> As well as the `read_csv` function for reading data from a file,
> Pandas provides a `to_csv` function to write dataframes to files.
> Applying what you've learned about reading from files,
> write one of your dataframes to a file called `processed.csv`.
> You can use `help` to get information on how to use `to_csv`.
> > ## Solution
> > In order to write the DataFrame `data` to a file called `processed.csv`, execute the following command:
> > ~~~
> > data.to_csv('processed.csv')
> > ~~~
> >{: .language-python}
> > For help on `to_csv`, you could execute, for example:
> > ~~~
> > help(data.to_csv)
> > ~~~
> >{: .language-python}
> > Note that `help(to_csv)` throws an error! This is a subtlety and is due to the fact that `to_csv` is NOT a function in 
> > and of itself and the actual call is `data.to_csv`. 
> {: .solution}
{: .challenge}