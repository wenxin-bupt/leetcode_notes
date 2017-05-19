### [Search Insert Position](https://leetcode.com/problems/search-insert-position/?tab=Description)

Difficulty **Easy**

tags `binary search` `array`

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.

`[1,3,5,6]`, 5 → 2

`[1,3,5,6]`, 2 → 1

`[1,3,5,6]`, 7 → 4

`[1,3,5,6]`, 0 → 0

**solution 1**
```c++
// assume that there are no duplicates

int searchInsert(vector<int>& nums, int target) {
    int h = 0, t = nums.size(), m =  h + ((t - h)>> 1);
    while (h < t) {
        m = h + ((t - h)>> 1);
        if (nums[m] > target) t = m;
        else if (nums[m] < target) h = m+1;
        else break;
    }
    return m;  
}
```

**solution 2**
```c++
// works whether there were duplicates. Always return the left most one. 
// 相等和大于放在一起处理。
int searchInsert(vector<int>& nums, int target) {
    int h = 0, t = nums.size(), m;
    while (h < t) {
        m = h + ((t - h)>> 1);
        if (nums[m] < target) h = m + 1;
        else  t = m;
    }
    return h;  
}
```
