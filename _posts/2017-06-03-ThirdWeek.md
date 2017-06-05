When I walked through Uncle Bob's bowling score kata powerpoint, one thing that really stood out to me was the way he refactored his tests to remove duplication and ugly comments. Not only did he write tests, but also held them in the highest standard. I am constantly reminded to write tests and clean up my code. Yet, my brain doesn't automatically extend the practice to "maintain your test" and "clean up your test". I can only speculate that while I was telling myself that tests are important, my mind still subconsciously saw them as secondary. Secondary to the code they accompany. After all, the production code is what gives life to our software, isn't it?

Then I read Uncle Bob's blog post <a href="https://8thlight.com/blog/uncle-bob/2013/09/23/Test-first.html">Test First</a>. What is "test first"? I used to simply associate this term with the order of operation, i.e. we write the test before we write the code. But there is more. In this post, Uncle Bob presented readers with a dilemma. Imagine you have been developing a program with complete test suites, using proper test-driven development (TDD) along the way. Now for some reason, you can only preserve half of your program, either the code, or the test. Which half would you rather preserve?

He went on to explain:

"If somehow all your production code got deleted, but you had a backup of your tests, then you'd be able to recreate the production system with a little work.
[...]
If, however, it was your tests that got deleted, then you'd have no tests to keep the production code clean. The production code would inevitably rot, slowing you down. The quality of the code, and the productivity of the team would be caught in the downward spiral towards zero -- and there's nothing they could do to stop it other than trying recreate the test suite. And, as we noted, recreating the test suite is very hard indeed."

Of course, this is just hypothetical, but it serves as a good thought experiment. After reading his analysis, I see tests in a whole new light. Do I still think the code is the hero? Yes. However, I no longer think of the test as secondary but rather that test and code are equal partners.

The three rules of TDD as summarized by Uncle Bob are:
* You are not allowed to write any production code unless it is to make a failing unit test pass.
* You are not allowed to write any more of a unit test than is sufficient to fail.
* You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

TDD is about taking small but solid steps and build up gradually. For me personally, this approach seems to help with problem solving. When presented with a complex problem, my thoughts often go off in several directions, searching for different possibilities. It can be very hard to get started. TDD keeps me grounded. Practicing TDD, I was told to start with the simplest test, answer the simplest questions then expand to more complicated ones. In the bowling score kata, the order of questions would be:

1. The simplest: Can I create a game?
2. A little more difficult: Can my game give me a correct score of 0 in case of a gutter game?
3. Again, just a little more difficult: Can my game give me a correct score of 20 in case I roll down 1 pin in each of the 20 rounds.

That's the order I write my tests and the order I develop my code. From there I go on with harder and harder tests until I have taken care of all scenarios.

When presented with a complex problem, this approach still works, but first, I need to break the big program into small components. Take Game Of Life for example, there is the algorithm (the set of rule to determines if a cell live or die), there's a board and there are points on the board. For TicTacToe, there is the Board/Grid, a set of rules to determine the state of the game, there's the AI (the computer player), and a set of prompt to obtain the human player's move. Having these components, I can now start asking questions about each of them in the order from simplest to hardest.
