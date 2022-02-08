# Unit 1

## Contents

- [Data types and operators](#data-types-and-operators)
  - [Numeric types](#numeric-types)
  - [Boolean type](#boolean-type)
  - [Text type](#text-type)
  - [`None` type](#none-type)
- [Input and output](#input-and-output)
- [Command line arguments](#command-line-arguments)
- [Exercises](#exercises)



## Data types and operators

To use Python, please connect to Corso via `ssh`; on this virtual machine, all required programs have been installed.

Execute the following statements in Python's interactive mode. Try to understand the result of each statement. If you encounter any difficulties, please do not hesitate to contact your course instructor.



### Numeric types

Numeric types represent numbers.

```python
42      # the integer number fourty-two
3.1415  # a decimal number (float) that approximates pi
```

The hash sign `#` indicates a *comment*: Python will ignore all remaining characters on the line. (Thus, you may readily omit all comments when you enter n the following code snippets into the Python interpreter!).

Numbers may be manipulated via *arithmetic operators*:


```python
1 + 2    # addition
7 - 4    # subtraction
5 * 6    # multiplication
54 / 7   # division
10 % 4   # modulo – returns remainder
```

The *assignment operator* assigns a name to a value:

```python
a = 343
b = 14
```

Now, the name (also called *variable*) may be used instead of the value in any statement:

```python
a
a + b
```

Variable names may comprise an arbitrary number of letters, digits, and underscores. However, a name must not start with a digit, and certain keywords (e.g., `if` and `for`) are disallowed as names.

```python
x = 1            # ok
first_value = 1  # ok
number5 = 1      # ok
2much = 1        # syntax error – name starts with a digit
for = 1          # syntax error – for is a keyword
```

Let's calculate the circumference and area of a circle with radius 5:

```python
# do the math
pi = 3.1415792
radius = 5
circumference = 2 * radius * pi
area = (radius ** 2) * pi

# print results
circumference
area
```

We added explicit parentheses around `radius ** 2` to indicate that Python should first calculate the power and then the product. (Actually, we could have omitted those parentheses, since the precedence of exponentiation is higher than the one of multiplication.) However, parentheses are required if the default *operator precedence* should be changed.

```python
3 * 5 + 8  # equivalent to (3 * 5) + 8
3 * (5 + 8)
```

When in doubt, always use parentheses to structure your calculations! Moreover, never distract from the actual order of calculations by using spaces like

```python
3  *  5+8  # might imply that 5 and 8 are added first
```


### Boolean type

There are only two Boolean values: `True` and `False`. Boolean types typically arise when using *comparison operators*:

```python
3 > 2    # greater than
7 < 4    # less than
10 >= 8  # greater than or equal
2 <= 2   # less than or equal
5 == 5   # equality
10 != 9  # inequality
```

*Logical operators* connect Boolean values to obtain new Booleans.

```python
(2 < 3) and (15 / 4 > 1)  # conjunction, true if both operands are true
(5 != 5) or (12 >= 13)    # disjunction, true if at least one operand is true
not (5 < 7)               # negation changes true to false and vice versa
```


### Text type

A *string* is a sequence of Unicode characters enclosed in single or double quotes.

```python
str_a = "a string"
str_b = 'also a string'
str_c = "a string with 'single' quotes"
```

You may also use multiline strings, which must be enclosed in triple quotes.

```python
'''a
multi-
line
string'''
```

String *concatenation* is done via the `+` operator:

```python
str_a + str_b
```

Since strings are sequences of characters, you can access a single character within a string by *indexing*.

```python
enzyme = "adenylyl cyclase"
enzyme[0]    # first character
enzyme[3]    # fourth Zeichen
enzyme[-2]   # second character counting from the end of the string
enzyme[2:8]  # substring from position 2 (included) to position 8 (excluded)
```

Please note:
* Python uses *zero-based indexing*, i.e., the first element has index `0`.
* The last line above demonstrates *slicing* `[start index : end index]`, which yields a substring. The character at the start index is included, while the character at the end index is excluded (!).

Both start index and end index are optional:

```python
enzyme[:4]   # substring up to position 4 4 (excluded)
enzyme[9:]   # substring starting at position 9 (included)
enzyme[:]    # a copy of the whole string
enzyme[-3:]  # substring starting at position 3 counting from the end
```


### `None` type

Python also knows a `None` type, which explicitly denotes the “nothing”:

```python
nothing = None
nothing
```

The `is` operator checks whether a variable is `None` (do not use a Boolean comparison in this case):

```python
nothing is None  # preferred
nothing == None  # avoid
```


## Input and output

Use the `print()` function to print arbitrary output to the screen.

```python
print("Hello world!")
print(3 + 5)
```

By contrast, the `input()` function read values supplied by the user:

```python
value = input("Please enter any value: ")
value
```

After calling `input()`, Python reads an arbitrary number of characters until the user presses Enter.

Note that `input()` always reads a string. Thus, the following program will not work as expected:

```python
number = input("Please enter a number: ")
print(number + 10)  # a string cannot be added to an integer!
```

If we plan to use the entered value for calculations, we must *convert* it to a numeric type via `int()` oder `float()`:

```python
x = "10"  # x is the string "10"
x * 2     # error!

y = int(x)  # convert the string "10" to an integer 10
y * 2       # this works!

a = float("2.3")  # a is now the float 2.3
a / 5.6           # this works!
```

The following program asks the user to enter two numbers and then calculates their product. Store these statements in the file `product.py` and execute this file with Python (`python product.py`)!

```python
print("Calculate the product of two numbers")

a = input("First number: ")
a = float(a)

b = input("Second number: ")
b = float(b)

print("The result is", a * b)
```


## Command line arguments

A Python program may also process values supplied on the command line (similar to a shell command). To enable this functionality, we have to import the `sys` module (unit 2 explains modules in more detail). We create a Python script `arguments.py` containing the following lines:

```python
from sys import argv

print(argv)
print(argv[0])
print(argv[1:])
```

We then execute the script as follows:

```bash
python arguments.py a 12 third_argument
```

The output tells us:
- First line: `argv` is a *list* with four elements. (The list type will be treated in unit 2. For now, you only need to know that you may access the elements of a list like characters in a string: Variable name followed by the index in brackets.)
- Second line: The first element `argv[0]` contains the name of the script that was called (`'arguments.py'`).
- Third line: The remaining elements `argv[1:]` contain the command line arguments in the given order.

By using command line arguments, we may calculate the product of two numbers as shown below:

```python
from sys import argv
a = float(argv[1])
b = float(argv[2])
print("The result is", a * b)
```

We store these statements in `product2.py` and execute the program:
```bash
python product2.py 35 10
```



## Exercises

Each unit concludes with several exercises which you should solve on your own. Your solutions will be graded, and the scores will be used to obtain your final grade for the course. Next to each exercise, the maximum number of points for the correct solution is given.

Store all files that you generate for unit 1 in the folder `python1` in your home directory.



#### Exercise 1.1 (3 P)

You have successfully solved this exercise as soon as you have worked through this unit. In particular, the folder `python1` must contain the following files, which you have created in the course of this unit:
- `product.py`
- `arguments.py`
- `product2.py`



#### Exercise 1.2 (2 P)

Write a Python program that executes the following statements consecutively. Store the program in `exercise_1_2.py`.
1. Store the integer 32 in the variable `z` and the float 2.5 in the variable `a`.
2. Store the product of `z` and `a` in the variable `b`.
3. Divide `a` by 8 and store the result in `a`.
4. Tell me whether `a` is greater than `b` (here, the program should print a single Boolean value.)
5. Tell me whether the sum of `z` and 11 is unequal to 44 (again, only print a single Boolean value).



#### Exercise 1.3 (2 P)

Write a program `exercise_1_3.py` that prompts the user to enter three numbers and then displays whether the division of the first number by the second number yields a remainder that is equal to the third number.

An exemplary run of the program may yield the following output:

```
$ python exercise_1_3.py
dividend: 10
divisor: 4
remainder: 2
True
```

The user entered the three values 10, 4, and 2; subsequently, the program printed `True`.

A different run of the program may look like

```
$ python exercise_1_3.py
dividend: 835
divisor: 111
remainder: 20
False
```

It does not matter whether you program prints anything in the input lines (such as “dividend:” above). However, it is important that the last printed line contains a single Boolean value that has been calculated correctly.


#### Exercise 1.4 (3 P)

Write a program `exercise_1_4.py` that reads four command line arguments. (Below, these arguments are called `dna`, `x`, `a`, `e`.). The first argument is a string of arbitrary length which represents a DNA sequence. Arguments 2 to 4 are integers. The program should print two lines:
- The first line contains the `x`-th base from the left in `dna`.
- The second line contains the subsequence `dna` that starts two bases before the position given by `a` and ends at the position given by `e`.

Bases are numbered starting from one (thus, the first base in the sequence is actually numbered “1”).

You may check that your program works correctly by using the following exemplary calls:

```
                          x  a   e
                          ↓  ↓   ↓
$ python exercise_1_4.py AGCTATAGTAATCCAAT 2 5 9
G
CTATAGT

                               a     x      e
                               ↓     ↓      ↓
$ python exercise_1_4.py ATCTACGCGATATCGCGATAGCCGATGCTGACGACTGACTTGACG 13 7 20
T
ACGCGATATCGCGATA
```
