### [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Difficulty **Easy**

tags `array` `dynamic programming`

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array `[-2,1,-3,4,-1,2,1,-5,4]`, the contiguous subarray `[4,-1,2,1]` has the largest sum = 6.

**solution 1**
```c++
// DP问题
// 考虑以第i个元素结尾的子序列，它有两种组成， 以i-1结尾的最大序列加上第i个元素， 或者单独第i个元素
// subArray(i) = subArray(i-1)+A[i] / A[i]
// 如果subArray(i-1) < 0 则A[i]即可， 否则为 subArray(i-1)+A[i]
// 这道题有经典
int maxSubArray(vector<int>& nums) {
    vector<int> sum; 
    sum.resize(nums.size());
    if (nums.size()==0) return 0;
    sum[0] = nums[0]
    int ans = sum[0];
    for (int i = 1; i < nums.size(); i++) {
        sum[i] = max(nums[i], nums[i]+sum[i-1]);
        ans = max(ans, sum[i]);    
    }
    return ans;            
}
```