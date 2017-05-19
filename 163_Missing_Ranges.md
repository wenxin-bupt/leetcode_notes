### [Missing Ranges](https://leetcode.com/problems/missing-ranges/)

Difficulty **Medium**

tags `array`

Given a sorted integer array where **the range of elements are in the inclusive range [lower, upper]**, return its missing ranges.

For example, given `[0, 1, 3, 50, 75]`, *lower* = 0 and *upper* = 99, return `["2", "4->49", "51->74", "76->99"]`.


未考虑到的corner case

1. lower 和 upper 是可以相等的，没有考虑到
2. 在程序中出现了很多的int相加减。在题目没有明确告知范围的情况下，我没有考虑到溢出的问题

代码的优美程度

1. 最早通过的代码没有较好地提炼出一个通用的模型。把头尾的两种情况分别提出来处理。 而后的这个版本考虑在lower之前加一个lower-1, 在upper之后加一个upper+1。从而数组中的数字可以统一处理

语法问题：

1. class 定义之后要加分号

**solution 1**
```c++
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        long pre, cur, lowerL = lower, upperL = upper;
        int p = 0;
        vector<string> res;
        pre = lowerL - 1;
        while (p <= nums.size()){
            cur = p==nums.size() ? upperL + 1 : nums[p];
            if (cur - pre == 2) res.push_back(to_string(pre + 1));
            if (cur - pre > 2)  
                res.push_back(to_string(pre + 1) + "->" + to_string(cur - 1));
            pre = cur;
            p++;
        }
        return res;
    }

};
```
