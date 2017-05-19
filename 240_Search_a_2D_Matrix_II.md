### [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

Difficulty **Medium**

tags `binary search` `vector` `array` `devide and conquer`

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted in ascending from left to right.

Integers in each column are sorted in ascending from top to bottom.

For example, Consider the following matrix:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
Given target = 5, return true.

Given target = 20, return false.


**solution 1**
```c++
// beautiful solution

// O(m+n)的解法 基于对数组的充分观察得到
// 为什么是从0，n-1的位置开始？
// 这样经过的路径刚好是在矩阵中以target为界的一条分界线
// 如果从0,0开始，就是发散型的，下一步是+row 还是 +col是不确定的
// 如果从0,n-1或者n-1, 0开始， 是收敛的, 即row和col的加减是反着的
// 这样最长的路径长度就是没有找到的情况即： m+n-1或者n-1

bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    if (m==0) return false;
    int n = matrix[0].size();
    if (n==0) return false;
    
    int row = 0, col = n - 1;
    while (row < m && col >= 0) {
        if (matrix[row][col] == target) return true;
        else if (matrix[row][col] < target) row++;
        else col--;
    }
    return false;
}
```

**solution 2**
```c++
// 这是我非常naive的算法，考虑到要解决的问题是一个排列有顺序的矩阵
// 考虑到有顺序就使用二分搜索好了。 于是想到了平面二分搜索的方法， 即沿着斜率为1的对角线搜索。
// 即使对于长和宽不同的情况，也是沿着对角线搜索。然后再处理，搜索之后剩余的区块， 进行递归搜索处理。
// 难以直接估算时间复杂度， 结果上比solution1的用时多一倍。
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0) return false;
        int m, n;
        m = matrix.size();
        n = matrix[0].size();
        return areaSearch(matrix, target, 0, 0, m, n);
    }
private: 
    bool areaSearch(vector<vector<int>>& matrix, int target, int x1, int y1, int x2, int y2) {
        if (x1 == x2 || y1 == y2) return false;
        int h = 0, t = min(x2 - x1, y2 - y1), mid;
        while (h < t) {
            mid = h + (t - h >> 1);
            if (matrix[x1+mid][y1+mid]==target) return true;
            else if(matrix[x1+mid][y1+mid]>target) t = mid;
            else h = mid + 1;
        }
        return areaSearch(matrix, target, x1+h, y1, x2, y1+h) || areaSearch(matrix, target, x1, y1+h, x1+h, y2); 
    }
};
```

