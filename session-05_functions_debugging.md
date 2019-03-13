# HEB CDA Python Lunch and Learn, Session 5

2019-02-25

## Topic: Functions and debugging

### Functions

- Defining a function
- DRY, KISS, and YAGNI
- Default variables
- Variable scope
- A function can be a variable

### Debugging

- Lots of `print` statements
- Lots of `logging` statements
- `%debug`
- `pdb`/`ipdb`
- Using an IDE

## Challenges

- Update the 401k calculator so that we can try different contribution strategies.
- Try debugging your calculator in different ways to get a feel for them.

## Functions

### Defining a function

```python
def function(variables):
    lines of code
    more code
    return result
```
As a more specific example
```python
def add_two(a, b):
    return a + b
```

You don't always have to `return` something - the function `print` doesn't return a value.
- More accurately, a function that doesn't return anything returns the special value `None`.

### When do you use a function?

- Ultimate goal: Simplify things for yourself and others.
  - Reduce repeated code (what do you do if you need to make a change to everything?).
  - Break code into understandable chunks.
  - Let yourself make subtle variations easily.

If all you need is a script, write a script! Once you feel like you're starting to repeat yourself, then write a function.

### DRY, KISS, and YAGNI

DRY = Don't Repeat Yourself

KISS = Keep It Stupid Simple

YAGNI = You Ain't Gonna Need It

> "I hate code and want as little of it as possible in our product." - Jack Diederich, Python Core Contributor

### Doc strings

- [Comment your f*cking code](https://blog.codefx.org/techniques/documentation/comment-your-fucking-code/).
- Triple-quoted string right at the top of a function (or python file).
- When you call `help(function)` it prints the docstring.
- Python doesn't have a well defined docstring style. There are some automatic documenters (like Sphinx) but no standard. I mostly copy how sci-kit learn writes their docstrings.

### Default variables

If you want to set something to be a default value, you just use `variable=default` in the function statement.

```python
def add_two(a, b, DEBUG=False):
    if DEBUG:
        print(a)
        print(b)
    return a + b
```

We would then call the function in a form like `add_two(1, 2, DEBUG=True)`. If you don't define the `DEBUG` value, it's assumed to be `False` in the example above.

These are also sometimes called *keyword arguments*, and they can be a helpful way to document what each thing does.
  - For example, `twitter_search('heb', False, False, 50)` would be less obviously clear than `twitter_search('heb', retweets=False, unicode=False, min_followers=50)`.

### Variable scope

Here's some buggy code:

```python
variable_1 = 'test'

def function_1():
    variable_2 = 'another test'
    function_2()
    print(variable_3)

def function_2():
    print(variable_1)
    print(variable_2)
    variable_3 = 'a third test'

function_1()
```
we can see `variable_1` but not `variable_2` - This is the joy of *variable scope* (which variables we can see inside of functions).
- Variables defined not inside a function are visible to everything.
- Variables defined inside a function are only accessible to the function.
  - This includes any functions called inside a function - they won't see variables from their parent function.

How to get around this: Only define variables in two places - outside of any functions or inside the function that will need them.

### A function can be a variable

You can pass in functions as variables to other functions. For example, let's say we were trying to get stock prices. We could get the prices from Quandl or IEX.

```python
def get_stock_quandl(stock):
    #code goes here

def get_stock_iex(stock):
    #code goes here

def get_stock_price(stock, provider=get_stock_quandl):
    return provider(stock)
```

## Debugging

If something breaks (we get an error), or an answer seems wrong (e.g. we computed our results in Excel and know what they "should" be) there's a few ways to investigate what happened.

### Lots of `print` statements scattered everywhere

- Pretty common, quick approach.
- HOWEVER, if you're doing this a lot, then when it comes time to remove the debug print statements, you may have difficulty figuring out which print statements are for debugging and which are for the regular program.
  - So don't do this unless it's a *very* quick check. 

### Lots of `logging` statements

- Like above, but better.
- We will discuss this in a later class, but you can use the logging code module as below

```python
import logging

# Set to one of logging.DEBUG, logging.INFO
# logging.WARN, logging.ERROR, or logging.CRITICAL
logging.basicConfig(level=logging.ERROR)

logging.debug("A debug message")
logging.info("An info message")
logging.warning("A warning message")
logging.error("An error message")
logging.critical("A critical error message")
```

- You can easily raise or lower the verbosity of your logging with setLevel.
- You can change how all your logs appears (e.g. include a timestamp). We'll see a bit more of this in a later lesson.

N.B. This apparently has some issues in Jupyter notebooks. This does work in Python scripts, however.

### Using `pdb`/`ipdb`

- You can type `import pdb; pdb.set_trace()` in your program, and when you get to that line of code execution will stop and you can go through your code line-by-line.
  - Commands are [here](https://docs.python.org/3/library/pdb.html).
  - If you type the names of variables, you can get their value.
    - Because of this, don't give your variables single letter names. 
  - You can also run bits of code while you're stopped, though that can make things confusing. 
- There is a similar thing called `ipdb`, that gives you some iPython commands as well as acting like normal pdb. You'll need to install it though (we'll do that in a later lesson).

### `%debug`

- If you're coding in iPython/Jupyter/Spyder, and your code has an error and stops, you can type `%debug` in a prompt and enter your program right before it broke to inspect things.

### Using an IDE

- IDEs like Spyder, PyCharm, etc let you click to add breakpoints in the code.
- Having things like the variable explorer makes it a lot easier to see what's happening.

## Challenges

Our old 401k code is as follows:

```python
years_saving = 20
weekly_principal = 200
rate_of_return = 0.04

weeks_saving = 52*years_saving
savings = 0
for week in range(weeks_saving):
    savings = savings + weekly_principal
    savings = savings * (1 + rate_of_return/52)

print("After %d years you will have saved $%2f."
      % (years_saving, savings))
```

Rewrite the code so that you call the inner set of lines as a function, and so we can try 4 different savings strategies.
1. Give a constant weekly principal.
2. For the first half of the years, give 150% of the weekly principal. Then give nothing.
3. For the first half of the years, give nothing. Then give 200% of the weekly principal.
4. Start by giving $10, and every week add another fifty cents to your weekly payment.

Your code should look something like

```python
years_saving = 20
weekly_principal = 200
rate_of_return = 0.04


def strategy1(years_saving, weekly_principal, week):
    # FILL IN BLANK


# Repeat for strategy2, 3, and 4.


def calc401k(years_saving, weekly_principal,
             rate_of_return, strategy=strategy1):
    # FILL IN BLANK


strategies = [strategy1, strategy2, strategy3, strategy4]

for idx, strat in enumerate(strategies):
    savings = calc401k(years_saving, weekly_principal,
                       rate_of_return, strategy=strat)
    print("Using strategy %d, final savings: $%.2f"
          % (idx + 1, savings))
```

Final output:
```
Using strategy 1, final savings: $318707.69
Using strategy 2, final savings: $286191.88
Using strategy 3, final savings: $255826.21
Using strategy 4, final savings: $375215.37
```
