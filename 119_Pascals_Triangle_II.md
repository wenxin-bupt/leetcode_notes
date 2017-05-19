### [Pascals's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)

Difficulty **Easy**

tags `vector`

Given an index k, return the kth row of the Pascal's triangle.

For example, given *k = 3*,

Return `[1,3,3,1]`.

**solution 1**
```c++
vector<int> getRow(int rowIndex) {
    vector<int> p;
    p.resize(rowIndex+1);
    for (int i = 0; i <= rowIndex; p[i++] = 1)
        for (int k = i-1; k > 0; k--)
            p[k] = p[k] + p[k-1];
    return p;
}
```
naive 想法自然是用两个数组轮番替换(用vector的swap). 这样需要`~n`的辅助空间. 研究后发现可以原地完成, 但是要从尾到头进行遍历. 然后再对代码优化一下(喂, 我不知道过几天你看不看得懂2333

看到了别人用lamada表达式写的算法, 真的好短!
