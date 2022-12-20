The project is a work in progress.
Discord: https://discord.gg/9pDdrKJy8n

# What is "Neuro Block AI"?

"Neuro Block AI" is a project with an alternative approach to code generation using AI. Or a computer program generation to be precise. The main principle is that a function generated with "Neuro Block AI" has no code under the hood - it contains only a miniature trained neural network. 

So in an application with "Neuro Block AI" a programmer may have many blocks of such small neural networks and use them like functions.

The approach is based on test-driven development (TDD). Here are the steps of a standard TDD approach for a function development:

1. A programmer writes unit-tests for a function.
2. A programmer develops an actual function which passes the tests.

These are the steps for a function development with the use of "Neuro Block AI":

1. A developer provides training data for a function - these are in fact the test cases.
2. The framework trains the network finding the opmital structure of the neural network.
3. If there is not enough training data, the framework requires additional test cases.
4. The neural network is ready to be used as a function.

The aim of the project is to develop a framework for generating and training these neurofunctions. The solution should be universal. Also, the framework should rely on the principles of functional programming since it looks most natural for the functions which have a neural network under the hood.

This is how a part of an application may look:
```python
# ------------ On a development stage -----------
# Creating dependencies:
calculate = neurofunction(OUTPUT_DATA_TYPE)

# Training dependencies
calculate.train(train_data)
# -----------------------------------------------

# -------------- On a running phase -------------
# Creating dependencies:
calculate = neurofunction(PATH_TO_PARAMETERS_OF_TRAINED_NETWORK)

def calculate_something(input_data):
  result = calculate(input_data)  # This is a neural network
  print(result)
# -----------------------------------------------

# P.S. - 'neurofunction' is a universal class representing a mini-network. 
# In the repo, it is called 'c_function', will rename it later.
```


# Example: Bulls and Cows

The repository contains the basic example of the usage of the system. This is a Bulls and Cows game implementation.

There are two functions which are implemented as neural network. The first one is a function to validate the guessed code that it contains no repetitions. The second one counts the amount of bulls in the code. Both of them are used in a manually written function which displays the result.

The implementation contains a class `c_function` - this is a universal class which encapsulates the neural network and the code to train it. This is a heart of the framework. Every function (`has_errors()` and `find_bulls_count()`) is an instance of this class.

For example, this is how the process of a function generation looks like for `has_errors()` function:
1. Create an instance of `c_function`:
```python
# The argument 'True' means that the function output is boolean
has_errors = c_function(True)
```
2. Provide train data for the function. For the validation function:
```python
train_data = [
  [1, 5, 8, 1], [1],  # The code '1581' has repetitions, expected output is "1"
  [3, 5, 8, 1], [0],  # The code '3581' has no repetitions, expected output is "0"
]
has_errors.train(train_data)
```
3. Use has_errors as a standard function in other parts of the application:
```python
if has_errors([2, 5, 4, 3])
  print("Error")
```

The function `try_guess()` is written manually. This is the work to do, because it requires to use a recurrent neural network in the nerofunction. And this is a work to do.

# Advantages and disadvantages

The advantage over linguistic generative models is that those networks are not universal. They may produce a correct code for tasks similar to which they have seen a lot during training, but for more rare cases they may be not able to generate a working code. The approach of "Neuro Block AI" is more universal. You provide your inputs and outputs - and the framework just generates an approximator which satisfies the test data.

Disadvantages:
* This is a black box.
* Sometimes it requires too much training data. In the example with Bulls and Cows, the network required over 5000 examples for errors detection and over 1200 examples for bulls calculation. That's too much to provide so much data by hand. And it's also difficult to make it unbiased if this data is generated manually. 

In fact, the second drawback is an achilles heel of the whole approach. The implementation of Bulls and Cows on Python may be no longer then a couple of lines. But in the repository, the functions to just generate the training data are much larger :) But I hope there is a solution, but it's a matter of time to see it. And I actually think that this idea is at least quite interesting and deserves some attention.
