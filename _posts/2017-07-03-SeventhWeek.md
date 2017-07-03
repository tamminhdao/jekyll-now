---
layout: page
title: Draw an expandable TicTacToe grid
---

![](/images/gridNote.jpg){:class="img-responsive"}
*This is my favorite thing in this (way too) long post so I put it on top*  

As I work on my TicTacToe game, more and more features are added each week. 
I went from a very simple traditional X vs O game to letting the players pick any string as their symbols of choice.
My current game is played on a 3x3 board but eventually it will also have the options to play on a 4x4, 5x5, etc... 
With each new feature, I have to improve the algorithm to draw my TicTacToe grid to the console.
This week, I finally get to a point where I can dynamically draw a pretty nice looking grid that meets all the following requirements:

* is a traditional looking tictactoe square grid which has no outside border but only the inner lines which divide the cells
* can be any dimension (3x3, 4x4, 5x5 etc...)
* can adjust to any length of the symbols input by players
* the symbols has to center in the middle of its cell

This has turned out to be quite an interesting challenge for me. 
While it's definitely not easy, the requirements were added incrementally so I was always solve only one problem at a time. This is by design, because I am still learning. 
However, in general, when presented with a complicated problem, a good strategy is to start with a simpler version of the big problem and build on from there.


**Stage 1: Draw a grid that works only which symbol length of 1 such as X and O**
For now, we will assume that the board only takes symbol with string length of 1.
Since the two symbols are of the same length, the grid will automatically be a square and the symbols can easily be centered
in its cell. We can just focus on the first two requirements about the grid lines and the dimension.

This is how a 3x3 grid would look like:

```java
 X | O | X 
--- --- ---
 O | X | O 
--- --- ---
 X |   |   
```

In order to draw, the Grid class must have information about the board, its dimension and the total number of cells.

Here is a Grid class that takes in a Board with a variable called boardSize (the total number of cells on the board) and a variable called CellsPerRow (the square root of boardSize).

```java
public class Grid {
    private Board board;
    private int boardSize;
    private int cellsPerRow;
    private final int CELL_OFFSET = 1;

    public Grid(Board board) {
        this.board = board;
        this.boardSize = board.getBoardSize();
        this.cellsPerRow = board.getCellsPerRow();
    }
}
```

My approach is to
1. Draw each row which, depending on the number of cells per row, is a repeating pattern of: a cell, a vertical divider, a cell, etc... and has to end with a cell
2. Draw each row divider which in this case is the series of dashes grouped together to match the length of each symbol and span across the full length of a row
3. Put the row and the divider together to construct the grid

Step 1: Draw each row
Notice that in a square grid indexing from 0, the indexes in last column divided by the number of cells per row will always result in a remainder of "the number of cells per row minus one". This pattern will help us draw all the vertical dividers between cells within a row without having an extra divider after the very last cell in each row.

A 3x3 board: the number of cells per row minus one = 3 - 1 = 2
```java
 0 | 1 | 2                  2 % 3 = 2
--- --- --- 
 3 | 4 | 5                  5 % 3 = 2
--- --- --- 
 6 | 7 | 8                  8 % 3 = 2
```
A 5x5 board: the number of cells per row minus one = 5 - 1 = 4
```java
 0  | 1  | 2  | 3  | 4                  4 % 5 = 4
---- ---- ---- ---- ---- 
 5  | 6  | 7  | 8  | 9                  9 % 5 = 4
---- ---- ---- ---- ---- 
 10 | 11 | 12 | 13 | 14                14 % 5 = 4
---- ---- ---- ---- ---- 
 15 | 16 | 17 | 18 | 19                19 % 5 = 4
---- ---- ---- ---- ---- 
 20 | 21 | 22 | 23 | 24                24 % 5 = 4
```

```java
    private String drawARow(int startingCell) {
        StringBuilder row = new StringBuilder ("");
        for (int cellIndex = startingCell; cellIndex < startingCell + cellsPerRow; cellIndex++) {
            if (cellIndex % cellsPerRow == this.cellsPerRow - 1) {
                row.append("  " + board.getSymbol(cellIndex) + "  ");
            } else {
                row.append("  " + board.getSymbol(cellIndex) + "  " + "|");
            }
        }
        return row.toString();
    }
```

Step 2: Draw each row divider

At this stage, the divider unit is hardcoded as "--- " because we are only working with string symbols of length 1.
We will have to change this in the future.

```java
    private String drawRowDivider() {
        String horizontalLine = "\n";
        for (int i = 0; i < this.cellsPerRow; i++) {
            horizontalLine += "--- "; 
        }
        horizontalLine += "\n";
        return horizontalLine;
    }
```

Step 3: Put the row and the divider together

Our `drawARow()` takes an index of the first cell in each row so it knows which row to draw.
The for loop in this `getGrid()` function steps through the cell indexes in strategic interval so it can pick out the first index in each row and pass that on to `drawARow()`.
The function alternates between drawing a row and drawing a line of divider, until it hits the first index of the last row then it skips the divider. 

```java
    public String getGrid() {
        StringBuilder grid = new StringBuilder ("");
        for (int cellIndex = 0; cellIndex <= this.boardSize - this.cellsPerRow; cellIndex += this.cellsPerRow){
            grid.append(drawARow(cellIndex));
            if (cellIndex < this.boardSize - this.cellsPerRow) {
                grid.append(drawRowDivider());
            }
        }
        grid.append("\n");
        return grid.toString();
    }

    public void drawGrid() {
        System.out.println(getGrid());
    }
```

**Stage 2: Improve the grid so that it can accomodate any string length**

At this stage, the players should be able to pick any string of any length as their symbols of choice. The two symbols do not neccessary have the same length. 
A few things we have to adjust with our grid:
1. If the two symbols are of different length, we need to find out the length of the longer string and set that as the width of each cells.
2. The longer string will fit snuggly in each cell space. We need to find a way to center the shorter string in the middle of the cell.
3. Our divider unit (--- ) cannot be hardcoded anymore.

Step 1: Take care of the length difference:
Loop through all the cells in the board, get the length of each element and add it to an array. Return the max value.

``` java
    private int findMaxStringLengthOfSymbols() {
        String[] boardArray = this.board.getSymbol();
        ArrayList<Integer> symbolLength = new ArrayList<>();
        for (int i = 0; i < boardArray.length; i++) {
            symbolLength.add(boardArray[i].length());
        }
        return Collections.max(symbolLength);
    }
```

Step 2: Center the shorter symbol
My first thought when I faced this problem was to pad both right and left side of the shorter symbol with a certain number of space character.
In the end, I discovered a library which does a much better job. If there were no such library, I would have started to pad left and right. 

```java
    private String getCenteredSymbols(String symbol, Integer maxLength) {
        if (symbol.length() == maxLength) {
            return symbol;
        } else {
            return StringUtils.center(symbol, maxLength);
        }
    }
```

Step 3: Adjust the line of divider, the number of dashes has to be calculated according to length of the longer string symbol.

```java
    private String drawRowDivider() {
        String horizontalLine = "\n";
        for (int i = 0; i < this.cellsPerRow; i++) {
            horizontalLine += this.drawingDividerUnit();
            horizontalLine += " ";
        }
        horizontalLine += "\n";
        return horizontalLine;
    }

    private String drawingDividerUnit() {
        String unit = "";
        int paddedSpaceCharacterOnBothEndsOfSymbols = 2;
        int maxLengthOfSymbols = this.findMaxStringLengthOfSymbols();
        for (int j = 0; j < maxLengthOfSymbols + paddedSpaceCharacterOnBothEndsOfSymbols; j++) {
            unit += "-";
        }
        return unit;
    }
```

At this point we have a pretty flexible grid, except when players choose long strings as their symbols, the grid is no longer square but rather rectangular looking.

```java
    1    |    2    |    3    
--------- --------- --------- 
    4    |    5    |    6    
--------- --------- --------- 
    7    |    8    |    9     

                                             |         | loooong
                                    --------- --------- --------- 
                                             |   sqw   |         
                                    --------- --------- --------- 
                                             |         |         

```
So we need to fix that. 

**Stage 3: Add vertical padding to square the grid**

The trick here is to decide when to add vertically padding, how many lines of padding on the top or on the bottom, and most difficult of all is to generallize the pattern.
I know the padding depends on the length of the longer symbol which determine the width of a cell. But the relationship between the vertical line height and the cell width is not a 1:1 relationship.
I need to figure out the appropriate ratio. 

If the row of padding is on top of the symbol, the pattern is something like this
```java
 "       |       |       + \n"
 ```
If the row of padding is below the symbol, the pattern has to change a little 
```java
"\n +       |       |       "
```
The exact number of space character in each cell is identical to the number of dashes used to draw a divider unit. There is a chance to refactor and reuse the function `drawingDividerUnit()`. 

At this point, the simplest approach is to test hardcode these upper and lower padding rows into `drawARow()`, print out the grid with different symbols of different length and see if a pattern emerges.
I had a table to track how many row of padding should be added according to the max length of the symbols.

![](/images/gridNote.jpg){:class="img-responsive"}

In the end, the pattern I spotted was: The total number of padding rows needed = Max length of symbol / 3. (take the quotient, ignore the remainder).
After that it's easy to divide the total in half, half padded on top half padded on the bottom, so that our symbol is centered vertically as well as horizontally. 
Why divided by 3? I called this number "WIDTH_TO_HEIGHT_RELATIVE_CONVERSION_UNIT". 
I've tested it with 4x4 and 5x5 grid and 3 seems to be truly the magic number. Probably because the length of three character next to each other is roughly even with the height of one horizontal line.


The final version of this Grid class can be found on my <a href="https://github.com/tamminhdao/tictactoe-java">tictactoe-java</a> repo on gitHub. But here is the result from a few tests I ran.

A 4x4 board with a symbol of 6 characters:
```java
        |        |        |        
 ABCDEF |  $$$   | ABCDEF |  $$$   
        |        |        |        
-------- -------- -------- -------- 
        |        |        |        
 ABCDEF |  $$$   | ABCDEF |  $$$   
        |        |        |        
-------- -------- -------- -------- 
        |        |        |        
 ABCDEF |  $$$   | ABCDEF |  $$$   
        |        |        |        
-------- -------- -------- -------- 
        |        |        |        
 ABCDEF |  $$$   | ABCDEF |  $$$   
        |        |        |        
```


A 5x5 board with a symbol of 9 characters:        

```java
           |           |           |           |           
           |     O     | abcdehijk |           |           
           |           |           |           |           
           |           |           |           |           
----------- ----------- ----------- ----------- ----------- 
           |           |           |           |           
           |           |           | abcdehijk |           
           |           |           |           |           
           |           |           |           |           
----------- ----------- ----------- ----------- ----------- 
           |           |           |           |           
           |           |     O     |           |           
           |           |           |           |           
           |           |           |           |           
----------- ----------- ----------- ----------- ----------- 
           |           |           |           |           
           |           |           |           |           
           |           |           |           |           
           |           |           |           |           
----------- ----------- ----------- ----------- ----------- 
           |           |           |           |           
 abcdehijk |           |           |           |           
           |           |           |           |           
           |           |           |           |           
```

Such pretty squares! 