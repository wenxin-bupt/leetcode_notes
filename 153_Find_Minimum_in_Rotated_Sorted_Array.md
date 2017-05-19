### [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

Difficulty **Medium**

tags `vector` `binary search` `array`

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

Find the minimum element.

You may assume no duplicate exists in the array.

**solution 1**
```c++
int findMin(vector<int>& nums) {
    int h = 0, t = nums.size()-1, m;
    while (nums[h]>nums[t]) {
        m = (h+t) >> 1;
        if (nums[m] > nums[t]) h = m+1;
        else t = m;
    }
    return nums[h];
}
```

我们在 033_Search_in_Rotated_Sorted_Array 里面见过面~ 这里`nums[h]>nums[t]` 作为判别条件, 可以让查找更快结束~

**solution 2**
```c++
int findMin(vector<int>& nums) {
    int h = 0, t = nums.size()-1, m;
    while (h<t) {
        m = (h+t) >> 1;
        if (nums[m] > nums[t]) h = m+1;
        else t = m;
    }
    return nums[h];
}
```

classical version 2333
