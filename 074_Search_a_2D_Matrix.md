### [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

Difficulty **Medium**

tags `array` `binary search`

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
For example,

Consider the following matrix:

```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```

Given target = `3`, return `true`.

**solution 1**
```c++
//  DON'T TREAT IT AS A 2D Matrix, JUST TREAT IT AS A Sorted Array!
//  log(m*n)

bool searchMatrix(vector<vector<int>>& matrix, int target) {
    if (matrix.size() == 0)    return false;
    int m = matrix.size();
    if (matrix[0].size() == 0) return false;
    int n = matrix[0].size();
    
    int h = 0, mid, t = m * n;
    
    while (h < t) {
        mid = h + (t - h >> 1);
        if (matrix[mid/n][mid%n] == target)     return true;
        else if (matrix[mid/n][mid%n] > target) t = mid;
        else h = mid + 1;
    }
    
    return false;
}
```