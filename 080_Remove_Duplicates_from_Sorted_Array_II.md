### [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

Difficulty **Medium**

tags `two-pointers` `array` `vector`

Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,

Given sorted array nums = `[1,1,1,2,2,3]`,

Your function should return length = `5`, with the first five elements of nums being  `1`, `1`, `2`, `2` and `3`. It doesn't matter what you leave beyond the new length.

**solution 1**
```c++
int removeDuplicates(vector<int>& nums) {
    int i = 0;
    for (int n : nums)
        if (i < 2 || n > nums[i-2])
            nums[i++] = n;
    return i;
}
```
因为是已经排序的, 所以直接遍历一遍, 利用排序的情况就可以惹.

看到这种写法, 认为 for (:) 对vector的遍历是顺序的.


**solution 2**
```c++
int removeDuplicates(vector<int>& nums) {
    int q = 0, p = 1;
    if (nums.size() == 0) return 0;
    while (p < nums.size()) {
        if (nums[p]==nums[q]) {
            nums[++q] = nums[p++];
            while (p<nums.size() && nums[p]==nums[q]) p++;
        } else nums[++q] = nums[p++];
    }
    return q+1;
}
```

kid's stuff
