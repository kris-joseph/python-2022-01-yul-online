---
title: "Reading Tracebacks"
teaching: 10
exercises: 10
questions:
- "What is a traceback?"
- "How do I read traceback messages?"
objectives:
- "Understand traceback messages."
- "Read a traceback and determine the file, function, and line number on which the error occurred, the type of error, and the error message."
keypoints:
- "Tracebacks provide valuable information about the piece of code (file, function, and line number) where a runtime error is occurring."
---

> ## Reading Error Messages
>
> Read the traceback below, and identify the following:
>
> 1. How many levels does the traceback have?
> 2. What is the file name where the error occurred?
> 3. What is the function name where the error occurred?
> 4. On which line number in this function did the error occur?
> 5. What is the type of error?
> 6. What is the error message?
>
> ~~~
> ---------------------------------------------------------------------------
> KeyError                                  Traceback (most recent call last)
> <ipython-input-2-e4c4cbafeeb5> in <module>()
>       1 import errors_02
> ----> 2 errors_02.print_friday_message()
>
> /Users/ghopper/thesis/code/errors_02.py in print_friday_message()
>      13
>      14 def print_friday_message():
> ---> 15     print_message("Friday")
>
> /Users/ghopper/thesis/code/errors_02.py in print_message(day)
>       9         "sunday": "Aw, the weekend is almost over."
>      10     }
> ---> 11     print(messages[day])
>      12
>      13
>
> KeyError: 'Friday'
> ~~~
> {: .error}
> > ## Solution
> > 1. Three levels.
> > 2. `errors_02.py`
> > 3. `print_message`
> > 4. Line 11
> > 5. `KeyError`. These errors occur when we are trying to look up a key that does not exist (usually in a data
> > structure such as a dictionary). We can find more information about the `KeyError` and other built-in exceptions
> > in the [Python docs](https://docs.python.org/3/library/exceptions.html#KeyError).
> > 6. `KeyError: 'Friday'`
> {: .solution}
{: .challenge}