### [Single Number](https://leetcode.com/problems/single-number/)

Difficulty **Easy**

tags `hash` `Bit Manipulation`

Given an array of integers, every element appears twice except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**solution 1**
```c++
int singleNumber(vector<int>& nums) {
    int res = 0;
    for (int i = 0; i < nums.size(); i++) res ^= nums[i];
    return res;
}
```

深刻的感觉是，如果不做好收集整理，人还是会踏入同一条河流的。 这道面试题竟然已经被我完全忘却了，我觉得它至少杀了我两个小时的时间。
