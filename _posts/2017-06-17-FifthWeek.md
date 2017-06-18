---
layout: page
title: I/O testing using Dependency Injection
---

This week, I worked on obtaining user input (cell selection input, to be specific) for TicTacToe. 
I started out planning to do this the proper TDD way and immediately ran into problem. This was the first time I encountered i/o testing. 

Since I couldn't imagine how the test would work, I decided to set TDD aside and just went straight for the code.
This is how I did it the first round:

```java
public class UserInputReceiver {
    private Scanner scanner = new Scanner(System.in);

    public int obtainCellSelection() {
        System.out.println("Enter your cell selection (1 - 9): ");
        int cell = this.scanner.nextInt();
        return cell;
    }
}
```

This code works. But I later learned that it cannot be tested. 
Currently, my `UserInputReceiver` class instantiates a Scanner on its own, a Scanner that take `System.in` as argument. In another word, `scanner` is a dependency of `UserInputReceiver`. 
When we run this code, there will be a user to enter this input. However, when we test, `System.in` became a problem because the test would just get stuck waiting for an input that will never arrive. 

To cope with this, we can use dependency injection. What is dependency injection?
This is a pretty good explanation from <a href="http://www.javaworld.com/article/2071914/excellent-explanation-of-dependency-injection--inversion-of-control-.html">javaworld.com</a>:

"Any nontrivial application is made up of two or more classes that collaborate with each other to perform some business logic. Traditionally, each object is responsible for obtaining its own references to the objects it collaborates with (its dependencies). When applying DI, the objects are given their dependencies at creation time. In other words, dependencies are "injected" into objects."

Here is my code again, using dependency injection:

```java
public class UserInputReceiver {
    private Scanner scanner;

    // Instead of having the class initiate the dependency (scanner), the dependency is passed in via a constructor. 
    public UserInputReceiver (Scanner scanner) {
        this.scanner = scanner;
    }

    public int obtainCellSelection() {
        System.out.println("Enter your cell selection (1 - 9): ");
        int cell = this.scanner.nextInt();
        return cell;
    }
}
```

With the code properly set up with DI, let's turn to the test.
Let's write out the easy part first, and put some question marks in the tricky parts.

```java
public class UserInputReceiverTest {
    @Test
    public void canObtainAndReturnCellSelection() {
        Scanner scanner = new Scanner(???);
        UserInputReceiver receiver = new UserInputReceiver(scanner);
        int cell = receiver.obtainCellSelection();
        assertEquals(cell, ???);
    }
}
```

To complete this test, I need to first understand the concept of `stream`. A stream is a sequence of data. Think of a stream as a conveyor belt of input or output.
The scanner I'm familiar with takes `System.in` as an argument. `System.in` happens to be an object of type InputStream. 
Because `System.in` would not work in the test, I need to substitute it with another InputStream object (this action is called stubbing). 
InputStream itself is an abstract class so I can't simply write:

`InputStream stream = new InputStream("myInput".getBytes());`

Instead, I need to find a concrete subclass of InputStream to wrap the string input in.
The final test looks like this:

```java
public class UserInputReceiverTest {
    @Test
    public void canObtainAndReturnCellSelection() {
        InputStream stream = new ByteArrayInputStream("99".getBytes());
        Scanner scanner = new Scanner(stream);
        UserInputReceiver receiver = new UserInputReceiver(scanner);
        int cell = receiver.obtainCellSelection();
        assertEquals(cell, 99);
    }
}
```

I was more involved with the method above, but Cyrus also showed me another way, using Interface.
I feel this post is not complete if I don't include it too.


First, create an interface:

```java
public interface InputReceiver {
    int returnInput();
}
```

Second, create two scanner classes that implement this interface. 
These scanners will be used as dependencies in our `UserInputReceiver`. 
Because both classes implement the same interface, they each have a `returnInput()` method that return an `int`.

```java
public class TestScanner implements InputReceiver {
    public int returnInput() {
        return 100;
    }
}
```

```java
public class ScannerImplementation implements InputReceiver {
    public int returnInput() {
        Scanner scanner = new Scanner(System.in);
        int scannerInput = scanner.nextInt();
        return scannerInput;
    }
}
```

Third, write the `UserInputReceiver` class using dependency injection:
```java

public class UserInputReceiver {
    private InputReceiver inputReceiver;

    public UserInputReceiver (InputReceiver receiver) {
        this.inputReceiver = receiver;
    }

    public int obtainCellSelection() {
        System.out.println("Enter your cell selection (1 - 9): ");
        int cell = this.inputReceiver.returnInput();
        return cell;
    }
}
```

Finally, the test:

```java
public class UserInputReceiverTest {
    @Test
    public void canCaptureAndReturnInput() {
        TestScanner scanner = new TestScanner();
        UserInputReceiver receiver = new UserInputReceiver(scanner);
        int output = receiver.obtainCellSelection();
        assertEquals(output, 100);
    }
}
```
Notice there is no mention of the actual `ScannerImplementation` in this test. 
`UserInputReceiver` is tested through the mock `TestScanner`.

---------

Regardless of the method, the test was essentially 5 lines of code. Getting there was a struggle.
There are different opinions among developers about i/o testing, to test or not to test, and how to test. I wanted to do this to have an idea how it might be done. 
Despite the struggle, I'm glad I've gone through this exercise. Moreover, I am happy to have explored various new concepts along the way, concepts not only specific to i/o testing but are good to know in general.
Last but not least, I'm lucky to have my mentors and my fellow apprentices to talk through my difficulties and have them point me to the right direction. Thank you all!
