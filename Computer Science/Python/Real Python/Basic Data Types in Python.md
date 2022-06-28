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
# Strings

# Escape Sequences

# Raw Strings

# Triple Quoted Strings

# Booleans

# Functions: Composite Data Types

# Functions: Math

# Functions: Type Conversion

# Functions: Iterables and Iterators

# Functions: Input and Output

# Functions: Variables, References, and Scope

# Functions: Miscellaneous