### [Search for a Range](https://leetcode.com/problems/search-for-a-range/)

Difficulty **Medium**

tags `binary search` `array`

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return `[-1, -1]`.

For example,

Given `[5, 7, 7, 8, 8, 10]` and target value 8,

return `[3, 4]`.

**solution 1**
```c++
// my solution
vector<int> searchRange(vector<int>& nums, int target) {
    int h = 0, t = nums.size(), m;   
    // 定位后h位于第一个大于等于target的位置， h和t位置相同
    while (h < t) {
        m = h + (t - h >> 1); 
        if (nums[m] < target) h = m + 1;
        else t = m;
    }
    // h定位失败，则在数组中不存在解.
    if (nums.size() == 0 || h >= nums.size() || nums[h] != target) 
        return {-1, -1};
    int l = h, r;
    t = nums.size(); // 只需要修改t即可，h位置固定.
    // 定位后t位于第一个大于target的位置， h和t位置相同
    while (h < t) {
       m = h + (t - h >> 1);
       if (nums[m] > target) t = m;
       else h = m + 1;
    }
    r = t - 1;
    return {l, r};
}
```