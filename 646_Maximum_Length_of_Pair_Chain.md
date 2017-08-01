### [Maximum Length of Pair Chain](https://leetcode.com/problems/maximum-length-of-pair-chain/)

Difficulty **Medium**

tags `greedy` 

You are given n pairs of numbers. In every pair, the first number is always smaller than the second number.

Now, we define a pair (c, d) can follow another pair (a, b) if and only if b < c. Chain of pairs can be formed in this fashion.

Given a set of pairs, find the length longest chain which can be formed. You needn't use up all the given pairs. You can select pairs in any order.

**Example 1:**
```
Input: [[1,2], [2,3], [3,4]]
Output: 2
Explanation: The longest chain is [1,2] -> [3,4]
```

**Note:**

1. The number of given pairs will be in the range [1, 1000].

思路：

将pair按后一位进行排序以后，进行贪婪选取即可。

正确性证明如下：

将整个pair数组按照后一位排序以后，设后一位最大的值为Pn, 那么我们可以将这个问题转述为: **求以<=Pn的值结尾的最长的Pair链的长度**  接下来我们用归纳法证明：

1. 设最小的Pair后一位的值为P0, 那么我们易知以<=P0结尾的最长Pair链的长度为1。  
2. 设第k小的Pair后一位的值为Pk， 且以<=Pk结尾的Pair链的最大长度为L， 最长链上最后一个Pair为(_,P), P<=Pk。 

    我们考虑k+1的情况， 对于以<=Pk+1结尾的Pair链的最大长度， 如果存在一个Pair（Q, Pk+1),  Q > P. 那么(Q, Pk+1)可以扩展以<=Pk结尾的Pair链， 最大长度即为L+1。 如果不存在以Pk+1结尾的Pair满足以上条件， 那么最大长度就无法扩展， 维持在L。

根据上面的归纳过程，迭代知道Pn即可得到<=Pn的值结尾的最长的Pair链的长度， 也即本题的答案。


**solution 1**
```c++
class Solution {
public:
    int findLongestChain(vector<vector<int>>& pairs) {
        sort(pairs.begin(), pairs.end(), [](vector<int> f, vector<int> s){return f[1] < s[1];});
        int res = 1, last = 0;
        for (int i = 0; i < pairs.size(); i++) {
            if (pairs[i][0] > pairs[last][1]) {
                last = i;
                res++;
            }
        }
        return res;
    }
};
```

