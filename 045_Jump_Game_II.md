### [Jump Game II](https://leetcode.com/problems/jump-game-ii/)

Difficulty **Hard**

tags `greedy` `array` `vector`

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

For example:
Given array A = `[2,3,1,1,4]`

The minimum number of jumps to reach the last index is `2`. (Jump `1` step from index `0` to `1`, then `3` steps to the last index.)

**solution 1**
```c++
int jump(vector<int>& nums) {
    int reach = 0, ptr = 0, stp = 0, next_reach;
    while(reach < nums.size() - 1) {
        next_reach = reach;
        for (; ptr <= reach; ptr++)
            if (nums[ptr]+ptr > next_reach)
                next_reach = nums[ptr] + ptr;
        if (next_reach == reach) return -1;  // 没有继续扩展意味着无法达到尾部, 跳出.
        reach = next_reach;
        stp++;
    }
    return stp;
}
```
这是一个~n的解法.  

在055_Jump_Game发现的是, 能否到达尾部, 能否继续扩展取决于目前reach中的元素.

这里我们发现, 最小步数是单调递增的. 在一个reach中的最小步数都是相等的. 所以这里再次变成扩展reach的问题.

这样就把一个~N^2的问题缩减成~N的问题.

**solution 2**
```c++
int jump(vector<int>& nums) {
    vector<int> steps(nums.size(), INT_MAX);
    steps[0] = 0;
    int last = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (i + nums[i] > last) { // 加入last防止重复遍历
            for (int j = last+1; j < nums.size() && j <= i + nums[i]; j++) {
                if (steps[i]+1 < steps[j]) steps[j] = steps[i]+1;
                if (j == nums.size()-1) return steps[j];
            }
            last = i + nums[i];
        }
    }
    return steps[nums.size()-1];
}
```
naive solution. 这是最初的算法, 通过加了一个last把~N^2变成了~N. 但是, 并不需要使用step数组来存step
