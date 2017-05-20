---
layout: page
title: Java and Test Driven Development
---

After a week, I am slowly warming up to Java’s verbose syntax and strict typing. Does a function return something or not, what type of data does it return, what type of parameter does it take? Everything has to be clearly stated in the function declaration. Since I have only coded in JavaScript, this new way of doing thing takes a while to get used to. However, having these strict rules also improve clarity of intent, which I assume can be comforting especially when things get more complicated.

A few things jump out to me as I transition from JavaScript to Java. In Java, there is no global variable, everything is wrapped in a class. If I want to shield something from being exposed or altered, I just label it private or final. This might not seem like a big deal to other people. However, coming from JavaScript where I know global variables are bad news yet have never been able to completely avoid them, this Java feature is like… I just arrive in a new city, and someone walks to me and hand me a hundred dollar bill as a “welcome to our city” present! 

Another cool thing is method overloading. The below methods can co-exist in the same class. They have the same name, but different argument lists. Java considers them two different things. No need to come up with different names. 

```java
        public void bringToLife (int x, int y) {
            coordinatesOfLivingCells.add(x,y);
        }

        public void bringToLife (Point p) {
            coordinatesOfLivingCells.add(p);
        }
```


Java is object oriented. I am not completely new to object oriented programming (OOP). JavaScript is also object oriented. However, I clearly have not understood or embraced OOP all the way. In our first live coding session, my mentors pointed out that there are many benefits of OOP (inheritance, encapsulation) and while I am coding in Java, I should fully immerse in its object-oriented style which means no more static method.


One brand new topic for me is test-driven development (TDD). Udacity briefly introduces testing with Jasmine.js in their front-end program. But I did not see TDD in action until my on-site interview at 8th Light when I pair-programmed with my mentors. These days, every piece of code I write has to be preceded by a test. The testing framework for Java is junit. Start with writing a test, and then write just enough code to pass the test. Keep going like that so every bit of code is fully tested. It’s easier said than done. First, I have to come up with an appropriate test. Second, make sure to test for all scenarios where my code might break. Hopefully, with practice, it will become more natural. 

