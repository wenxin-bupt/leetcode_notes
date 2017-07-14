### [Number of Islands II](https://leetcode.com/problems/number-of-islands-ii/)

Difficulty **Hard**

tags `union find` 

A 2d grid map of `m` rows and `n` columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, **count the number of islands after each addLand operation.** An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example**:

Given `m = 3, n = 3`, `positions = [[0,0], [0,1], [1,2], [2,1]]`.
Initially, the 2d grid `grid` is filled with water. (Assume 0 represents water and 1 represents land).

```
0 0 0
0 0 0
0 0 0
```
Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.

```
1 0 0
0 0 0   Number of islands = 1
0 0 0
```

Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.

```
1 1 0
0 0 0   Number of islands = 1
0 0 0
```

Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.

```
1 1 0
0 0 1   Number of islands = 2
0 0 0
```

Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.

```
1 1 0
0 0 1   Number of islands = 3
0 1 0
```

We return the result as an array: [1, 1, 2, 3]

**Challenge:**

Can you do it in time complexity O(k log mn), where k is the length of the `positions`?

思路：

Coursera 的普林斯顿算法课的第一次作业, 直接回忆写了出来。 论坛中虽然有一维数组能提供更快(快50ms)的运行时间，但是时间复杂度并没有比我的有提升。

失误和要注意的Case：

1.  犯了如下代码中的严重错误, 找了很久才找到这个i, j更新问题。 *The Practice of Programming* 中也提到了这个问题。 
```c++
while (map[i][j][0] != i || map[i][j][1] != j) {
            int i = map[i][j][0];
            int j = map[i][j][1];
}
```

2. 同样是在*findCluster()*中, 要使用**path compression**来提升之后的**查找效率**。 查找越多，提升越显著。

3. 这种初始化vector的方法明显比所谓的简便写法快。
```c++
void initMap(int m, int n) {
    for (int i = 0; i < m; i++) {
        vector<vector<int>> row;
        map.push_back(row);
        for (int j = 0; j < n; j++){ 
           map[i].push_back({i, j, 0});
        }
    }
}
```




**solution 1**

```c++
class Solution {
    vector<vector<vector<int>>> map;
    int cnt;
    void initMap(int m, int n) {
        for (int i = 0; i < m; i++) {
            vector<vector<int>> row;
            map.push_back(row);
            for (int j = 0; j < n; j++){ 
               map[i].push_back({i, j, 0});
            }
        }
    }
    
    vector<int> findCluster(int i, int j) {
        while (map[i][j][0] != i || map[i][j][1] != j) {
            int n_i = map[i][j][0];
            int n_j = map[i][j][1];
            
            // path compression
            map[i][j][0] = n_i;
            map[i][j][1] = n_j;

            i = n_i;
            j = n_j;
        }
        return map[i][j];
    }
    
    void mergeCluster(vector<int> c1, vector<int> c2) {
        if (c1[2] >= c2[2]) {
            map[c1[0]][c1[1]][2] = c1[2] + c2[2];
            map[c2[0]][c2[1]] = map[c1[0]][c1[1]];
        } else {
            map[c2[0]][c2[1]][2] = c1[2] + c2[2];
            map[c1[0]][c1[1]] = map[c2[0]][c2[1]];
        }
    }
    
    bool isInOneCluster(vector<int> c1, vector<int> c2) {
        if (c1[0] == c2[0] && c1[1] == c2[1])
            return true;
        return false;
    }
    
public:
    vector<int> numIslands2(int m, int n, vector<pair<int, int>>& positions) {
        vector<int> res;
        initMap(m, n);
        for (pair<int, int> p : positions) {
            int i = p.first, j = p.second;
            if (map[i][j][2] > 0) continue;
            map[i][j][2] = 1;
            cnt++;
            if (i-1>=0 && map[i-1][j][2] > 0) {
                vector<int> c1 =  findCluster(i, j);
                vector<int> c2 =  findCluster(i-1, j);
                if (!isInOneCluster(c1, c2)) {
                    mergeCluster(c1, c2);
                    cnt--;
                }
            }
            if (j-1>=0 && map[i][j-1][2] > 0) {
                vector<int> c1 =  findCluster(i, j);
                vector<int> c2 =  findCluster(i, j-1);
                if (!isInOneCluster(c1, c2)) {
                    mergeCluster(c1, c2);
                    cnt--;
                }
            }
            if (j+1<n && map[i][j+1][2] > 0) {
                vector<int> c1 =  findCluster(i, j);
                vector<int> c2 =  findCluster(i, j+1);
                if (!isInOneCluster(c1, c2)) {
                    mergeCluster(c1, c2);
                    cnt--;
                }
            }
            if (i+1<m && map[i+1][j][2] > 0) {
                vector<int> c1 =  findCluster(i, j);
                vector<int> c2 =  findCluster(i+1, j);
                if (!isInOneCluster(c1, c2)) {
                    mergeCluster(c1, c2);
                    cnt--;
                }
            }
            res.push_back(cnt);
        }
        return res;
    }
};
```
