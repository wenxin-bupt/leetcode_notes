### [Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)

Difficulty **Medium**

tags `array` `vector`

Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

**Could you do it without extra space and in O(n) runtime**?

Example:
````
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
````

**solution 1**
```c++
vector<int> findDuplicates(vector<int>& nums) {
     vector<int> res;
     int i = 0;
     while (i<nums.size()) {
         if (nums[nums[i]-1] != nums[i]) swap(nums[nums[i]-1], nums[i]);
         else i++;
     } // 所有i扫过的地方, 都已经满足了条件~
     for (int i = 0; i < nums.size(); i++) {
         if (nums[i] != i+1) res.push_back(nums[i]);
     }
    return res;
}
```
时间 ~N 空间 ~1 的算法 主要用了条件`1 ≤ a[i] ≤ n (n = size of array)` 相当于构造了一个本地hash表.

非~1空间的算法很多, 不表.
