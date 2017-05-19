### [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

Difficulty **Medium**

tags `recursion` `backtracking` `array`

Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used once in the combination.

Note:
- All numbers (including target) will be positive integers.
- he solution set must not contain duplicate combinations.

For example, given candidate set `[10, 1, 2, 7, 6, 1, 5]` and target `8`,

A solution set is:
```
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**solution 1**
```c++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<int> val;
        vector<vector<int>> res;
        sort(candidates.begin(), candidates.end());
        recur(val, candidates.begin(), candidates, res, target);
        return res;
    }
private:
    void recur(vector<int> &val, vector<int>::iterator p, vector<int> &cad, vector<vector<int>> &res, int target) {
        if (target == 0) {res.push_back(val); return;}
        while (p != cad.end()) {
            if (*p > target) break;
            val.push_back(*p);
            recur(val, p+1, cad, res, target-*p);
            val.pop_back();
            p = upper_bound(cad.begin(), cad.end(), *p);
        }
    }
};
```

用递归, 是 039_Combination_Sum 的变体, 所以用`p+1`参数进入下一层, 这样就满足了题目的要求.
