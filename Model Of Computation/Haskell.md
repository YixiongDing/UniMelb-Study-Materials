# A Note For Haskell

Summary is wriiten by Yixiong Ding  
The University of Melbourne  
Augest, 2018  
_ _ _

## 1. Functions and Recursion
_ _ _
### 1.1 Hello, Haskell!

Haskell is a functional programming language. It features a strong type system, and it is compiled rather than interpreted.

Our first Haskell program:
```
hello = "Hello, World!"
```
### 1.2 Functional programming
#### 1.2.1 Functional programming

We mentioned before that Haskell is a functional programming language. Functions in Haskell are different from functions in procedural languages like C or object-oriented languages like Java.

1. Functions in Haskell are **pure**; they contain no state and have no side-effects. This way functions form self-contained units with all connections to the outside world made explicit; the functions are transparent building blocks.

2. Functions in Haskell are defined using expressions, rather than as a sequence of steps. This makes them less akin to the functions we know from from other programming languages, and more like functions from mathematics, like this one: f: Z -> Z, f(x)= x^2. This also means there is no native concept of sequence, branching, or loops.

#### 1.2.2 Defining a function

Let's take a closer look at some mathematical functions, and see how we could define them in Haskell.

For example, the mathematical function:

f: Z -> Z, f(x)= x^2.

It describes a function **f** that takes an integer (a number in **Z**) as input, then squares this number and returns the result as its output.

In Haskell, this function would be defined by the code:
```
f :: Int -> Int
f x = x^2
```
To compute **f(3)** we would write **f 3**. 

*Note*: Unlike Maths, Haskell doesn't use parentheses to separate functions from their arguments. In both function definition and use, we use spaces instead: it's **f 3** rather than **f(3)**.

Remember, Haskell uses functions for everything—it would be messy and annoying if we needed to use parentheses all the time! Instead, we save them for when we want to control the order of operations.

#### 1.2.3 Multiple arguments

Functions can of course have multiple arguments. For example:

g: Z^2 -> Z, g(x,y) = xy

This function takes a pair of integers, multiplies them together, and returns this product. In Haskell:
```
g :: Int -> Int -> Int
g x y = x * y
```

For **g(1,2)** we'd write **g 1 2**.

*Note*: The type signature for a function with multiple arguments is slightly different from you might expect. Instead of using a domain of pairs of integers (as in Z^2 -> Z), we specify the type of g as **Int -> Int -> Int**.

This is a quirk that proves incredibly useful when you want to make more advanced use of these functions.

Exercise: 

Implement the following mathematical functions in Haskell:

1. f: Z -> Z, f(x) = x^2 + 2x + 4

2. g: Z^2 -> Z, g(x,y) = x^2 - y^2

Answer:

1. 
```
f :: Int -> Int
f x = x^2 + 2*x + 4
```

2.
```
g :: Int -> Int -> Int
g x y = x^2 - y^2
```

As you might have guessed, -- starts a line comment in a Haskell program. You can use {- and -} for block comments.

#### 1.2.4 Building complex functions

Functions like these are the basic building blocks of Haskell programs. A Haskell program is essentially one big expression to be evaluated.

A useful strategy for building complex functions is to break the computation down into smaller pieces, and write helper functions to accomplish these smaller tasks.

Small-scale functions get combined to form larger-scale functions. Larger-scale functions are combined in the same way to form even larger functions, until eventually we have a complete program.

A simple way to combine functions is to take the result of one function application and make that the input argument of another.

### 1.3 Recursion
#### 1.3.1 Recursion: all we have left
Recursion plays a big role in Haskell programming. It's probably the only construct remaining from procedural programming.

On top of being used for things that it would normally be used for in a procedural language, it's also the native means of producing iteration, since Haskell has no looping constructs.

Fortunately, writing recursive functions in Haskell couldn't be more simple! Here's an example, a recursive function for calculating the factorial of a non-negative integer:
```
fact :: Integer -> Integer
fact 0 = 1
fact n = n * fact (n-1)
```
*Note*: 
1. We needed to use parentheses around **(n-1)**. That's because function application has high precedence in Haskell, i.e., **f n-1** is interpreted as **(f n)-1** by default.
2. **Integer** is an infinite precision integer type. That is to say it can store any number, however large (well, until your operating system runs out of memory). **Int**, on the other hand, is a finite precision integer type.

**Recursive Fibonacci**: Write a recursive Haskell function **fib**, which takes an integer input n and returns the nth Fibonacci number, *F(n)*, where *F(0) = 0, F(1) = 1, F(n) = F(n-1)+F(n-2)*

Answer:
```
fib :: Int -> Int
fib 0 = 0
fib 1 = 1
fib n = fib (n-1) + fib (n-2)
```
#### 1.3.2 Pattern matching
The previous definitions made use of a feature of Haskell called 'pattern matching': we gave multiple equations describing the function, and Haskell chose the right equation to follow each time based on the input.

As a reminder, here's the recursive definition for our factorial function:
```
fact :: Integer -> Integer
fact 0 = 1
fact n = n * fact (n-1)
```
Consider a function call with input **x**. Roughly speaking, here's what happens:
1. Let's say **x** is 0. In that case, Haskell notices that the input matches the 'pattern' 0 in the first equation fact 0 = 1. Thus, the first equation applies, and the function returns 1.
2. Alternatively, if **x** is anything other than 0, then Haskell will fail to match it with the pattern 0, and it will move on to try the next equation (fact n = ...). This pattern match will succeed, and n will be used in the definition with the value of x.

*Note*: Patterns are matched in top-down order. So if multiple patterns could potentially match, only the first matching equation will be applied. Likewise, it no patterns match, that will cause a runtime error. It's usually best to make sure that your patterns are both mutually exclusive (no input matches multiple patterns) and exhaustive (every input matches some pattern).

#### 1.3.3 Guards

Pattern matching provides a convenient way to define a function with multiple 'cases', such as recursive functions with a recursive case and a base case.

However, there is a glaring flaw in the previous definition of the factorial function: namely, it will run forever if you ask it for the factorial of a negative number.

To avoid this, we can use another method of defining a function with multiple cases: **guards**.

With guards, we can specify the expression to use for a particular input based on a sequence of boolean conditions. As an example, assuming we define the factorial of a negative number to be, say, 0, here's the factorial function defined with guards instead of multiple patterns:
```
fact :: Integer -> Integer
fact n
    | n < 0     = 0
    | n == 0    = 1
    | otherwise = n * fact (n-1)
```
*Note*: A definition using guards has no equals sign before the first |. Instead, each | starts with a Boolean expression (a 'guard', such as n < 0) and then the corresponding definition is placed after an =. Just like with pattern matching, guards are tested in the order they are written. The first guard to evaluate to **True** will be the definition used. **otherwise** is often used as the last guard: it will always evaluate to **True**.

**Recursive Fibonacci with Guards**: Write a recursive Haskell function fib, which takes an integer input n and returns the nth Fibonacci number, *F(n)* . Assume the Fibonacci numbers are defined as follows:

In the base case: F(0) = 0, F(1) = 1

or, assuming  is greater than 1: F(n) = F(n-1) + F(n-2)

and otherwise (if  is negative): F(n) = 0

Define your function using one pattern and multiple guards.

**Answer**:
``` 
fib :: Int -> Int
fib n
    | n <= 0    = 0
    | n == 1    = 1
    | otherwise = fib (n-1) + fib (n-2) 
```

### 1.4 Operators
#### 1.4.1 Operators

Throughout these exercises we have used many basic operators. Almost all of the operators you are familiar with from other languages are available, including +, -, *, ^, /, <, <=, ==, >=, >, &&, and ||.

Some noteworthy special cases are as follows:
- **/** provides float division, even if its arguments are both integers. For integer division (rounding down), use the div function, as in div 16 3 (which will give 5).
- **%** is not used as the 'modulo' operator. Use the mod function instead, as in mod 16 3 (which will give 1)
- Using **-** as a unary negative sign (as in -2) rather than a subtraction operator (as in 5 - 2) can cause problems. Always enclose such a negative number in parentheses: (-2).
- **!** isn't for Boolean negation (not). Use the not function instead, as in not True (which will give False).
- **!=** is not defined. For 'not equals', use the /= operator.

#### 1.4.2 Operators are functions!
As with pretty much everything in Haskell, operators are just functions! The only difference is that most of them get their arguments from either side, rather than from directly after them.

That is to say, **+** could just as well be a normal function called **plus**, except that we write **1 plus 2** instead of **plus 1 2**.

We say that + (and most other operators) are functions using infix notation, rather than prefix notation, as a way of describing where the arguments should be placed.

If needed, we can easily use an infix function as a prefix function, or the other way around.

To use an infix function (like +) as a prefix function, just enclose it in parentheses: 
```
f :: Int -> Int -> Int
f x y = (+) x y
```
To use a prefix function (like div) as an infix function, just enclose it in backticks ('`' characters):
```
g :: Int -> Int -> Int
g x y = x `div` y
```

#### 1.4.3 Leap year checker
Using your newfound knowledge of operators, write a Haskell function leap which takes an integer year year and returns True if year is a leap year, or False otherwise.

A year is a leap year if it is divisible by 4, unless it is also divisible by 100 but not by 400.

For example:
```
Main> leap 2000
True
Main> leap 2003
False
Main> leap 2004
True
Main> leap 1900
False
```

**Answer**: 

1. 
```
leap :: Int -> Bool
leap year = (year `mod` 4 == 0) && (year `mod` 400 == 0 || year `mod` 100 /= 0)
```


2. This answer performs the tests in order of likelyhood, to minimise the number of tests required for the average year.

```
leap :: Int -> Bool
leap year
    | year `mod` 4   /= 0 = False
    | year `mod` 100 /= 0 = True
    | year `mod` 400 /= 0 = False
    | otherwise           = True
```

#### 1.4.4 Becoming exclusive

A logical operator we will become very familiar with this semester is the exclusive **or**, or **xor**.

The or operator we are already familiar with takes two Boolean arguments and returns True when either of its arguments are True or when both arguments are True.

Like or, this 'exclusive' or operator also takes two Boolean arguments and also returns True if either of them are True. Unlike or, xor will return False in the case where they are both True. That is, for exclusive or to return True, you need one argument or the other to be True, not both arguments.

For this exercise, define a Haskell function **xor** which takes Boolean arguments **x** and **y** and returns the Boolean value that is the exclusive or of **x** and **y**.

**Answer**:
```
-- 'xor' is really just the same as 'not equals' for Booleans:

xor :: Bool -> Bool -> Bool
xor x y = x /= y

-- That was too easy!
-- Here are some alternative solutions:

-- By enumerating all patterns:

xor' False False = False
xor' False True  = True
xor' True  False = True
xor' True  True  = False

-- Other solutions that use pattern matching are possible.
-- For example:

xor'' False x = x
xor'' True  x = not x

-- Guards could work too:

xor''' x y
    | x         = not y
    | otherwise = y

-- Haskell also has an if-then-else expression, which we could use here:

xor'''' x y = if x then not y else y
```
