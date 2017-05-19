### [Remove Element](https://leetcode.com/problems/remove-element/)   

Difficulty **Easy**

tags `array` ` vector` `two-pointers`

Given an array and a value, remove all instances of that value in place and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example**:

Given input array nums = `[3,2,2,3]`, val = `3`

Your function should return length = 2, with the first two elements of nums being 2.

**solution 1**

```c++
int removeElement(vector<int>& nums, int val) {
	int i = 0, n = nums.size();
	while (i<n) {
		if (nums[i] == val) nums[i] = nums[n---1];
		else                ++i;
	}
	return i;
}
```
用从头开始双指针是一种常见的方法. 但是酱紫在最坏的情况下会有n-1次赋值操作. 双指针一边在头, 一边在尾会把赋值次数缩减到val值出现的次数.
