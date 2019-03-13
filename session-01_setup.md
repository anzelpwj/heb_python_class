# HEB CDA Python Lunch and Learn, Session 1

2019-01-11

## Introductions

I'm going to try and use HackMD for taking notes together. Please leave your name below to let me know you were able to log in to this and can edit this document. Also let me know what you'd most like to get out of this lunch-and-learn.

__Responses clipped__

## Challenge
 At the end of this session everyone should be able to pull up the Python REPL, iPython, Jupyter, and Spyder and successfully run some code ("Hello World").

## Preliminaries

There are a few variants of Python available (and if you're on Mac or Linux, you will have a version of Python already installed) but we're going to work with Anaconda Python, because:
- It comes with a number of scientific and data processing packages already installed.
- It handles installing certain numerical-work heavy packages better.
- We're using Anaconda Python already in our Big Data pipeline, so if you start doing more DS stuff for HEB you'll be ready to go.

Before we get started, please make sure to install Aanconda Python on your computer. To do so, do the following.
1. Go to [LARS](https://lars.heb.com/Login.aspx) and request administrative rights for your computer.
2. Once this has been granted, go to Anaconda.com and click on Download.
3. You will want to download the Graphical Installer for your OS (which is probably 64-bit), Python 3.7 (not 2.7).
5. Once the installer is downloaded (and you've been granted admin right) open it up and start the installation.
6. Go with all the defaults for installation, __except__ when it asks you if you want to add Ananconda to your system path, say yes (this might just be Windows specific). You also don't need to install VS Code if you don't want (it's a free IDE by Microsoft and I hear it's decent, but I'm not experienced with it).
*  If you accidentally click no, let me know and we can fix this, it'll just take a little effort.
7. Once the installation is complete, verify if Anaconda Navigator is available on your computer.

![](https://i.imgur.com/fp5eA5H.png)

7. Open up Navigator and see if you get this window.

![](https://i.imgur.com/wVNNoOf.png)

If you're at this point, you're done! If something isn't working, please let me know before Friday's meeting.

## L&L notes

__Confluence page link removed__

I'm going to select a couple of y'all every session to fill in notes (starting with "Starting to do stuff with Python").

Please feel free to interrupt me if you feel like I'm not explaining something well.

Two colors of sticky notes:
- Yellow: I need help with something.
- Blue: I'm finished with things, and am ready to move on.

## Hello World

Pretty much every programming tutorial starts with the Hello World exercise, where your goal is to run a program that prints "Hello World" (or any other text of your choosing) out. For some programs this can be a [surprisingly complex process](https://www.johndcook.com/blog/2014/11/12/hello-world-is-the-hard-part/) (the point of the Hello World exercise is to make sure you can compile and run code, which isn't necessarily trivial) but thankfully for Python, this is an easy single line of code.

```python
print("Hello HEB")
```

N.B. in (older) Python 2 you would instead type:

```python
print "Hello HEB"
```

## Starting to do stuff with Python

### Python on terminal

- How to open terminal (Mac and Windows)
    - PC
        - Use anaconda prompt for PCs
        - Type cmd and open anaconda prompt
    - Mac
        - Just open terminal and type `python`
- Opening the REPL
    - Read, evaluate, print loop in terminal
    - Can do simple math operators, or test simple pieces of code in the command line
    - Difficult to excute multiple lines of code
    - To get out of REPL, type exit()


### Running code ("Hello world")
- No difference between ' and "
- Will get an error with print('can't') because it reads the single quote as the end of the string, so should use print("can't") and it will work
- Also can use escape characters: 
    - `print('You can\'t always get what you want')`
    
### Text editors
- Open Anaconda Navigator
    - qtconsole (aka ipython):
        - Better than REPL - provides some help and color coding
        - Includes ipy magics
            - %reset resets entire workspace (variables, etc)
            - %timeit - logs the time it takes to execute a command (or set of commands)
            - ls, cd, and other unix commands that don't work in REPL

### iPython and Jupyter notebook

- Code, figures and text.
- [Example](https://nbviewer.jupyter.org/github/anzelpwj/Stats-week/blob/master/Fourier.ipynb)
- Launch juptyer - will open terminal window as well as browser (ignore terminal window but don't close it)
- Open a new notebook using the New button in the upper righthand corner
- Each cell will execute with shift-enter
- If you don't know what a command does, enter it with a ? at the end and you'll get a help document
    - `np.arange?`
- Jupyter also accepts markdown
    - Uses same syntax as LaTeX for equations
- Need to be careful to execute the variable assignment you want before you execute a cell that consumes them
    - Best practice is to use "Reset and Clear Output" under Kernel (across top bar of notebook)
    - Kernel -> Interrupt will break something that hangs
        - In terminal window use ctrl-C to break


### IDEs

- Spyder
    - Most familiar to Matlab users
- Rodeo
- PyCharm
- All IDEs have their own idiosyncracies, but can enable you to work much faster
    - Typically NOT free
    - Include code highlighting, error alerts, style guide conformity, and better debugging
        - Set breakpoint allows you to run everything up to that point and investigate current state of variables

### How I do my work

- Jupyter + Text editor
    - Sublime, Atom, etc.
    - Do not use MS Word!!
- Test in Jupyter
- Write in text editor
- run from command line `python yourfile.py`

## Where to get help

- Python docs
- `help()` and `dir()`
- StackOverflow and The Goog

## I'm very impatient and want to start learning Python on my own

5 points for you! Here are my top recommendations of resources for learning Python.
- [Quantitative Economics](https://lectures.quantecon.org/py/): An online textbook focused on teaching economics students programming. Probably my top recommendation for now. You can read through the first few chapters and skim through the actual econ stuff - or maybe that's highly relevant to your work!
- [Scipy lecture notes](https://www.scipy-lectures.org/): Kind of similar to Quant Econ. I like QE a bit more, but this does cover some stuff (like basic machine learning) that QE doesn't.
- [Automate the Boring Stuff with Python](https://automatetheboringstuff.com/): Book is free online, or you can buy a copy (or look at the copy at my desk). A bit simpler, but the latter part of the book covers some essential working topics (regular expressions, webscraping, moving files around) that the others don't. I also like the author's approach of teaching you the minimum Python you need to start doing useful things.
- [Learn Python the Hard Way](https://learncodethehardway.org/python/): A sample of the books is available free online, but you'll need to buy it otherwise. This is the book I learned Python with. Does cover in more depth some stuff the other resources lack (like more about object-oriented programming).

QuantEcon and Automate the Boring Stuff are going to be my primary resources for these sessions.
