# HEB CDA Python Lunch and Learn, Session 2

2019-01-18

## Topic: Simple variables and flow control

### Variables

- How do we do basic math?
- How do we format strings?

### Comments
- How do you comment your code?

### Flow control

- How do we repeat something a set number of times?
- How do we repeat something until we decide not to?
- How do we decide to do one thing or another?
- Python white-spacing

## Our challenges

By the end of this lunch-and-learn, we should have created (or at least know how to create) two simple programs
- 401k calculator: How much can I expect in my 401k after a set number of years, where I contribute a set amount of money per week and it grows with interest?
- Mortgage calculator: How long will it take to pay of an interest-bearing debt if you make a periodic payment?

## Simple variables

### Numbers (integers and floating point numbers)

- Integers: Whole numbers. `5`
- Floats: Numbers with decimals. `5.0`
- Can cast from one to another with `int()` or `float()`
- Standard math operators, `+`, `-`, `*` (multiply), `/`, `**` (exponent).
- Dividing in Python 2: `5/2 = 2` floor division. If you want a float, you need to run `float(5)/2`.
- Dividing in Python 3: `5/2 = 2.5`.
- Define a variable with `=`. `x = 5` assigns 5 to the variable `x`.
- Dividing by 0 and calling variables that haven't been assigned raise an error.

### Boolean

- Values `True` and `False`.
- Comparison operators  `==` (equals), `<`, `>`, `<=`, `>=`, and `!=` (not equals).
- Non-zero numbers can act as "True" in if-statements. Zeros will act as false.
- Be very careful when testing equality with floating point numbers. Instead of `x == 0.5`, do `abs(x - 0.5) < 0.001` or something.

### Strings

- Can use single or double-quotes.
- Multi-line strings with triple quotes.
  - Very useful for embedding SQL or JSON or other type code in your Python. 
- Use of escape characters like `\n`, `\0`.
- Strings can be concatenated by being added together (`'test1 ' + 'test2' = 'test1 test2'`), and repeated by multiplying (`'test'*5 = 'testtesttesttesttest'`).
- Strings are always variable width, and single characters are also strings.

## Flow control

### White-spacing

- Blocks of code for for/if/etc are set apart by indentation.
- Example:

```python
for ind in range(3):
   print(ind)
   print('a')
print('b')

# returns:
# 0
# a
# 1
# a
# 2
# a
# b
```
- The `print(ind)` and `print('a')` get run repeatedly, the `print('b')` is outside the loop and only gets run once.
- 4 spaces is traditional, though others work.
- Don't mix tabs and spaces.

### For (with range)

- General syntax `for VARIABLE in COLLECTION:`
- `for ind in range(n):` goes from `0` to `n-1`. Notice this means you repeat your code `n` times.
- `ind` above is just a variable - you don't need to define it before, and you can access it afterwards.
  - Don't overwrite anything important!
- `range(n)` goes from 0 to`n-1`. `range(m, n)` goes from `m` to `n-1`. `range(m, n, s)` goes `m, m+s, m+2s,...,n-1`.
  - Ex: If you want to count down from 10 down to 1, do `range(10, 0, -1)`.

### While

- `while x:` repeats every time `x` is True.
- `while` is not smart. `n = 5; while n < 10: print(n)` will happily run forever.
  - Pressing `Control-C` is how you break out. 

### If

- `if x:` runs code if `x` is true.
- `if x: ___ else: ___` if `x` is false, run the code in the else block instead.

## Comments

- `#` characters define comments. You need one per line.
- You can also use multiline strings that don't do anything as comments.
  - This is used to define help information for code.

## Challenge 1: 401k Calculator

Let's assume that we put a set amount of money weekly into the HEB 401k, and we've found a fund that has a guaranteed 4% return, compounding weekly. After a given number of years, how much money will we have?

To simplify this problem, we're going to do two things:
1. We will try solving this problem in Excel.
  ** This will not only help us see the structure of our program, but we can use this as a way to test our output. The results we get from Python should match what we get here.
3. I'm going to give you the program, but the lines will be out of order (and there will be no white-spacing to help). Your task is to rearrange the lines until they give a correct program.

```python
weeks_saving = 52*years_saving
print("After", years_saving, "years you will have saved $", savings)
weekly_principal = 200
savings = 0
rate_of_return = 0.04
savings = savings + weekly_principal
for week in range(weeks_saving):
years_saving = 20
savings = savings * (1 + rate_of_return/52)
```

Final check: If you have interest compound after the principal payment, after 20 years you should have saved $318707.69.

## Challenge 2: Mortgage calculator

Now, instead of investing, we're paying off a debt instead.

What is different between paying off a debt and investing?
- What flow-control functions will we need?
- What happens if debt payments are less than interest? What would happen with the program?
- Are there additional complications that we're dealing with a fixed amount of money?

Let's try prototyping something in Excel.

Now, to do this in Python, let's fill in the blanks.

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

N.B. This program has a minor bug in it - for the last payment, we (almost always) will slightly overpay. Your "homework" assignment will be to fix the program so that once we pay off the debt, we adjust the last `total_paid` value to reflect the partial payment.

Final check: It should take 478 weeks to pay off the mortgage with the values above, with a total payment of $238890.88.
