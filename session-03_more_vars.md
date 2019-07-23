# HEB CDA Python Lunch and Learn, Session 3

2019-01-25, 2019-02-18, and 2019-02-22

## Topic: More on variables and flow control

### Variables
- A taxonomy of variables
- More on strings
- Simple iterables (lists and tuples)
- Slicing iterables 

### Flow control
- For-each
- Else/elif
- Break
- Continue

## Our challenges

- Let's categorize our variables from our two projects from last week.
- The mortgage calculator from earlier could potentially run forever. Let's try using `for` and `break` instead.
- Our big-data workflow scheduler Oozie produces a table of all the tasks that get run and important information about them (start and end time, whether the job succeeded or failed) but the output needs a bit of processing to easily process the code. Let's clean out extraneous stuff and generate a CSV of our data.

## From before: Let's review the mortgage calculator code

```python
import sys  # Don't worry about understanding this for now
debt = -200000
weekly_payment = 500
rate_of_interest = 0.04

if ______:
    print("Weekly payment too small to pay off the debt.")
    sys.exit(1)  # This is how we stop a program with error

total_paid = 0
number_of_weeks = 0
while ______:
    debt = ______  # Make payment
    debt = ______  # Add interest
    total_paid = ______  # Update how much you paid
    number_of_weeks = ______  # Increase the number of weeks

print("It will take", number_of_weeks, "to pay off the debt.")
print("You will have paid a total of $", total_paid)
```


## Variables

### Basic taxonomy

A [nice list](http://www.cs.joensuu.fi/~saja/var_roles/role_intro.html) of how variables are often used by novices.

| Variable type | Definition | Example |
|---------------|------------|---------|
|Fixed value| Value that remains constant through the program| `interest_rate = 0.04` |
| Stepper | Something where you're going through a known succession of values | `for week in range(10):` |
| Most recent holder | Something holding the last value in a succession of unpredictable values, or last value as input | `number_of_weeks += 1` |
| Most wanted holder | Something holding the best value encountered so far | maximum value |
| Gatherer | Something accumulating the effects of individual values | debt calculation |
| Follower | Keeps track of the prior value of another variable | `x = y; y = y + 1` |
| One-way flag | Two valued data that can't get initial value once value has changed (usually goes from 0 to 1) | `errors_occurred` |
| Temporary | Something holding data for very short time | Calculate the first month's interest in mortgage |

Note that this list isn't complete, and there are a couple more types the authors demonstrate in the above link, but this is a good starting point to work from.

### Strings

- `len(string)` gives the length of a string.
- String formatting.
  - [printf style](https://en.wikipedia.org/wiki/Printf_format_string).
  - [Format strings](https://realpython.com/python-string-formatting/).
  - It doesn't really matter which one you use, but it's preferable that, if you're sticking in a lot of elements into a string, you use one of the forms that lets you define insert variable names (see Format strings link).
- [String methods](https://www.w3schools.com/python/python_ref_string.asp).
  - Splitting: `string.split(delimiter)`
  - Joining: `'delimiter'.join(iterable)`
  - Stripping: `string.strip()`

### Iterables (lists and tuples)

- A list is defined by using square brackets and commas.
  - `[1, 2, 3]` is a list of 3 elements.
- Lists can have any data type, including other lists.
  - `[53, 'nose', [1, 2, 3]]` is a perfectly valid list.
- To select the nth item from a list `x`, use `x[n]`.
  - Python is 0-indexed, so the first element is `x[0]`.
  - If you have a list inside a list, you'd call its internal elements with `x[n][m]`.
- To select a range of items, do `x[m:n]` (this goes `m, m+1, ..., n-1`).
- `x[5:]` goes from element 5 to the end. `x[:5]` goes from the beginning to element 4.
- `len(x)` tells you how many elements `x` has.
- Can use indices like -1, -2, etc to represent the last element, second to last element, etc.
- You can concatenate lists with `+`. You can repeat lists with `*`.
  - No easy vector math here, but lists are the wrong tool for that anyway.
  - `x.append(y)` adds element `y` to the end of list `x`.
- `y in x` returns True if any element of `x` equals `y`, False otherwise.
- Tuples are similar to lists, but you use `()` instead of `[]`.
  - You don't actually need parentheses. `x = 5,` is a tuple. This can be a fun source of bugs.
- Lists are mutable, you can change them (e.g. setting `x[5] = 20`, or using append). Tuples are not.
- 9 times out of 10, I use lists over tuples.
- Strings basically act like tuples of single letters.

## Flow control

### For-each
- You don't need to have a `range` function in your for loop, it just runs over everything in a list (or tuple, or other "iterable") that you give it.
- Example:
```python
competitor_stocks = ['NASDAQ:TGT', 'NASDAQ:WMT', 'NASDAQ:COST',
                     'NASDAQ:SWY', 'NASDAQ:KGR']
for stock in competitor_stocks:
    print(stock)
    # We might have some code here to get the
    # current value of stock.
```
- Sometimes it's really useful to iterate over items but also have a running number for each for-loop. You can use the `enumerate` function for this.
```python
competitor_stocks = ['NASDAQ:TGT', 'NASDAQ:WMT', 'NASDAQ:COST',
                     'NASDAQ:SWY', 'NASDAQ:KGR']
for index, stock in enumerate(competitor_stocks):
    print("Stock number %d: %s" % (index, stock))
# Prints:
# Stock number 0: NASDAQ:TGT
# Stock number 1: NASDAQ:WMT
# Stock number 2: NASDAQ:COST
# Stock number 3: NASDAQ:SWY
# Stock number 4: NASDAQ:KGR
```
- An example 

### Else/elif
- For if statements, you can use `else` and `elif` (else if) for alternate branches.
- Example:
```python
if store == 'HEB':
    print("This is us.")
elif store in ["Mi Tienda", "Joe V's", "Central Market"]:
    print("This is also us.")
else:
    print("This is a competitor.")
```
- `else` can also pop up in `for` loops, and should be called `no break`.

### Break

- Break kicks you out of any for or while loop you're in.
- Example:
```python
# Finds the first vowel in a word
for letter in word:
    if letter in 'aeiou':
        break
print("First vowel is %s" % letter)
```

### Continue
- Goes back to the beginning of your `for` or `while` loop.

```python
# Disemvowels a word (removes all vowels)
dword = ''
for letter in word:
    if letter in 'aeiou':
        continue
    dword = dword + letter
print("%s disemvoweled is %s" % (word, dword))
```

## Working on our challenges

### Categorization

### 401k:
- years_saving:
- weekly_principal:
- rate_of_return:
- weeks_saving:
- savings:
- week:

### Mortgage calculator:
- debt:
- weekly_payment:
- rate_of_interest:
- total_paid:
- number_of_weeks:

### Mortgage calculator

Code from above

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

Let's make the following adjustments:
- Replace the while loop with a `for` loop for a large number (say 100 years * 52 weeks = 5200).
- Once the debt is paid off, you can use `break` to stop the loop.
- Use an `if/else` statement at the very end to control whether we print that we paid off the debt or that we were unable to pay it off in 100 years.

### Oozie workflow processor

We will need the following commands

```python
# Open the file and return a list of text, each line giving
# one string in the list
with open('oozie_output.txt', 'r') as fp:
    text = fp.readlines()

# Split a string into a list with a given delimiter
elements = string.split("delimiter")

# Recombine a list into a single string with a given delimiter
string = "delimiter".join(elements)

# You can split and recombine things to switch out delimiters.
# Or you can call
string = string.replace("delim1", "delim2")

# Return True if string starts with a substring
string.startswith("substring")

# Write a string to a file
with open("oozie.csv", 'w') as fp:
    fp.write(string)

# Useful characters
tabchar = '\t'
newline = '\n'
```
