---
title: "Lists"
teaching: 10
exercises: 10
questions:
- "How can I store multiple values?"
objectives:
- "Explain why programs need collections of values."
- "Write programs that create flat lists, index them, slice them, and modify them through assignment and method calls."
keypoints:
- "A list stores many values in a single structure."
- "Use an item's index to fetch it from a list."
- "Lists' values can be replaced by assigning to them."
- "Appending items to a list lengthens it."
- "Use `del` to remove items from a list entirely."
- "The empty list contains no values."
- "Lists may contain values of different types."
- "Character strings can be indexed like lists."
- "Character strings are immutable."
- "Indexing beyond the end of the collection is an error."
---
## A list stores many values in a single structure.

*   Doing calculations with a hundred variables called `bmi_001`, `bmi_002`, etc.,
    would be at least as slow as doing them by hand.
*   Use a *list* to store many values together.
    *   Contained within square brackets `[...]`.
    *   Values separated by commas `,`.
*   Use `len` to find out how many values are in a list.

~~~
bmi_values = [27.9, 33.77, 33, 22.705, 28.88]
print('BMI values:', bmi_values)
print('length:', len(bmi_values))
~~~
{: .language-python}
~~~
BMI values: [27.9, 33.77, 33, 22.705, 28.88]
length: 5
~~~
{: .output}

## Use an item's index to fetch it from a list.

*   Just like strings.

~~~
print('zeroth item of BMI values:', bmi_values[0])
print('fourth item of BMI values:', bmi_values[4])
~~~
{: .language-python}
~~~
zeroth item of BMI values: 27.9
fourth item of BMI values: 28.88
~~~
{: .output}

## Lists' values can be replaced by assigning to them.

*   Use an index expression on the left of assignment to replace a value.

~~~
bmi_values[0] = 30.8
print('bmi_values is now:', bmi_values)
~~~
{: .language-python}
~~~
bmi_values is now: [30.8, 33.77, 33, 22.705, 28.88]
~~~
{: .output}

## Appending items to a list lengthens it.

*   Use `list_name.append` to add items to the end of a list.

~~~
ages = [6, 1, 3]
print('ages is initially:', ages)
ages.append(2)
print('ages has become:', ages)
~~~
{: .language-python}
~~~
ages is initially: [6, 1, 3]
ages has become: [6, 1, 3, 2]
~~~
{: .output}

*   `append` is a *method* of lists.
    *   Like a function, but tied to a particular object.
*   Use `object_name.method_name` to call methods.
    *   Deliberately resembles the way we refer to things in a library.
*   We will meet other methods of lists as we go along.
    *   Use `help(list)` for a preview.
*   `extend` is similar to `append`, but it allows you to combine two lists.  For example:

~~~
teen_ages = [19, 18, 14, 17]
middle_ages = [46, 56, 37, 41]
print('ages is currently:', ages)
ages.extend(teen_ages)
print('ages has now become:', ages)
ages.append(middle_ages)
print('ages has finally become:', ages)
~~~
{: .language-python}
~~~
ages is currently: [6, 1, 3, 2]
ages has now become: [6, 1, 3, 2, 19, 18, 14, 17]
ages has finally become: [6, 1, 3, 2, 19, 18, 14, 17, [46, 56, 37, 41]]
~~~
{: .output}

Note that while `extend` maintains the "flat" structure of the list, appending a list to a list makes the result
two-dimensional - the last element in `primes` is a list, not an integer.

## Use `del` to remove items from a list entirely.

*   We use `del list_name[index]` to remove an element from a list (in the example, 9 is not a prime number) and thus shorten it.
*   `del` is not a function or a method, but a statement in the language.

~~~
ages = [6, 1, 3, 2, 8]
print('ages before removing last item:', ages)
del ages[4]
print('ages after removing last item:', ages)
~~~
{: .language-python}
~~~
ages before removing last item: [6, 1, 3, 2, 8]
ages after removing last item: [6, 1, 3, 2]
~~~
{: .output}

## The empty list contains no values.

*   Use `[]` on its own to represent a list that doesn't contain any values.
    *   "The zero of lists."
*   Helpful as a starting point for collecting values
        (which we will see in the [next episode]({{ page.root }}/07-for-loops/)).

## Lists may contain values of different types.

*   A single list may contain numbers, strings, and anything else.

~~~
goals = [1, 'Create lists.', 2, 'Extract items from lists.', 3, 'Modify lists.']
~~~
{: .language-python}

## Character strings can be indexed like lists.

*   Get single characters from a character string using indexes in square brackets.

~~~
region = 'northwest'
print('zeroth character:', northwest[0])
print('third character:', northwest[3])
~~~
{: .language-python}
~~~
zeroth character: n
third character: t
~~~
{: .output}

## Character strings are immutable.

*   Cannot change the characters in a string after it has been created.
    *   *Immutable*: can't be changed after creation.
    *   In contrast, lists are *mutable*: they can be modified in place.
*   Python considers the string to be a single value with parts,
    not a collection of values.

~~~
region[0] = 'N'
~~~
{: .language-python}
~~~
TypeError: 'str' object does not support item assignment
~~~
{: .error}

*   Lists and character strings are both *collections*.

## Indexing beyond the end of the collection is an error.

*   Python reports an `IndexError` if we attempt to access a value that doesn't exist.
    *   This is a kind of [runtime error]({{ page.root }}/05-built-in/#runtime-error).
    *   Cannot be detected as the code is parsed
        because the index might be calculated based on data.

~~~
print('99th element of region is:', region[99])
~~~
{: .language-python}
~~~
IndexError: string index out of range
~~~
{: .output}

> ## Fill in the Blanks
>
> Fill in the blanks so that the program below produces the output shown.
>
> ~~~
> values = ____
> values.____(1)
> values.____(3)
> values.____(5)
> print('first time:', values)
> values = values[____]
> print('second time:', values)
> ~~~
> {: .language-python}
>
> ~~~
> first time: [1, 3, 5]
> second time: [3, 5]
> ~~~
> {: .output}
>
> > ## Solution
> > ~~~
> > values = []
> > values.append(1)
> > values.append(3)
> > values.append(5)
> > print('first time:', values)
> > values = values[1:]
> > print('second time:', values)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

> ## How Large is a Slice?
>
> If 'low' and 'high' are both non-negative integers,
> how long is the list `values[low:high]`?
>
> > ## Solution
> > The list `values[low:high]` has `high - low` elements.  For example,
> > `values[1:4]` has the 3 elements `values[1]`, `values[2]`, and `values[3]`.
> > Note that the expression will only work if `high` is less than the total
> > length of the list `values`.
> {: .solution}
{: .challenge}

> ## From Strings to Lists and Back
>
> Given this:
>
> ~~~
> print('string to list:', list('tin'))
> print('list to string:', ''.join(['g', 'o', 'l', 'd']))
> ~~~
> {: .language-python}
> ~~~
> string to list: ['t', 'i', 'n']
> list to string: gold
> ~~~
> {: .output}
>
> 1.  What does `list('some string')` do?
> 2.  What does `'-'.join(['x', 'y', 'z'])` generate?
>
> > ## Solution
> > 1. [`list('some string')`](https://docs.python.org/3/library/stdtypes.html#list) converts a string into a list containing all of its characters.
> > 2. [`join`](https://docs.python.org/3/library/stdtypes.html#str.join) returns a string that is the _concatenation_
> >    of each string element in the list and adds the separator between each element in the list. This results in
> >    `x-y-z`. The separator between the elements is the string that provides this method.
> {: .solution}
{: .challenge}

> ## Working With the End
>
> What does the following program print?
>
> ~~~
> region = 'southeast'
> print(region[-1])
> ~~~
> {: .language-python}
>
> 1.  How does Python interpret a negative index?
> 2.  If a list or string has N elements,
>     what is the most negative index that can safely be used with it,
>     and what location does that index represent?
> 3.  If `values` is a list, what does `del values[-1]` do?
> 4.  How can you display all elements but the last one without changing `values`?
>     (Hint: you will need to combine slicing and negative indexing.)
>
> > ## Solution
> > The program prints `t`.
> > 1. Python interprets a negative index as starting from the end (as opposed to
> >    starting from the beginning).  The last element is `-1`.
> > 2. The last index that can safely be used with a list of N elements is element
> >    `-N`, which represents the first element.
> > 3. `del values[-1]` removes the last element from the list.
> > 4. `values[:-1]`
> {: .solution}
{: .challenge}

> ## Stepping Through a List
>
> What does the following program print?
>
> ~~~
> region = 'southwest'
> print(region[::2])
> print(region[::-1])
> ~~~
> {: .language-python}
>
> 1.  If we write a slice as `low:high:stride`, what does `stride` do?
> 2.  What expression would select all of the even-numbered items from a collection?
>
> > ## Solution
> > The program prints
> > ~~~
> > suhet
> > tsewhtuos
> > ~~~
> > {: .language-python}
> > 1. `stride` is the step size of the slice.
> > 2. The slice `::2` selects all even-numbered items from a collection: it starts
> >    with element `0` (which is the first element, since indexing starts at `0`),
> >    goes on until the end (since no `end` is given), and uses a step size of `2`
> >    (i.e., selects every second element).
> {: .solution}
{: .challenge}

> ## Slice Bounds
>
> What does the following program print?
>
> ~~~
> product = 'insurance'
> print(product[0:20])
> print(product[-1:3])
> ~~~
> {: .language-python}
>
> > ## Solution
> > ~~~
> > insurance
> > 
> > ~~~
> > {: .output}
> The first statement prints the whole string, since the slice goes beyond the total length of the string.
> The second statement returns an empty string, because the slice goes "out of bounds" of the string.
> {: .solution}
{: .challenge}

> ## Sort and Sorted
>
> What do these two programs print?
> In simple terms, explain the difference between `sorted(letters)` and `letters.sort()`.
>
> ~~~
> # Program A
> letters = list('gold')
> result = sorted(letters)
> print('letters is', letters, 'and result is', result)
> ~~~
> {: .language-python}
>
> ~~~
> # Program B
> letters = list('gold')
> result = letters.sort()
> print('letters is', letters, 'and result is', result)
> ~~~
> {: .language-python}
>
> > ## Solution
> > Program A prints
> > ~~~
> > letters is ['g', 'o', 'l', 'd'] and result is ['d', 'g', 'l', 'o']
> > ~~~
> > {: .output}
> > Program B prints
> > ~~~
> > letters is ['d', 'g', 'l', 'o'] and result is None
> > ~~~
> > {: .output}
> > `sorted(letters)` returns a sorted copy of the list `letters` (the original
> > list `letters` remains unchanged), while `letters.sort()` sorts the list
> > `letters` in-place and does not return anything.
> {: .solution}
{: .challenge}

> ## Copying (or Not)
>
> What do these two programs print?
> In simple terms, explain the difference between `new = old` and `new = old[:]`.
>
> ~~~
> # Program A
> old = list('gold')
> new = old      # simple assignment
> new[0] = 'D'
> print('new is', new, 'and old is', old)
> ~~~
> {: .language-python}
>
> ~~~
> # Program B
> old = list('gold')
> new = old[:]   # assigning a slice
> new[0] = 'D'
> print('new is', new, 'and old is', old)
> ~~~
> {: .language-python}
>
> > ## Solution
> > Program A prints
> > ~~~
> > new is ['D', 'o', 'l', 'd'] and old is ['D', 'o', 'l', 'd']
> > ~~~
> > {: .output}
> > Program B prints
> > ~~~
> > new is ['D', 'o', 'l', 'd'] and old is ['g', 'o', 'l', 'd']
> > ~~~
> > {: .output}
> > `new = old` makes `new` a reference to the list `old`; `new` and `old` point
> > towards the same object.
> > 
> > `new = old[:]` however creates a new list object `new` containing all elements
> > from the list `old`; `new` and `old` are different objects.
> {: .solution}
{: .challenge}