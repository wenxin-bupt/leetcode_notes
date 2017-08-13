### [Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

Difficulty **Medium**

tags `DP` 

Given a positive integer n, find the least number of perfect square numbers (for example, `1, 4, 9, 16, ...`) which sum to n.

For example, given *n* = `12`, return `3` because `12 = 4 + 4 + 4`; given *n* = `13`, return `2` because `13 = 4 + 9`.

思路：

容易想到*n*的平方序列可以由两个数i,j且i+j=n用各自的平方序列相加而成。 但是这样的话， 每更新一次n， 就需要进行n/2 (+1 if odd)次相加。

优化的思路是，*n*总由*1~n-1*之间的数再加上一个新的平方数而成。 那么我们需要考虑的可能就被缩小为"sqrt(n)"级别。 这算是一个很大的提升。

trick:

用static变量避免重复生成数组带来的开销。 

**solution 1**
```c++
class Solution {
public:
    int numSquares(int n) {
        static vector<int> v = {0};
        int s = v.size();
        if (s>=n+1) return v[n];
        for(int i=s; i<=n; i++) {
            int m = INT_MAX;
            for(int j=1; j*j<=i; j++) {
                m = min(m, v[i-j*j]+1);
            }
            v.push_back(m);
        }
        return v[n];
    }
};
```
