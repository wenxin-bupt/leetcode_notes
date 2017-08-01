### [Set Mismatch](https://leetcode.com/problems/set-mismatch/)

Difficulty **Easy**

tags `set`

The set S originally contains numbers from 1 to n. But unfortunately, due to the data error, one of the numbers in the set got duplicated to another number in the set, which results in repetition of one number and loss of another number.

Given an array nums representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.

**Example 1:**
```
Input: nums = [1,2,2,4]
Output: [2,3]
```

**Note:**

1. The given array size will in the range [2, 10000].
2. The given array's numbers won't have any order.

**solution 1**
```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int m, l;
        vector<bool> s(nums.size()+1, false);
        for (int i = 0; i < nums.size(); i++) {
            if (s[nums[i]]) m = nums[i];
            else s[nums[i]] = true;
        }
        
        for (int i = 1; i < nums.size()+1; i++) 
            if (!s[i]) {
                l = i;
                break;
            }
        return {m, l};
    }
};
```

