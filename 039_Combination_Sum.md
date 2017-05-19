### [Combination Sum](https://leetcode.com/problems/combination-sum/)

Difficulty **Medium**

tags `recursion` `backtracking` `array`

Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:
- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

For example, given candidate set `[2, 3, 6, 7]` and target `7`,

A solution set is:
```
[
  [7],
  [2, 2, 3]
]
```

**solution 1**
```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> val, key;
        vector<vector<int>> res;
        sort(candidates.begin(), candidates.end());
        recur(val, key, candidates, res, 0, target);
        return res;
    }
private:
    void recur(vector<int> &val, vector<int> &key, vector<int> &cad, vector<vector<int>> &res, int sum, int target) {
        int t = target - sum;
        if (val.size()>0 && t<val[val.size()-1]) return;
        if (binary_search(cad.begin(), cad.end(), t)) {
            val.push_back(t);
            res.push_back(val);
            val.pop_back();
        }
        auto p = cad.begin();
        if (val.size()>0) p = cad.begin()+key[key.size()-1];
        while (p != cad.end()) {
            if (sum+*p > target) break;
            key.push_back(p-cad.begin()); val.push_back(*p);
            recur(val, key, cad, res, sum+*p, target);
            key.pop_back(); val.pop_back();
            p = upper_bound(cad.begin(), cad.end(), *p);
        }
    }
};
```
这里使用递归(backtracking?), 来做. 这里要求给出各种长度的排列, 显然不使用递归是不能完成任务哒.

solution 1 是最初始的版本. 基本思想是每一层递归, 先找在这层中有没有可以满足target的数. 如果有, 就把它列入结果中. 对剩下的其他数, 满足递归条件的放入下一层递归.

`upper_bound`返回的是在vector中大于当前寻找元素的第一个元素的`iterator`.

1. 没有必要设一个sum项, 每层修改target值就好. sum项冗余.
2. 没有必要保存所有的key, 其实只和上一次用到的key有关联, key不需要vec保存.
3. 把能加入res的和满足进入下一层条件的数字统一, 可以减少判断逻辑.

**solution 2**

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> val;
        vector<vector<int>> res;
        sort(candidates.begin(), candidates.end());

        unique(candidates.begin(), candidates.end()));
        recur(val, candidates.begin(), candidates, res, target);
        return res;
    }
private:
    void recur(vector<int> &val, vector<int>::iterator p, vector<int> &cad, vector<vector<int>> &res, int target) {
        if (target == 0) {res.push_back(val); return;}
        while (p != cad.end()) {
            if (*p > target) break;
            val.push_back(*p);
            recur(val, p, cad, res, target-*p);
            val.pop_back();
            p = upper_bound(cad.begin(), cad.end(), *p);
        }
    }
};

```
这里使用了`vector<int>::iterator`作为层之间传递的参数. 这样在很多重复的情况下可以很快递进到目标(喂, 那你为什么不先unique!!! 见solution 3).

upper_bound的代码有趣,值得看下

每一层都首先判断是否满足情况, 否则递进到下一层. 这里的元素是可以重复的, 所以我们每次都从元素自己开始, 把这一层的元素导入下一层递归. break的条件是当前元素比target大, 就不再把它扔到下一层递归了.

**solution 3**

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> val;
        vector<vector<int>> res;
        sort(candidates.begin(), candidates.end());
        auto it = unique(candidates.begin(), candidates.end());
        candidates.resize(it - candidates.begin());
        recur(val, candidates.begin(), candidates, res, target);
        return res;
    }
private:
    void recur(vector<int> &val, vector<int>::iterator p, vector<int> &cad, vector<vector<int>> &res, int target) {
        if (target == 0) {res.push_back(val); return;}
        while (p != cad.end()) {
            if (*p > target) break;
            val.push_back(*p);
            recur(val, p, cad, res, target-*p);
            val.pop_back();
            p++;
        }
    }
};
```

额, 在愚蠢过后是比较合适的解法惹! 这个解法是最快的!

unique的作用是去除数组中重复元素区间上除第一个元素之外的重复的元素.  所以和sort搭配食用的作用是去除所有的重复元素.

沾沾自喜的时候往往后面是大大的错误!
