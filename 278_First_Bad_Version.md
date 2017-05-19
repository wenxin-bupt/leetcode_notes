### [First Bad Version](https://leetcode.com/problems/first-bad-version/)

Difficulty **Easy**

tags `binary search` `array`

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**solution 1**
```c++
bool isBadVersion(int version);

class Solution {
public:
  int firstBadVersion(int n) {
      // corner case 中的n给到了 INT_MAX，所以修改了二分搜索的区间构成方式。
      // 同时借鉴了034中找最左和最右值的经验。
      int h = 1, t = n, m;
      while (h <= t) {
          m = h + (t - h >> 1);
          if (isBadVersion(m)) t = m - 1;
          else h = m + 1;
      }
      return h;
  }
};
```
