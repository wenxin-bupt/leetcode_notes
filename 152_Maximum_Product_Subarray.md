### [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

Difficulty **Medium**

tags `array`

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array `[2,3,-2,4]`,
the contiguous subarray `[2,3]` has the largest product = 6.

**solution 1**
```c++

// 是solution2 思路的简化版
int maxProduct(vector<int>& nums) {
    if (nums.size() == 0) return 0;
    int ans = INT_MIN, front = 1, back = 1;
    for (int i=0; i < nums.size(); i++) {
        front = front*nums[i];
        back  = back*nums[nums.size()-1-i];
        ans = max(ans, max(front, back));
        if (front==0) front = 1;
        if (back==0)  back  = 1;
    }
    return ans;
}
```

**solution 2**
```c++
// silly version
// 数组中的0是对product结果影响最大的, 以0分割开数组
// 如果分组的product大于0, 即可得此分组最大product
// 否则分组的product小于0，最大的product只可能从左或者从右开头的一个子序列。
int maxProduct(vector<int>& nums) {
class Solution {
public:
    int sub_Product(vector<int>& nums, int h, int t, int acc) {
        if (h > t) return acc;
        else return sub_Product(nums, h+1, t, acc*nums[h]);
    }
    int maxProduct(vector<int>& nums) {
        int l = 0, r, res = INT_MIN;
        while (l<nums.size() && nums[l]==0) l++;
        r = l;
        for (; r <= nums.size(); r++) {
            if((r == nums.size() || nums[r] == 0) && r > l) {
                    int sum = sub_Product(nums, l, r-1, 1);
                    if (sum < 0 && r-l>1){
                       int _l = l, _r = r-1;
                       for (; _l<r-1 && nums[_l]>0; _l++);   _l++;
                       for (; _r>l   && nums[_r]>0; _r--);   _r--;
                       if (_r >= l)   sum = max(sum, sub_Product(nums,  l,  _r, 1));
                       if (_l <= r-1) sum = max(sum, sub_Product(nums, _l, r-1, 1));
                    }
                    res = max(res, sum);
                    while (r<nums.size() && nums[r]==0) r++;
                    l = r;
            }
        }
        l = 0, r = nums.size();
        int sum = sub_Product(nums, l, r-1, 1);
        if (sum < 0 && r-l>1){
           int _l = l, _r = r-1;
           for (; _l<r-1 && nums[_l]>0; _l++);   _l++;
           for (; _r>l   && nums[_r]>0; _r--);   _r--;
           if (_r >= l)   sum = max(sum, sub_Product(nums,  l,  _r, 1));
           if (_l <= r-1) sum = max(sum, sub_Product(nums, _l, r-1, 1));
        }
        res = max(res, sum);
        return res;
    }
};
```
