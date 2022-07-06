# Overview
In this section we'll cover basic data types that are built into Python.
- You'll learn about several basic **numeric, string** and **Boolean** types that are built into Python.
- You'll see what objects of these **types** look like and how you can **represent** them.
- You'll get an overview of Python's built-in **functions**.

[[Basic Data Types in Python#Integers|Integers]]
[[Basic Data Types in Python#Floats|Floats]]
[[Basic Data Types in Python#Complex Numbers|Complex Numbers]]
[[Basic Data Types in Python#Strings|Strings]]
[[Basic Data Types in Python#Escape Sequences|Escape Sequences]]
[[Basic Data Types in Python#Raw Strings|Raw Strings]]
[[Basic Data Types in Python#Triple Quoted Strings|Triple Quoted Strings]]
[[Basic Data Types in Python#Booleans|Booleans]]
[[Basic Data Types in Python#Functions Composite Data Types|Functions: Composite Data Types]]
[[Basic Data Types in Python#Functions Math|Functions: Math]]
[[Basic Data Types in Python#Functions Type Conversion|Functions: Type Conversion]]
[[Basic Data Types in Python#Functions Iterables and Iterators|Functions: Iterables and Iterators]]
[[Basic Data Types in Python#Functions Input and Output|Functions: Input and Output]]
[[Basic Data Types in Python#Functions Variables References and Scope|Functions: Variables, References, and Scope]]
[[Basic Data Types in Python#Functions Miscellaneous|Functions: Miscellaneous]]

# Integers
- Whole numbers.
- Any length allowed up to the memory limit of the computer.
- Default is decimal (base 10), can be viewed and defined in different bases.
``` Python
a = 1
b = 10
a + b           # 11
a - b           # -1
c = 0b101010111 # 343 in binary
d = 0o454312    # 153802 in octal
e = 0xac4d      # 44109 in hexidecimal
m = '1235'      # Cannot perform math operations because it is of type str not int, will cause a TypeError
m2 = int(m)     # Example of type conversion
type(m2)        # <class 'int'>
bin(m2)         # 0b10011010011
oct(m2)         # 0o2323
hex(m2)         # 0x4d3
```

[[Basic Data Types in Python#Overview|Overview]]

# Floats
- Any number with a decimal point. Are represented differently in Python's memory than integers.
- Any division operation will return a float.
- Can be defined using scientific notation.
```Python
a = 4.2
type(a)      # <class 'float'>
b = 4.0
type(b)      # <class 'float'>
c = 10
d = 5
f = c / d    # 2.0, the value is of type float
g = c // d   # 2, the // represents floor division and will always return a value of type int
h = c // 4   # returns 2 not 2.5
i = float(3) # returns 3.0, type float
j = 4e3      # 4000.0, scientific notation 
k = 4e-3     # .004
```

- floating point errors are something to be aware of, this is something that affects all computer languages. Floating-point numbers are represented using a fixed number of binary digits and it can be difficult to represent some decimal numbers in binary so in many cases it leads to small round-off errors. 
	- Take the number 1.2 as an example, the representation of 0.2 in binary is 0.00110011001100110011... and so on. It is difficult to store this infinite decimal number internally. Normally a float object's value is stored in binary floating-point with fixed precision (typically 53 bits).
	- So we represent 1.2 internally as, 1.0011001100110011001100110011001100110011001100110011
	- Which is exactly equal to 1.1999999999999999555910790149937383830547332763671875

[[Basic Data Types in Python#Overview|Overview]]

# Complex Numbers
- Expressed in the form ***ax + bj***. 
- Consists of 2 elements, the real element, and the imaginary element.
- ***j*** is the square root of -1.   

```Python
a = 2+3j
type(a)     #<class 'complex'>
b = 3+5j
a + b       # (5+8j)
a * a       # (-5 + 12j)
```

[[Basic Data Types in Python#Overview|Overview]]

# Strings
- A sequence of zero of more characters.
- Can be enclosed using " or ' characters.
- Formats include raw and triple-quoted strings.

 ```Python
 a = "hello world"
 b = 'hello world'
 c = "I wasn't at school today."
 type(a)                         #<class 'str'>
 d = str(51)
 d                               # contains the value '51' and is of <class 'str'> 
```

[[Basic Data Types in Python#Overview|Overview]]

# Escape Sequences
```Python
a = "He said "I wasn't at school today" to me"    
# The above line is not valid syntax, below is the proper syntax
a = 'He said I wasn\'t at school today" to me'
b = 'This is line 1.\n This is line 2.'
# print(b) would return the following:
# This is line1.
# This is line2.

# \' -  Single Quote
# \" - Double Quote
# \\ - Backslash
# \n - Newline 
# \t - Tab 
# \r - Carriage Return
# \b - Backspace
# \f - Form Feed
# \v - Vertical Tab
# \onn - Character with octal value
# \xnn - Character 
```

[[Basic Data Types in Python#Overview|Overview]]

# Raw Strings
- In a raw string escape sequence characters are not interpreted

```Python
a = "One line\nTwo!"
# print(a) would result in the following:
# One Line
# Two!
b = r"One Line\nTwo!"
#print(b) would result in the follwing:
# One Line\nTwo!
``` 

[[Basic Data Types in Python#Overview|Overview]]

# Triple Quoted Strings
- Can use both double and single quotes within the triple quotes without the need to use escape sequences.
- Their main uses is to allow strings which cover multiple lines.
- The most common use for triple quoted strings is to provide doc strings for functions.

```Python
print("""This is a "triple quoted" string, isn't it?""")

print("""This strings spans across multiple lines.
	  It isn't going wrong when I do this!
	  Even if I do mulitiple empty lines...
	  
	  
	  It still works!""")


def my_function(value):
	"""Takes a value and prints it to the screen"""
	print(f"{value} is a nice value")
	return
```

[[Basic Data Types in Python#Overview|Overview]]

# Booleans
- A variable that can take two values - *True* or *False*.
- False is equivalent to 0, and True is equivalent to any non-zero number.
- Objects have "truthiness" in Python, and making use of this can make for more readable, 'Pythonic' code.

```Python
a = True
type(a)     #<class 'bool'>
b = False
c = 1
bool(c)     # True
bool(0)     # False
```

[[Basic Data Types in Python#Overview|Overview]]

# Functions: Composite Data Types
- Composite data types are made up of other data types.

```Python
a = [1, 2, 3, 4, 5]     # A list, definied using square brackets and a series of values seperated by commas
a[0]                    # 1
a[1]                    # 2
a[-1]                   # 5
a[-2]                   # 4
a[1:3]                  # [2. 3] 
a[1] = 6                # a = [1, 6, 3, 4, 5], lists are mutable


b = (1, 2, 3, 4, 5)     # A tuple, looks very similar to a list but is defined with curved brackets
b[0]                    # 1
b[-1]                   # 5
b[1:3]                  # (2, 3)
b[1] = 6                # This will result in an error: 'tuple' object does not support item assignment.
                        # Once you've created it, it can't be changed because it is immutable.


a = [1, 2, 3, 3, 3, 3, 4]
b = set(a)              # A set is a data structure which can only contain unique elements.
print(a)                # [1, 2, 3, 3, 3, 3, 4]
print(b)                # set([1, 2, 3, 4])
c = list(b)             # cast to a list 
print(c)                # [1, 2, 3, 4]


a = dict()              # Creates an empty dictionary, can be thought of as a lookup table which allows you to store values
                        # in it as key value pairs.
a['name'] = 'Darren'    # Values are added with this notation, providing they key and it's corresponding value
a['age'] = 48
a                       # {'name': 'Darren', 'age': 48}
print(a['age'])         # 48
print(a['country'])     # Results in the following error: KeyError: 'country'. This is because the key 'country' does not exist.
print(a.get('country')) # Returns the value None. This is a safer way to access values in a dictionary.
```

[[Basic Data Types in Python#Overview|Overview]]

# Functions: Math
  
# Functions: Type Conversion

# Functions: Iterables and Iterators

# Functions: Input and Output

# Functions: Variables, References, and Scope

# Functions: Miscellaneous