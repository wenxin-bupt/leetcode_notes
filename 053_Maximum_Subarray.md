### [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Difficulty **Easy**

tags `array` `dynamic programming`

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array `[-2,1,-3,4,-1,2,1,-5,4]`, the contiguous subarray `[4,-1,2,1]` has the largest sum = 6.

**solution 1**
```c++
// DP����
// �����Ե�i��Ԫ�ؽ�β�������У�����������ɣ� ��i-1��β��������м��ϵ�i��Ԫ�أ� ���ߵ�����i��Ԫ��
// subArray(i) = subArray(i-1)+A[i] / A[i]
// ���subArray(i-1) < 0 ��A[i]���ɣ� ����Ϊ subArray(i-1)+A[i]
// ������о���
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