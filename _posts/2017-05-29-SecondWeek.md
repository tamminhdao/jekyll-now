---
layout: page
title: Apprenticeship Patterns. Recursion.
---


I've started reading this book: <a href="https://www.amazon.com/Apprenticeship-Patterns-Guidance-Aspiring-Craftsman/dp/0596518382">Apprenticeship Patterns - Guidance for the Aspiring Software Craftsman.</a> The second chapter of the book is titled "Emptying the Cup". It's the idea that in order to learn and be receptive to learning, you have to keep an open mind, willing to try counter-intuitive approaches, let go of old habits. I've been programming for only a year. My cup is pretty empty to begin with. Nevertheless, I find this advice very helpful.

My very first programming language was actually Python. My first line of code was:
```python
print("Hello World!")
```
I didn't write anything significant in Python, only used it to do some short exercises and learnt the most basic programming concepts. But to this day, Python has a special place in my heart. I think it's a great language to start coding with. The syntax is simple and reads almost like English.

When I started Udacity front-end program and switched to Javascript, I didn't want to empty my cup. I didn't want to empty a single drop from my cup! I kept forgetting to put "var" at the beginning of every variable declaration and hated having to do it. Python didn't make me do that. I especially hated the cumbersome squiggly brackets. I hated the Javascript syntax until I eventually wrote enough code to get used to it. It took me much longer to get where I needed to be because I was resisting all the way. It's hard enough learning a new language with enthusiasm let alone holding a grudge against it from the start. So this time, going from JavaScript to Java while reading this chapter, I try not to compare the two languages. If I compare them, I am actually marketing Java to myself, actively thinking about the potential superior features of the language. I think it's working.

Two more patterns mentioned in this book are Expose Your Ignorance and Confront Your Ignorance. In the spirit of exposing and confronting ignorance, let's talk about recursion. It's a technique I've used in my code but still feel shaky about. If I am presented with a solution to a problem which utilizes recursion, the logic of the solution is often very clear to me. I run into problem when I have to construct my own solution with a recursive function. First of all, I am not yet skilled at recognizing a situation where recursion would be a good strategy. And even if I know recursion would potentially be helpful, I still have a hard time translate the idea into code.

However, the few times that I've successfully used recursion to solve problems, I am super impressed with how elegant and powerful this technique is. If done right, recursion makes the solution looks effortless. I was working on the coin changer kata last week. My first version had a lot of code repetition. One of the clean code principles is Don't Repeat Yourself (DRY). Don't repeat because it's hard to maintain repetitive logic. I spent two days DRY-ing my codebase. I finally did it using recursion. The essence of this strategy is divide and conquer. We can't immediately solve this big complicated problem but we can solve a small simpler version of it. Solutions to small problems can then be combined to solve the original big problem. In programming, we apply this technique by writing a recursive function, i.e. a function that calls itself.

There are two parts to a recursive function: the base case and the recursive case.
- The base case gives a direct answer to the simplest version of the problem. It also serves as a break, preventing the function from going into an infinite loop.
- The recursive case is where the function calls on a simpler version of itself.

For a much better explanation of recursion with detailed examples, I recommend checking out <a href= "https://www.amazon.com/Grokking-Algorithms-illustrated-programmers-curious/dp/1617292230"> Grokking Algorithms: An illustrated guide for programmers and other curious people</a> by Aditya Bhargava.
