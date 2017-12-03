---
layout: page
title: I/O testing revisited
---

While working on my current project, I have to deal with obtaining input and publishing output for my program.
While I'm at this, I also revisit a few things I've touched on in the past, i/o testing, stubbing and mocking.

If we have a method which generates a string. We definitely need to test that method to ensure that we’re producing the expected string. The answer to whether it is necessary to test the actual i/o bit differs from one person to another.
i/o testing are tests concerning things that directly reads from or writes to the terminal, `InputStream` and `OutputStream` classes in Java for example.
When I worked on tictactoe in Java, I tried i/o testing where I learned about stubbing.
In the snippet of code below, I created and fed a stream into a scanner.

```java
//Code
public class CommandLineIO implements IO {
    private Scanner scanner;
    private PrintStream outStream;

    public CommandLineIO(Scanner scanner) {
        this.scanner = scanner;
        this.outStream = System.out;
    }

    @Override
    public String obtainInput() {
        String input = this.scanner.nextLine();
        return input;
    }
}
//Test
public class CommandLineIOTest {
    @Test
    public void canCaptureAndReturnInput() {
        String input = "X";
        byte[] inputSource = input.getBytes(StandardCharsets.UTF_8);
        InputStream stream = new ByteArrayInputStream(inputSource);
        Scanner scanner = new Scanner(stream);
        CommandLineIO io = new CommandLineIO(scanner);
        String userInput = io.obtainInput();
        assertEquals(userInput, "X");
    }
}
```

Supposed I had decided that it's not necessary to do i/o testing.
I still need to test `obtainInput` method of CommandLineIO class to make sure that it indeed obtain and return the expected string.
However,  I will trust that Scanner's `nextLine` function knows how to do its job. Therefore, I would not go through the
whole process of stubbing out my input.

```java
//create an Interface
public interface InputReceiver {
    String returnInput();
}
```

```java
//Both the mock and the real Scanner implements the same interface

//mock
public class MockScanner implements InputReceiver {
    public String returnInput() {
        return "hello";
    }
}

//real
public class ScannerImplementation implements InputReceiver {
    public String returnInput() {
        Scanner scanner = new Scanner(System.in);
        String scannerInput = scanner.nextLine();
        return scannerInput;
    }
}
```

```java
//Code
public class CommandLineIO implements IO {
    private InputReceiver inputReceiver;
    private PrintStream outStream;

    public CommandLineIO(InputReceiver inputReceiver;) {
        this.inputReceiver = receiver;
        this.outStream = System.out;
    }

    @Override
    public String obtainInput() {
        String input = this.receiver.returnInput();
        return input;
    }
}

//Test
public class CommandLineIOTest {
    @Test
    public void canCaptureAndReturnInput() {
       MockScanner mockScanner = new MockScanner();
       CommandLineIO io = new CommandLineIO(mockScanner);
       String userInput = io.obtainInput();
       assertEquals(userInput, "hello");
    }
}
```

I still remember how much I struggled to understand these test a few months ago.
Back then, I thought these were just two different ways to test.
I didn't fully understand that these two ways reflect two different
testing philosophies, for lack of a better word. Whether one chooses to go the first or second
route depends on what one's view about i/o testing, whether it is necessary or not.

I guess one thing I am still not sure is the rationale of i/o testing.
The argument for not doing i/o testing is that we don't need to test any built in classes or functionalities of a language.
In the example above, at some point, we trust that `Scanner` which is built into Java knows what to do.

Nobody has explained to me the opposite side of why i/o testing might be necessary.
Does it mean we don't trust `Scanner` and its '`nextLine` method? It seems reasonable to me that we should be able to stop there.
So maybe I'm already leaning in the "no i/o testing camp". But I don't want to sort myself too soon. I'll keep an open mind.
