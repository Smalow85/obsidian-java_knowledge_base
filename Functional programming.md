There are two main groups of functional programming languages, which are:

- Pure functional languages: These languages support only functional paradigms. Haskell is an example of this type of language.
- Impure functional languages: These languages support the functional paradigms and imperative style programming. Lisp is a common example of an impure functional language.

**Here's a list of the most common characteristics of functional languages:**  
### First-class functions

Many functional programming languages allow **functions** to act as several computational elements, such as the result of another function, in a collection of other functions, as a variable or as an argument for another function. Programmers refer to these kinds of functions as first-class because of their versatility. Higher-order functions are a type of first-class function that allows for arguments in the form of other functions or return functions as a result of execution. For example, a function within these languages can cause a computation that results in the creation of several other functions with defined variables.
### Immutable data

Data is **immutable** if nothing can modify it once a program creates it. Functional programming languages use only immutable data, and because of this, they also maximize the ability to reference previous data. For instance, a function that deletes data may not fully delete it in functional languages because deleting a piece of data implies some change to its existence.
### Pure functions

When using pure functions, the result of a function is always the same if the arguments are the same. Additionally, pure functions have no side effects, meaning there are no changes to the state of the program as a result of the execution of the function. For example, a pure function might be the formula "2 + 2 = 4." In this, "2 + 2" is the argument, and "4" is the result.

### Recursion

The solving of complex problems using simpler, smaller solutions is a fundamental principle of many functional programming languages. An example of recursion is any mathematical function in which you call upon the function itself in order to solve the problem. Many functions have solutions that don't use their own function in the solution, but they may be more complex and computationally intensive.

