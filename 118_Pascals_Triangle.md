### [Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)   

Difficulty **Easy**

tags `vector`

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return
```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
**solution 1**
```c++
vector<vector<int>> generate(int numRows) {
   vector<vector<int>> r;
   if(!numRows) return r;
   r.resize(numRows);
   for (int i = 0; i < numRows; i++) {
       r[i].resize(i+1);
       r[i][0] = r[i][i] = 1;
       for (int k = 1; k < i/2+1; k++) r[i][i-k] = r[i][k] = r[i-1][k-1] + r[i-1][k];
   }
   return r;
}
```
`resize()`方法后不需要再进行`push_back()`. `resize()`方法在这里要比`reserve()`快.


还可以在外面维护一个数组! 来做这个事情!
```

``
