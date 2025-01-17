# Checking Fly Buys Cards

## Introduction

Develop a Ruby application that solves the problem described below. The aim of the problem is to allow the candidate to *demonstrate their skill and experience* in aspects of the development process including domain modelling, object orientated design, use of language construct and idioms, and testing (unit or otherwise).

There is provided sample data to be used for testing and the candidate should be able to demonstrate their solution using the supplied data in the form of a command line interface and/or unit test.

You will be required to talk through your solution at the interview.

## Problem

We want to identify different Flybuys cards by running some sanity checks on the card number.

A common check that is performed upfront is to validate the card type based on the starting digits and length of card number. The main patterns that we care about are as follows:

    +=================+==============================+===============+
    | Card Type       | Begins With                  | Number Length |
    +=================+==============================+===============+
    | Flybuys Black  | 60141                        | 16, 17        |
    +-----------------+------------------------------+---------------+
    | Flybuys Red    | 6014352                      | 16            |
    +-----------------+------------------------------+---------------+
    | Flybuys Green  | 6014355526 - 6014355529      + 16            |
    +-----------------+------------------------------+---------------+
    | Flybuys Blue   | Every other card number      | 16            |
    |                 | starting with 6014           |               |
    +-----------------+------------------------------+---------------+

All of these card types also generate numbers such that they can be validated by an algorithm, so that's the second check systems usually try. The steps are:

1. Starting with the next to last digit and continuing with every second digit going back to the beginning of the card, double the digit
2. Sum all doubled and untouched digits in the number. For digits greater than 9 you will need to split them and sum them independently (i.e. <code>"10", 1 + 0</code>).
3. If that total is a multiple of 10, the number is valid.

For example, given the card number <code>6014355510000028</code>:

    1. 12 0 2 4 6 5 10 5 2 0 0 0 0 0 4 8
    2. 1+2+0+2+4+6+5+1+0+5+2+0+0+0+0+0+4+8 = 40
    3. 40 % 10 == 0

Thus that card is valid.

Let's try one more, <code>6014365510000028</code>:

    1. 12 0 2 4 6 6 10 5 2 0 0 0 0 0 4 8
    2. 1+2+0+2+4+6+6+1+0+5+2+0+0+0+0+0+4+8 = 40
    3. 41 % 10 == 1

This card is not valid.

## Task

Your task is to write a ruby program that accepts Flybuys card numbers. Card numbers must be passed in line by line (one set of numbers per line). The program should print the card in the following format <code>"TYPE: NUMBERS (VALIDITY)"</code>.

## Input and Output

Given the following Flybuys cards:

    60141016700078611
    6014152705006141
    6014111100033006
    6014709045001234
    6014352700000140
    6014355526000020
    6014 3555 2900 0028
    6013111111111111

I would expect the following output:

    Flybuys Black: 60141016700078611 (valid)
    Flybuys Black: 6014152705006141 (invalid)
    Flybuys Black: 6014111100033006 (valid)
    Flybuys Blue: 6014709045001234 (valid)
    Flybuys Red: 6014352700000140 (valid)
    Flybuys Green: 6014355526000020 (valid)
    Flybuys Green: 6014355529000028 (invalid)
    Unknown: 6013111111111111 (invalid)



## Investigation/planning
Initial considerations:
It shouldn't be too hard to come up with a solution/s for this but whats a better way to architect a solution for a company like Loyalty?
The main issue that keeps coming to my mind is that there is likely a lot of existing systems out there that deal with cards/card types/validations (other loyalty systems, credit/bank cards...) that could be beneficial to use or model after but they may not cover the particular challenges this system has to deal with. In terms of domains/subdomains, what is core to this loyalty scheme and what is generic?
I likely need a solution that is tailored but still similar/compatible enough to other systems.

Objectives:
- Evaluate existing solutions and relevance to this project
- Employ some Domain Analysis - identifying core aspects required vrs generic functionalities
- Build out a simple functional MVP within a reasonable time - a system designed to handle the identification, validation, and categorization of card numbers
- Separate concerns, keep it modular, adhere to the Single Responsibility Principle
- Follow a TDD approach to iteratively develop features, I'll be learning as I go
- Include validations
- Include error handling
- Include documentation
- Consider how functionality may be expanded upon later
