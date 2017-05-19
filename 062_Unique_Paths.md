### [Unique Paths](https://leetcode.com/problems/unique-paths/)

Difficulty **Medium**

tags `array` `vector` `Dynamic Programming` `DP`

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**solution 1**
```c++
int uniquePaths(int m, int n) {
    if (m<1 || n<1) return 0;  // test case 中的m,n都是大于1的. 但是, 显然官方的测试例子里面是考虑了这个哒.
    vector<int> paths(m, 1);
    for (int i = 1; i < n; i++)
        for (int j = 1; j < m; j++)
            paths[j] = paths[j-1] + paths[j];
    return paths[m-1];
}
```
在写出答案的时候并没有想到这个是DP的, 因为在搜狗做的就是酱紫的事情嘛23333 很熟练地写了出来.

为什么会用到DP: 考虑[m, n]作为终点(m>1, n>1), 通向它的路径根据规则只能来自两个方向[m-1, n]和[m, n-1], 所以自然是这两者的和.
