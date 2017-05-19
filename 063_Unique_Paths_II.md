### [Unique Paths](https://leetcode.com/problems/unique-paths/)

Difficulty **Medium**

tags `array` `vector` `Dynamic Programming` `DP`

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,

There is one obstacle in the middle of a 3x3 grid as illustrated below.
```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```
The total number of unique paths is `2`.


**solution 1**
```c++
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
    if (obstacleGrid.empty() || obstacleGrid[0].empty()) return 0;
    int m = obstacleGrid.size(), n = obstacleGrid[0].size();
    vector<int> paths(n, 0);
    paths[0] = 1;
    for (int i = 0; i < m; i++) {
        if (obstacleGrid[i][0]==1) paths[0] = 0;
        for (int j = 1; j < n; j++) {
            if (obstacleGrid[i][j]==1) paths[j] = 0;
            else paths[j] = paths[j] + paths[j-1];
        }
    }
    return paths[n-1];
}
```
基于062_Unique_Paths 的方法, 稍作修改以适应条件.
