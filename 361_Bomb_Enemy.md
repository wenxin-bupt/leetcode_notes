### [Bomb Enemy](https://leetcode.com/problems/bomb-enemy/)

Difficulty **Medium**

tags `array`

Given a 2D grid, each cell is either a wall 'W', an enemy 'E' or empty '0' (the number zero), return the maximum enemies you can kill using one bomb.
The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.
Note that you can only put the bomb at an empty cell.

Example:

```
For the given grid

0 E 0 0
E 0 W E
0 E 0 0

return 3. (Placing a bomb at (1,1) kills 3 enemies)
```

思路：

1. 最初想了一个从四个方向都统计一遍的方法，这样的话空间占用是4xMxN. 当然时间还是4xMxN的。 还是略愚蠢。

2. 后面这个方法， 只在遇到墙的时候发起统计。 空间占用是 ~N, 同时时间占用是严格的 2xMxN. 所以时间上也缩小了二分之一。

**solution 1**

```c++
class Solution {
public:
    int maxKilledEnemies(vector<vector<char>>& grid) {
        int rlen = grid.size(), res = 0;
        if (rlen == 0) return res;
        int clen = grid[0].size();
        vector<int> col(clen, 0);
        for (int i = 0; i < rlen; i++) {
            int row = 0;
            for (int j = 0; j < clen; j++) {
                if (grid[i][j] == 'W') continue;
                if (j == 0 || grid[i][j-1] == 'W') row = countRow(grid, i, j);
                if (i == 0 || grid[i-1][j] == 'W') col[j] = countCol(grid, i, j);
                if (grid[i][j] != 'E')             res = max(res, row+col[j]);
            }
        }
        return res;
    }
private:
    int countRow(vector<vector<char>>& grid, int i, int j) {
        int cnt = 0;
        while (j < grid[0].size()) {
            if (grid[i][j] == 'E') cnt++;    
            if (grid[i][j] == 'W') break;
            j++;
        }
        return cnt;
    }
    
    int countCol(vector<vector<char>>& grid, int i, int j) {
        int cnt = 0;
        while (i < grid.size()) {
            if (grid[i][j] == 'E') cnt++;
            if (grid[i][j] == 'W') break;
            i++;    
        }
        return cnt;
    }
};
```

