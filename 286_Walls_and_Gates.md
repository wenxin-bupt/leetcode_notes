### [Walls and Gates](https://leetcode.com/problems/walls-and-gates/description/)

Difficulty **Medium**

tags `BFS` `island`

You are given a m x n 2D grid initialized with these three possible values.

1. `-1` - A wall or an obstacle.
2. `0` - A gate.
3. `INF` - Infinity means an empty room. We use the value `2^31 - 1 = 2147483647` to represent INF as you may assume that the distance to a gate is less than `2147483647`.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

For example, given the 2D grid:

```
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```

After running your function, the 2D grid should be:

```
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```
思路：

容易想到分别从不同的gate开始进行BFS， 自然就可以得到最优距离。 这里的优化是**同时**从所有的gate开始BFS。 

**solution 1**
```c++
class Solution {
public:
    void wallsAndGates(vector<vector<int>>& rooms) {
        queue<vector<int>> q;
        vector<vector<int>> ds = {{-1,0}, {1, 0}, {0,1}, {0,-1}};
        for (int i=0; i<rooms.size(); i++) {
            for (int j=0; j<rooms[i].size(); j++) {
                if (rooms[i][j]==0) {
                    q.push({i, j});
                }
            }
        }
        while (!q.empty()) {
            auto p = q.front();
            q.pop();
            for (auto d : ds) {
                int nx = p[0]+d[0], ny = p[1]+d[1];
                if (nx>=0 && nx<rooms.size() && ny>=0 && ny<rooms[0].size() && rooms[nx][ny]==INT_MAX) {
                    rooms[nx][ny] = rooms[p[0]][p[1]] + 1;
                    q.push({nx, ny});
                }
            }
        }
    }
};
```
