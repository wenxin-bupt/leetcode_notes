### [Range Sum Query 2D - Mutable](https://leetcode.com/problems/range-sum-query-2d-mutable/)

Difficulty **hard**

tags `binary_indexed_tree`

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

Range Sum Query 2D

![](http://oq5gcgc8d.bkt.gdipper.com/308_Range_Sum_Query_2D_Mutablepic1.png)

The above rectangle (with the red border) is defined by (row1, col1) = (2, 1) and (row2, col2) = (4, 3), which contains sum = 8.

Example:
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```
**Note**:

- The matrix is only modifiable by the update function.
- You may assume the number of calls to update and sumRegion function is distributed evenly.
- You may assume that row1 ≤ row2 and col1 ≤ col2.

思路：

1. 肯定不能暴力写。所以自己写了一个按行来进行累加和的版本，对这道题的case比binary indexed tree都快。

2. 学习了binary indexed tree. 然后写了一篇[Blog](http://www.cnblogs.com/whensean/p/Binary_Indexed_Tree.html), 体会到了给别人讲授知识反而可以理清自己的知识。写博客的习惯还是要坚持的， 就是要控制时间利用2333

    注意，这是binary indexed tree的多维应用.

3. segment tree如何实现，需要重新学习下。 上次用的时候最起码是半年前了，完全忘却。

4. 还是老话题需要熟悉下STL的API了， 毕竟这种写法能省很多功夫和时间：
```c++
orimatrix = vector<vector<int>>(rlen, vector<int>(clen, 0));
```

    或者考察下实现机制，如果明确知道数组定义之后&未人为初始化时-->里面的值是0的话, 就直接用。

既然折腾了一下binary indexed tree, 那么答案就还是bit版本吧。

**solution 1**

```c++
class NumMatrix {
public:
    NumMatrix(vector<vector<int>> matrix) {
        rlen = matrix.size();
        if (rlen) clen = matrix[0].size();

        orimatrix = vector<vector<int>>(rlen, vector<int>(clen, 0));
        bitmatrix = vector<vector<int>>(rlen, vector<int>(clen, 0));

        for (int i = 0; i < rlen; i++)
            for (int j = 0; j < clen; j++)
                update(i, j, matrix[i][j]);

    }

    void update(int row, int col, int val) {
        int change = val - orimatrix[row][col];
        orimatrix[row][col] = val;
        row++;
        col++;
        for (int i = row; i <= rlen; i += i&(-i))
            for (int j = col; j <= clen; j += j&(-j))
                bitmatrix[i-1][j-1] += change;
    }

    int sum(int row, int col){
        int sum = 0;
        row++;
        col++;
        for (int i = row; i > 0; i -= i&(-i))
            for (int j = col; j > 0; j -= j&(-j))
               sum += bitmatrix[i-1][j-1];
        return sum;
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return sum(row2, col2) - sum(row2, col1-1) - sum(row1-1, col2) + sum(row1-1, col1-1);
    }

private:
    vector<vector<int>> bitmatrix;
    vector<vector<int>> orimatrix;
    int rlen, clen;
};
```
