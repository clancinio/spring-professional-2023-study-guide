# Spring Certified Professional 2023 - Study Guide

## Module 2 - Aspect Oriented Programming

***

### 1. What is the concept of AOP?

AOP - Aspect Oriented Programming is a programming paradigm that compliments OOP by providing a way to seperate groups of cross-cutting concerns from business logic code. This is achieved by the ability to add additional behaviour to the code without having to modify the code itself. This is achieved by specifying:

- Location of the code which the behaviour should be altered - Pointcut is matched with Join point
- Code which should be executed that implements cross-cutting concern - Advice

#### Which problem does it solve?

AOP solves the following:

- Allows proper implementation of cross-cutting concerns
- Solves code duplications by eliminating the need to repeat  the code for functionalities across different layers. Such functionalities may include logging, performance logging, monitoring, transactions, caching.
- Avoids mixing unrelated code. For example, mixing transaction logic code (commit, rollback) with business code makes code harder to read. By separating concerns, code is easier to read, interpret, and maintain.


#### Name three typical cross-cutting concerns?

- Logging
- Performance Logging
- Caching
- Security
- Transactions
- Monitoring 

#### What two problems arise if you don't solve a cross?

Implementing cross-cutting concerns without using AOP produces the following challenges:

- Code duplications - Before/After code is duplicated in all locations when normally Advice would be applied. Refactoring by extraction helps, but does not fully solve the problem
- Mixing of concerns - business logic code mixed with logging, transactions, caching makies the code hard to read and maintain

***

### 1. What is a pointcut, a join point, an advice, an aspect, and weaving?
