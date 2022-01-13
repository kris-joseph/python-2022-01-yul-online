---
title: "Processing data from a file"
teaching: 45
exercises: 25
questions:
- "How can I read and write files?"
- "What kind of data files can I read?"

objectives:
- "Describe a file handle"
- "Use `with open() as` to open files for reading and auto-close files"
- "Create and open for writing or appending and auto-close files"
- "Explain what is meant by a record"

keypoints:
- "Reading data from files is far more common than program 'input' requests or hard coding values"
- "Python provides simple means of reading from a text file and writing to a text file"
- "Tabular data is commonly recorded in a 'csv' file"
- "Text files like csv files can be thought of as being a list of strings. Each string is a complete record"
- "You can read and write a file one record at a time"
- "Python has builtin functions to parse (split up) records into individual tokens"
---

## Reading and Writing datasets

In all of our examples so far, we have directly allocated values to variables in the code we have written before using the variables.

Python has an `input()` function which will ask for input from the user, but for any large amounts of data this will be an  impractical way of collecting data.

The reality is that most of the data that your program uses will be read from a file. Additionally, apart from when you are developing, most of your program output will be written to a file.

In this episode we will look at how to read and write files of data in Python.

There are in fact many different approaches to reading data files and which one you choose will depend on such things as the size of the file and the data format of the file.

In this episode we will;

* We will read a file which is in .csv (Comma Separated Values) format.
* We will use standard core Python functions to do this
* We will read the file one line at a time ( line = record = row of a table)
* We will perform simple processing of the file data and print the output
* We will split the file into smaller files based on some processing

**PLEASE NOTE:** Some of the file manipulation we will do in this section uses Python operations we haven't learned yet. We will explain what those sections of code are doing, but do not worry if it doesn't make sense yet. We'll be covering loops and conditionals in-depth our next session.

The file we will be using is only a small file (1338 data records), but the approach we are using will work for any size of file. Imagine 131M records. This is because we only process one record at a time so the memory requirements of the programs will be very small. The larger the file the more the processing time required.

Other approaches to reading files will typically expect to read the whole file in one go. This can be efficient when you subsequently process the data but it require large amounts of memory to hold the entire file. We will look at this approach later when in the course when we look at the Pandas package.

For our examples in this episode we are going to use the insurance.csv file. This is available for download [here](../data/insurance.csv), and the data is taken from [this dataset on Kaggle](https://www.kaggle.com/mirichoi0218/insurance), and a description of the dataset can be found on that page.

The code assumes that the file is in the same directory as your notebook.

We will build up our programs in simple steps.

### Step 1 - Open the file , read through it and close the file

~~~
with open("insurance.csv") as f:       # Open the file and assign it to a new variable which we call 'f'.
                                          # The file will be read-only by default.
                                          # As long as the following code is indented, the file 'f' will be open.
    for line in f:                        # We use a for loop to iterate through the file one line at a time.
        print(line)                       # We simply print the line.
        
print("I'm on to something else now.")    # When we are finished with this file, we stop indenting the code and the file is closed automatically.
~~~
{: .language-python}

~~~
age,sex,bmi,children,smoker,region,charges
19,female,27.9,0,yes,southwest,16884.924
18,male,33.77,1,no,southeast,1725.5523
28,male,33,3,no,southeast,4449.462
33,male,22.705,0,no,northwest,21984.47061
32,male,28.88,0,no,northwest,3866.8552
31,female,25.74,0,no,southeast,3756.6216
46,female,33.44,1,no,southeast,8240.5896
37,female,27.74,3,no,northwest,7281.5056
37,male,29.83,2,no,northeast,6406.4107
60,female,25.84,0,no,northwest,28923.13692
...
~~~
{: .output}

You can think of the file as being a list of strings. Each string in the list is one complete line from the file.

If you look at the output, you can see that the first record in the file is a header record containing column names. When we read a file in this way, the column headings have no significance, the computer sees them as another record in the file.

### Step 2 - Select a specific 'column' from the records in the file

We know that the first record in the file is a header record and we want to ignore it. To do this we call the `readline()` method of the file handle f. We don't need to assign the line that it will return to a variable as we are not going to use it.

As we read the file the line variable is a string containing a complete record. The fields or columns of the record are separated by each other by "," as it is a csv file.

As line is a string we can use the `split()` method to convert it to a list of column values. We are specifically going to select the column which is the 4th entry in the list (remember the list index starts at 0). This refers to the `children` column. We are going to examine the data about the number of children associated with each person in the file.

~~~
with open ("SAFI_results.csv") as f:  # Open the file and assign it to a variable called 'f'.
                                      # Indent the code to keep the file open. Stop indenting to close.
    f.readline()                      # First line is a header so ignore it.

    for line in f:
        print(line.split(",")[3])    # Index 18, the 19th column is C01_respondent_roof_type.

~~~
{: .language-python}

~~~
0
1
3
0
0
0
1
3
...
~~~
{: .output}

Having a list of the number of children from all of the records is one thing, but it is more likely that we would want a histogram or tally of this data. By scanning up and down the previous output, there appears to be a maximum of 5 children for any one person, but we will play safe and assume there may be more.

### Step 3 - Use Pythong to tally the records for each posible number of children

~~~
# 1
with open ("insurance.csv") as f:

# 2
    f.readline()

# 3
    childCountTally_0 = 0
    childCountTally_1 = 0
    childCountTally_2 = 0
    childCountTally_3 = 0
    childCountTally_4 = 0
    childCountTally_5 = 0

    for line in f:
# 4
        childCount = line.split(",")[3]
# 5    
        if childCount == 0 :
            childCountTally_0 += 1
        elif childCount == 1 :
            childCountTally_1 += 1
        elif childCount == 2 :
            childCountTally_2 += 1
        elif childCount == 3 :
            childCountTally_3 += 1
        elif childCount == 4 :
            childCountTally_4 += 1
        elif childCount == 5 :
            childCountTally_5 += 1

#6
print("There are ", childCountTally_0, " people with 0 children")
print("There are ", childCountTally_1, " people with 1 child")
print("There are ", childCountTally_2, " people with 2 children")
print("There are ", childCountTally_3, " people with 3 children")
print("There are ", childCountTally_4, " people with 4 children")
print("There are ", childCountTally_5, " people with 5 children")
~~~
{: .language-python}

~~~
There are 574 people with 0 children
There are 324 people with 1 child
There are 240 people with 2 children
There are 157 people with 3 children
There are 25 people with 4 children
There are 18 people with 5 children
~~~
{: .output}

What are we doing here?

1. Open the file
2. Ignore the headerline
3. Initialise child tally variables to 0
4. Extract the `children` information from each record
5. Increment the appropriate variable
6. Print out the results (we have stopped indenting so the file will be closed)

Instead of printing out the counts of each number of children, you may want to extract the records for people with a certain number of children to a separate file. Let us assume we want all of the records for people with three children to be written to a file.

~~~
# 1
with open ("insurance.csv") as fr:              # Note how we have used a new variable name, 'fr'.
                                                   # The file is read-only by default.

    with open ("insurance_threeChildren.csv", "w") as fw:  # We are keeping 'fr' open so we indent.
                                                   # We specify a second parameter, "w" to make this file writeable.
                                                   # We use a different variable, 'fw'.
        for line in fr:
# 2    
            if line.split(",")[3] == 3 :
                fw.write(line)

~~~
{: .language-python}

What are we  doing here?

1. Open the files. Because there are now two files, each has its own file handle: `fr` for the file we read and `fw` for the file we are going to write. (They are just variable names so you can use anything you like). For the file we are going to write to we use `w` for the second parameter. If the file does not exist it will be created. If it does exist, then the contents will be overwritten. If we want to append to an existing file we can use `a` as the second parameter.
2. Because we are just testing a specific field from the record to have a certain value, we don't need to put it into a variable first. If the expression is True, then we use `write()` method to write the complete line just as we read it to the output file.

In this example we didn't bother skipping the header line as it would fail the test in the if statement. If we did want to include it we could have added the line

~~~
fw.write(fr.readline())
~~~
{: .language-python}

before the for loop

> ## Exercise
>
> From the insurance.csv file extract all of the records where the `smoker` field (index 4) has a value of `'no'` and the `region` (index 5) has a value of `'southeast'` and write them to a file. Within the same program write all of the records where `smoker` (index 4) has a value of `'no'` and the `region` (index 19) has a value of `'northwest'` and write them to a separate file. In both files include the header record.
>
> > ## Solution
> >
> > ~~~
> > with open ("insurance.csv") as fr:
> >
> >    with open ("insurance_nonsmokers_southeast.csv", "w") as fw1:
> >       with open ("insurance_nonsmokers_northwest.csv", "w") as fw2:
> >
> >           headerline = fr.readline()
> >           fw1.write(headerline)
> >           fw2.write(headerline)
> >
> >           for line in fr:
> >               if line.split(",")[4] == 'no' :
> >                   if line.split(",")[5] == 'southeast' :
> >                       fw1.write(line)
> >                   if line.split(",")[5] == 'northwest' :
> >                       fw2.write(line)    
> >
> > ~~~
> > {: .language-python}
> >
> {: .solution}
{: .challenge}


In our example of printing the tallies for the number of children, we assumed that we knew what all the possible values were. Had there been any entries higher than 5 we would still be none the wiser. We were able to decide on the possible values for the `'children'` column by manually scanning the list of values. This was only practical because of the small file size. For a multi-million record file we could not have done this.


Let's say we would like a way of creating a list of the different regions wihtout having to check the data file for all of the possible values first. We can do this by using a special Python structure called a *dictionary*.



## The Python dictionary structure

In Python a dictionary object maps keys to values. A dictionary can hold any number of keys and values but a key cannot be duplicated.

The following code shows examples of creating a dictionary object and manipulating keys and values.

~~~
# an empty dictionary
myDict = {}

# A dictionary with a single Key-value pair

personDict = {'Name' : 'Peter'}

# I can add more about 'Peter' to the dictionary

personDict['Location'] = 'Manchester'


# I can print all of the keys and values from the dictionary

print(personDict.items())

# I can print all of the keys and values from the dictionary - and make it look a bit nicer

for item in personDict:
    print(item, "=", personDict[item])

# or all of the keys

print(personDict.keys())

# or all of the values

print(personDict.values())

# I can access the value for a given key

x = personDict['Name']
print(x)

# I can change value for a given key

personDict['Name'] = "Fred"
print(personDict['Name'])

# I can check if a key exists

key = 'Name'

if key in personDict :
    print("already exists")
else :
    personDict[key] = "New value"
~~~
{: .language-python}

~~~
dict_items([('Location', 'Manchester'), ('Name', 'Peter')])
Location = Manchester
Name = Peter
dict_keys(['Location', 'Name'])
dict_values(['Manchester', 'Peter'])
Peter
Fred
already exists
~~~
{: .output}

> ## Exercise
>
> 1. Create a dictionary called `dict_childCountTallies` with initial keys of `childCount_0` and `childCount_1` and give them values of 1 and 3.
> 2. Add a third key `childCount_2` with a value of 6.
> 3. Add code to check if a key of `childCount_3` exists. If it does not, add it to the dictionary with a value of 1. If it does alredy exist, increment its value by 1
> 4. Add code to check if a key of `childCount_4` exists. If it does not, add it to the dictionary with a value of 1. if it does already exist, increment its value by 1
> 5. Print out all of the keys and values from the dictionary
>
> > ## Solution
> >
> > ~~~
> >
> > # 1
> > dict_childCountTallies = {'childCount_0' : 1 , 'childCount_1' : 3}
> >
> > # 2
> > dict_childCountTallies['childCount_2'] = 6
> >
> > # 3
> > key = 'childCount_3'
> > if key in dict_childCountTallies :
> >     dict_childCountTallies[key] += 1
> > else :
> >     dict_childCountTallies[key] = 1
> >
> > # 4
> > key = 'childCount_4'
> > if key in dict_childCountTallies :
> >     dict_childCountTallies[key] += 1
> > else :
> >     dict_childCountTallies[key] = 1
> >  
> > # 5
> > for item in dict_childCountTallies:
> >     print(item, "=", dict_childCountTallies[item])
> >     
> > ~~~
> > {: .language-python}
> >
> {: .solution}
{: .challenge}

We are now in a position to re-write our child-tallying example without knowing in advance what the possible values for `'children'` are.

~~~
# 1
with open ("insurance.csv") as f:

# 2
    f.readline()

# 3
    dict_childCountTallies = {}

    for line in f:
# 4
        childCount = line.split(",")[3]
# 5    
        if childCount in dict_childCountTallies :
            dict_childCountTallies[childCount] += 1
        else :
            dict_childCountTallies[childCount] = 1

# 6

for item in dict_childCountTallies:
    print("People with", item, "children =", dict_childCountTallies[item])
~~~
{: .language-python}

~~~
People with 0 children = 574 
People with 1 children = 324 
People with 2 children = 240 
People with 3 children = 157 
People with 4 children = 25 
People with 5 children = 18 
~~~
{: .output}

What are we doing here?

1. Open the file
2. Ignore the headerline
3. Create an empty dictionary
4. Extract the `'children'` information from each record
5. Either add to the dictionary with a value of 1 or increment the current value for the key by 1
6. Print out the contents of the dictionary (here, we stopped indenting so the file is closed)


You can apply the same approach to count values in any of the fields/columns of the file.

## Your assignment

For next class, complete the following exercise:

Write a script that opens the `'insurance.csv'` file, reads it, and creates a new file called `'insurance_male10k.csv'` that contains only the records for males whose charges are greater than $10,000.

**Hint**: Use one of the examples in this episode as a template. In the the `if` statements, how can you test the value of the entry in the `charges` column to keep only entries greater than a certain value?