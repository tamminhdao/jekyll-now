---
layout: page
title: Expressive Code
---

There were two or three times when Cyrus commented that he liked a particular section of my code because it was expressive.
For a long time, I didn't quite understand what it means for code to be expressive. Now, after I have some acquaintances with good design rules and principles, I understand expressive as to clearly express the developer's intent. Expressive code allows someone to read the code and easily follow the logic of the system. The more expressive the better because reading code is difficult. Developers have to cultivate the ability to read and pick up code in unfamiliar languages (this reminds me of sight reading for musicians, the ability to read and perform music at first sight). I need to practice reading a lot more code. As I added new features into my TicTacToe game, once in a while I've had to go back and make change to code I wrote a month before that. A month is not that long, given that this is my code in a project I'm actively working on, I was a little upset at how much time it took me to understand my own code.

Last week I wrote about Minimax and I had this snippet of code on my post. 

```java
public class UnbeatableComputerPlayer{
    public int obtainValidCellSelection() {
        // This variable takes care of the discrepancy between 0-based index and normal counting from 1
        int CELL_OFFSET = 1;
        //Create the map to store pairs of score and cell
        HashMap<Integer, Integer> scoreAndCell = new HashMap<>();
        //Looping through each empty cell to calculate its score
        //Save score and cell to the map
        for (int index = 0; index < board.getBoardSize(); index++) {
            if (isEmptyCell(board, index)) {
                board.insertSymbol(selfSymbol, index);
                Minimax algorithm = new Minimax(this.rule, this.board, this.selfSymbol, this.opponentSymbol);
                int score = algorithm.scoreACell(board, false, 0);
                scoreAndCell.put(score, index);
                board.resetCell(index);
            }
        }
        //Get the highest score from the map and its corresponding cell
        Set allKeys = scoreAndCell.keySet();
        ArrayList<Integer> listOfKeys = new ArrayList<>(allKeys);
        int maxScore = Collections.max(listOfKeys);
        int cell = scoreAndCell.get(maxScore);
        return cell + CELL_OFFSET;
    }
}
```

This week, I look at it again and I realize that this function is not great. It's doing too much. I'm adding more comments here to better explain what this function does, but not in my source code. That's a bad sign right there when the function is so hard to read that comments have to be added to explain what's going on. This function's objective is returning a cell number after it has calculated the best move. It does so by looping through each cell and score each of them using an algorithm.
There is a map of each cell and its score. Every time the function scores a cell, it saves the cell and score information into a key value pair in this map. In the end, it picks the highest score and return the corresponding cell number. 
I am pretty sure that when I revisit it in a month I will need to spend a lot of time trying to figure out what I was thinking. Better improve it right now and save my future self.
The loop can be extracted into a private method, so is the logic of obtaining the highest score in the map. Each method should have a name that describe what the method accomplishes. These days I find myself spending a lot of time thinking about names.

```java
    private void testAndScoreEveryEmptyCell(HashMap<Integer, Integer> scoreAndCell) {
        for (int index = 0; index < board.getBoardSize(); index++) {
            if (isEmptyCell(board, index)) {
                board.insertSymbol(selfSymbol, index);
                Minimax algorithm = new Minimax(this.rules, this.board, this.selfSymbol, this.opponentSymbol);
                int score = algorithm.scoreACell(board, false, 0);
                scoreAndCell.put(score, index);
                board.resetCell(index);
            }
        }
    }

    private int pickCellWithTheHighestScore(HashMap<Integer, Integer> scoreAndCell) {
        Set allKeys = scoreAndCell.keySet();
        ArrayList<Integer> listOfKeys = new ArrayList<>(allKeys);
        int maxScore = Collections.max(listOfKeys);
        return scoreAndCell.get(maxScore);
    }
```

My original function now looks like this:

```java
    public int obtainValidCellSelection() {
        int CELL_OFFSET = 1;
        HashMap<Integer, Integer> scoreAndCell = new HashMap<>();
        testAndScoreEveryEmptyCell(scoreAndCell);
        int cell = pickCellWithTheHighestScore(scoreAndCell);
        return cell + CELL_OFFSET;
    }
```

Anyone who reads this function will get a better idea of the steps involved in obtaining a valid cell selection.
Maybe after another month when I look at this again, I will be able to improve it even more.
For now at least it's much more expressive than the previous version.