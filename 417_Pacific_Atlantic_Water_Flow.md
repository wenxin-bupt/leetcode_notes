### [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

Difficulty **Medium**

tags `DFS` `island`

Given an *m x n* matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions (up, down, left, or right) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

**Note:**

The order of returned grid coordinates does not matter.
Both m and n are less than 150.

**Example:**

``` 
Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).
```

思路：

DFS即可，下面的我的解法虽然节省了空间但是没有可读性， 显晦涩。 不推荐。

**solution 1**
```c++
class Solution {
    void dfs_spread(vector<pair<int, int>> &res, vector<vector<int>>& matrix, int i, int j, bool round) {
        static vector<vector<int>> dirs = {{-1,0}, {1,0}, {0,1}, {0,-1}};
        int t = matrix[i][j];
        if (round) {
            if (matrix[i][j] == 0) return;
            if (matrix[i][j] < 0) {
                res.push_back(make_pair(i, j));
            }
            matrix[i][j] = 0;
        } else {
            if (matrix[i][j] < 0) return;
            matrix[i][j] = -matrix[i][j];
        }
        for (vector<int> dir : dirs) {
            if (i+dir[0]>=0 && i+dir[0]<matrix.size() && j+dir[1]>=0 && j+dir[1]<matrix[0].size()) {
                if (round) {
                    if (matrix[i+dir[0]][j+dir[1]] != 0 && abs(matrix[i+dir[0]][j+dir[1]]) >= abs(t))
                        dfs_spread(res, matrix, i+dir[0], j+dir[1], round);
                } else {
                    if (matrix[i+dir[0]][j+dir[1]] > 0 && matrix[i+dir[0]][j+dir[1]] >= t)
                        dfs_spread(res, matrix, i+dir[0], j+dir[1], round);
                }
            }
        }
    }
public:
    vector<pair<int, int>> pacificAtlantic(vector<vector<int>>& matrix) {
        vector<pair<int, int>> res;
        if (matrix.size() == 0) return res;
        for (int i=0; i<matrix.size(); i++) {
            for (int j=0; j<matrix[0].size(); j++) {
                matrix[i][j]++;
            }
        }
        for (int i=0; i<matrix.size(); i++) 
            dfs_spread(res, matrix, i, 0, false);
        for (int j=1; j<matrix[0].size(); j++)
            dfs_spread(res, matrix, 0, j, false);
        for (int i=0; i<matrix.size(); i++) 
            dfs_spread(res, matrix, i, matrix[0].size()-1, true);
        for (int j=matrix[0].size()-2; j>=0; j--) 
            dfs_spread(res, matrix, matrix.size()-1, j, true);
        return res;
    }
};
```
