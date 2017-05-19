### [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

Difficulty **Easy**

tags `two-pointers`

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array *nums* = `[1,1,2]`,

Your function should return length = `2`, with the first two elements of nums being `1` and `2` respectively. It doesn't matter what you leave beyond the new length.

**solution 1**
```c++
int removeDuplicates(vector<int>& nums) {
   return unique(nums.begin(), nums.end()) - nums.begin();
}
```
unique 会返回一个指向unique后的[first, last)中的last的迭代器，所以只要减去就可以啦。

**solution 2**
```c++
int removeDuplicates(vector<int>& nums) {
  int count = 0, n = nums.size();
  for(int i = 1; i < n; i++){
      if(nums[i] == nums[i-1]) count++;
      else nums[i-count] = nums[i];
  }
  return n-count;
}
```
quite concise solution
