### [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

Difficulty **Hard**

tags `two-pointers` `binary search`

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

**solution 1**
```c++
int search(vector<int>& nums, int target) {
   int l = nums.size(), i = 0, r;
   int h = 0, t = l-1, m = (h+t)>>1;
   if (l==0) return -1;
   if (nums[h]>nums[t]) {
       while (m>h) {
           if (nums[m]<nums[t]) t = m;
           else h = m;
           m = (t+h)>>1;
       }
       i = m+1;
   }
   h = 0;  t = l;
   while (t>h) {
       m = (h+t)>>1;
       r = (i+m) % l;
       if (nums[r] < target) h = m + 1;
       else if (nums[r] > target) t = m;
       else return r;
   }
   return -1;
}
```
很显然是想让我们用两次二分搜索，第一次找到最小的值，第二次再用二分搜索来做。 思路实现简单。

corner case 比较烦人。 test case中有没有经过翻转的数组， 数组长度为1的数组还有数组长度为2的数组。 在一开始的时候，设计的二分程序是找数组中的最大值。最后的搜索结果会停在最大值上面， 还需要再处理才能指向最小值。 **在迭代时， h, t没有步进，始终停留在各自的一侧，即h在较大的一侧，t在较小的一侧** 需要专门用语句来应对corner case 代码看起来挺脏的。

**solution 2**
```c++
int search(vector<int>& nums, int target) {
    int l = nums.size(), i, r;
    int h = 0, t = l-1, m;
    while (t>h) {
        m = (h+t)>>1;
        if (nums[m]>nums[t]) h = m+1;
        else t = m;
    }
    i = h;  h = 0;  t = l;  
    while (t>h) {
        m = (h+t)>>1;
        r = (i+m) % l;
        if (nums[r] < target) h = m + 1;
        else if (nums[r] > target) t = m;
        else return r;
    }
    return -1;
}
```
以上方法的第一步是寻找最小值, 这样得出的结果可以直接使用. 每步有h = m+1 这样的步进，h,t最后指向最小值. 刚好精巧地满足了各种corner case. 棒棒棒！
