### [3Sum Closest](https://leetcode.com/problems/3sum-closest/)

Difficulty **Medium**

tags `vector` `two-pointers` `array` `k-sum`

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
```
For example, given array S = {-1 2 1 -4}, and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**solution 1**
```c++
int threeSumClosest(vector<int>& nums, int target) {
    vector<vector<int>> result;
    int sum, front, back, cache, min = nums[0] + nums[1] + nums[2];
    int length = nums.size();
    sort(nums.begin(), nums.end());
    for (int i=0; i<length; i++) {
        front  = i + 1;
        back   = length - 1;
        while (front<back) {
            sum = nums[i] + nums[front] + nums[back];
            if (abs(target-sum) < abs(target-min)) min = sum;
            if (sum>target) {
                cache = nums[back];
                while (front<back && nums[back] == cache)  back--;
            } else if (sum<target) {
                cache = nums[front];
                while (front<back && nums[front]== cache) front++;
            }
            else return sum;
        }
        while(i<length-1 && nums[i+1]==nums[i])  i++;		
    }
    return min;
}   
```
用3sum中的双指针稍加修改就可以胜任工作~  

space cost `inplace`  time cost `~N^2`

已经进行了充分的规避重复元素来减少运算. 经过小范围实验发现没有能在中途制止两个指针收缩的条件, 必须把数组都撸一遍才可以获取到所有可能的取值, 酱紫得到目标中的最小值.

考虑过使用multimap, 这里的情况需要穷举, 所以用upper_bound和lower_bound并不能满足条件 :-/
