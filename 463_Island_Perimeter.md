### [Island Perimeter](https://leetcode.com/problems/island-perimeter/description/)

Diffculty **Easy**

tags `island`

You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

Example:
```
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

Answer: 16
```
Explanation: The perimeter is the 16 yellow stripes in the image below:

![](http://oq5gcgc8d.bkt.gdipper.com/463_Island_Perimeter/island.png)

要注意的case:

**一个break只能break一个for循环**

思路：

DFS也可以， 但是这种方法是经过观察的可行的方法。


**solution 1**
```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int count=0, neighbor=0;
        for (int i=0; i<grid.size(); i++) {
            for (int j=0; j<grid[0].size(); j++){
                if (grid[i][j]) {
                    count++;
                    if (i>0 && grid[i-1][j]) neighbor++;
                    if (j>0 && grid[i][j-1]) neighbor++;
                }
            }
        }
        return count*4 - neighbor*2;
    }
};
```
