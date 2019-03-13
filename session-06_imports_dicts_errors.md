# HEB CDA Python Lunch and Learn, Session 6

2019-03-04, 2019-03-15

## Topic Imports, dictionaries, and error handling

### Imports
- General idea
- Import renaming
- Good practices
- Importing your own code
- `if __name__ == '__main__':`

### Dictionaries
- Defining
- Use

### Error handling
- Why we do this
- Try/except
- Raise
- Assert

## Challenges
- Make a module of 401k strategies, and import them.
- Have the Mortgage Calculator raise a ValueError if we won't pay off the mortgage.
- Parse a JSON file of a HEB flier.

## Notes

### Imports
- Imports let you take code from elsewhere and stick it in your code.
    - Put some of your code in its own file and import from there.
    - Take other useful libraries of code and use them as part of your code.
- Example - to use Python's JSON parsing module, you'll add `import json` to the top of your file.
    - You almost always want to stick imports at the top of your file.
    - To call our json code, we'll then need to call functions like `json.loads()` or `json.load()`. 
- You can selectively import bits of a library by calling something like `from LIBRARYNAME import DESIREDFUNCTION`.
    - You can do `from LIBRARYNAME import *` to import everything from the library without having to reference the name of the library with every call, but this is usually bad form.
- You can also rename imports using `as`. `import datetime as dt`.
- If you have another Python file (say, `utility.py`) in the same folder as your current file, you can import things from the other Python file using `import utility` or `from utility import ___`.
- Import utility names need to start with letters and cannot have hyphens or periods (except for the `.py` ending). Because of this, naming your Python files (or folders) in ways that don't work with this will cause trouble down the line. Don't do this.
- Every time you import a file, you run the entire script, even if you're only importing a few functions or values from the file.
- If you want to have certain parts of your library only run when you call it, but not when you import it, you can put the parts that only run during a direct call in a block of code that goes:

```python
if __name__ == "__main__":
    # Relevant code goes here...
```
I've often put code to test the functions in the module in my `__name__ == __main__` section.

### Dictionaries
- Instead of having a list where we need to remember an number of access something of interest, we can use a dictionary where we give items any sort of index (quite often using strings for names).
- Syntax:
```python
x = {'first': 5, 'second': 'testing'}
print(x['first'])
# Returns 5
print(x['second'])
# Returns testing
```
- You'll need both the curly braces and the colons to define a dictionary.
- The value before the colon are called *keys*. The values afterwards are *values*. If you're familiar with Key-Value pairs in Hadoop, this should be very familiar.
- If you go `for x in dictionary:` your for-loop iterates over the keys.
    - Similarly `x in dictionary` tests if `x` is a key, not a value. If you want to see if `x` is in the values, do `x in dictionary.values()`. 
    - If you want a list of a dictionary's keys, run `keys = dictionary.keys()`.
    - You can do `for k, v in dictionary.iteritems():` for both keys and their paired values.
- If you're familiar with parsing JSON data, they are extremely close to dictionaries & lists. In fact, Python's `json` module does this as a direct translation.
- MATLAB's (and other languages) structs would also be decent analogues.

### Error handling
- If code breaks, you'll get an *exception*, like `NameError` or `ZeroDivisionError`.
    - [List of Python's built-in exceptions](https://docs.python.org/3/library/exceptions.html).
- If you have an exception, your program automatically quits.
- To "catch" these errors without your program ending, you use `try/except` blocks.
    - If you have an error in the `try` block, you go to the `except` block.

Example:
```python
# Code to compute the harmonic mean
values = [1, 2, 5, 3, 5, 0, 2]

accumulator = 0
len_values = 0
for value in values:
    try:
        accumulator += 1/value
        len_values += 1
    except ZeroDivisionError:
        print("Found a 0 value. Ignoring.")

print("Harmonic mean = %.2f"
      % (len_values/accumulator))
```
- When should you worry about try/except blocks?
  - Your code takes a long time to run, and something breaking will cause a major problem.
  - If there is specific numerical code that you need to be precise, you can check that everything works as expected.
  - [Stupidity Driven Development](http://ivory.idyll.org/blog/2014-research-coding.html).
- You have your own errors fire off by using `raise`.
```python
# Code to compute the harmonic mean
values = [1, 2, 5, 3, 5, 0, 2]

def harmonic_mean(values):
    """Calculate harmonic mean."""
    if 0 in values:
        raise ValueError("Harmonic mean does not work with 0 values.")
    accumulator = 0
    for value in values:
        accumulator += 1/value
    return len(values)/accumulator

print("Harmonic mean = %.2f"
      % (harmonic_mean(values)))
```
- Giving a helpful error message (the bit in the parentheses for the `ValueError` above) with whatever you raise is very good practice.
- Another common way to fire of errors is to use the command `assert`, which raises an `AssertionError` if the statement given is not True. Example:
```python
assert 0 not in values, "Harmonic mean requires non-zero values."
```
- Assertion errors are very common in unit tests to validate that a function works as planned.

## Challenges

### Make a module of 401k strategies, and import them.

Take the 4 strategies from [the last session](https://hackmd.io/J6f52OcpSiaTr0VDdkg0LA), put them into their own file, and import them.

### Adjust the Mortgage Calculator to raise an error.

```python
import sys
debt = -200000
weekly_payment = 500
rate_of_interest = 0.04

if (weekly_payment <= -(debt+weekly_payment)*rate_of_interest/52):
    print("Weekly payment too small to pay off the debt.")
    sys.exit(1)

total_paid = 0
number_of_weeks = 0
while debt < 0:
    debt = debt + weekly_payment
    debt = debt * (1 + rate_of_interest/52)
    total_paid = total_paid + weekly_payment
    number_of_weeks = number_of_weeks + 1
    if debt > 0:
        total_paid = total_paid - debt

print("It will take", number_of_weeks, "to pay off the debt.")
print("You will have paid a total of $", total_paid)
```

Let's remove the `sys.exit(1)` and raise a `ValueError` instead.

### Parse a JSON file of a HEB flier

I have downloaded a JSON of data from Flipp (a website that does online fliers) for a HEB flier on Confluence. This is difficult to read as a person, but we can write a quick Python script to read the JSON, go over each item, and print some relevant information from each.

You can convert the file from JSON to a more amenable format with:

```python
import json
with open('flipp_store1.json', 'r') as fp:
    flipp = json.load(fp)
```

I'm not going to tell you what the most relevant fields of the JSONs are - but the goal is to get an idea of what each flier item is. Some fields will be null (in Python "None") for some items and highly relevant for others.

You should run a for-loop that goes over all the items from the JSON, pulls out relevant fields, and prints them (along with a caption explaining what they are).
