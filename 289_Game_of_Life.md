### [Game of Life](https://leetcode.com/problems/game-of-life/)   

Difficulty **Medium**

tags `array`

According to the [Wikipedia's article](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life): "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with [its eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood) (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population..
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state.

**Follow up:**

1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?


思路:


current state | neighbor number | next round state
--------------|-----------------|---------------
1 | <2  | 0
1 | 2,3 | 1
1 | >3  | 0
0 | 3   | 1

每有一个活着的邻居+2， 这样就能用奇偶性推断初始状态。

**solution 1**

```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        vector<vector<int>> d = {{0,-1},{-1,-1},{-1,0},{-1,1},{0,1},{1,1},{1,0},{1,-1}};
        for (int x=0; x<board.size(); x++) 
            for (int y=0; y<board[0].size(); y++)
                if (board[x][y]%2 > 0)
                    for (int k=0; k<8; k++) {
                        int x_ = x+d[k][0], y_ = y+d[k][1];
                        if (x_>=0 && x_<board.size() && y_>=0 && y_<board[0].size()) 
                            board[x_][y_] += 2;
                    }
        for (int x=0; x<board.size(); x++) 
            for (int y=0; y<board[0].size(); y++) {
                if (board[x][y] >= 5 && board[x][y] <= 7)
                    board[x][y] = 1;
                else
                    board[x][y] = 0;
            }   
    }
};
```
