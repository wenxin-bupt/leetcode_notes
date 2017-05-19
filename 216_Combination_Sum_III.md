### [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

Difficulty **Medium**

tags `recursion` `backtracking` `array`

Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.


Example 1:

Input: k = 3, n = 7

Output:

`
[[1,2,4]]
`

Example 2:

Input: k = 3, n = 9

Output:

`
[[1,2,6], [1,3,5], [2,3,4]]
`

**solution 1**
```c++
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> res;
        vector<int> vals;
        this->k = k;
        recur(res, vals, 1, n);
        return res;
    }

private:
    int k;
    void recur(vector<vector<int>> &res, vector<int> &vals, int now, int target) {
        if (vals.size()==k) {
            if (target==0) res.push_back(vals);
            return;
        }
        while (now<10 && now<=target) {
            vals.push_back(now);
            recur(res, vals, now+1, target-now);
            vals.pop_back();
            now++;
        }
    }
};
```
是I, II, III的变体, 稍加修改就可以辣.
