### [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

Difficulty **Hard**

tags `vector` `binary search` `array`

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

Find the minimum element.

The array may contain duplicates.

**solution 1**
```c++
int findMin(vector<int>& nums) {
    int h = 0, t = nums.size()-1, m;
    while (h<t && nums[h]>=nums[t]) {
        m = h + (t-h) >> 1;
        if (nums[m] == nums[h]) {
            while (nums[h] == nums[m] && h<t) ++h;
        } else if (nums[m] > nums[h]) {
            h = m+1;
        } else {
            t = m;
        }
    }
    return nums[h];
}
```
对 153 解法的发展,  在有重复元素的情况下, 保持二分搜索. 要排除一部分重复元素.
思路是考虑h, t分属于两个group,  因为要寻找的在t所在的group. 这样知道h进入t的group的时候, h所在的点就是最小的点.

面临了的corner case:
1. 数组中只有一个元素的情况
2. 数组本身是正序的情况

所以设了 h<t && nums[h]>=nums[t] 规避Corner case

**solution 2**
```c++
int findMin(vector<int>& nums) {
    int h = 0, t = nums.size()-1, m;
    while (h<t) {
        m = h + (t-h) >> 1;
        if (nums[m] == nums[t]) {
            t--;
        } else if (nums[m] > nums[t]) {
            h = m+1;
        } else {
            t = m;
        }
    }
    return nums[h];
}
```

这种方法相比于solution 1, 看起来更neat! 是153解法的发展~


**binary search bug**
(h+t) >> 1 will cause overflow sometimes. Here using h + (t-h) >> 1 is great to do this.
