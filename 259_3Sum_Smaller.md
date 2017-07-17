### [3Sum Smaller](hhttps://leetcode.com/problems/3sum-smaller/)

Difficulty **Medium**

tags `two pointers` `sum problem`

Given an array of n integers nums and a target, find the number of index triplets `i, j, k` with `0 <= i < j < k < n` that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.

For example, given nums = `[-2, 0, 1, 3]`, and target = 2.

Return 2. Because there are two triplets which sums are less than 2:

```
[-2, 0, 1]
[-2, 0, 3]
```

**Follow up:**

Could you solve it in O(n2) runtime?

思路：

这道题是最近以来一道明显做得不好的题。 因为粗心和推理不明遇到了各种问题，花了一个半小时才写出能用的版本。 

对于`i<j<k`三个数，我的最初想法是先遍历j, k所有可能的组合，对于每个组合找到满足条件的最大的i, 那么从第0个元素到第i个元素之间的所有元素都满足条件。 既然要遍历j, k的所有组合， 就会产生~N^2的访问复杂度， 而我们要最后的复杂度控制在O(N^2), 就必须使查找i的复杂度控制在常数级别。 **这个实际上是做不到的。** 当然我使用了 `multiset`的upper_bound()和`map`来实现了名义上常数的访问。 `multiset`和`map`是用红黑树实现的， 他们的查找操作都是~logN级别的， 这样想来还不如手写一个二分来的快。 这种方法的时间复杂度是~logN * N^2。 而且还遇到了很多的corner case。

应该考虑，用~N的复杂度实现一个`2Sum Smaller`， 然后外层再遍历一次， 自然就是~N^2的算法。 而线性`2Sum Smaller`是用`two pointers`实现的。

需要注意的cases:

1. 认真读题， 本题并不考虑数组中有重复的情况。 

    ps: 关于去重， 如果**不**要求结果有序的话， 用set配合vector做去重比sort之后再unique更快。 参见: [What's the most efficient way to erase duplicates and sort a vector?
](https://stackoverflow.com/questions/1041620/whats-the-most-efficient-way-to-erase-duplicates-and-sort-a-vector)

2. 如果想用 nums[i] + nums[k] >= target 的情况来跳过循环的时候， 要注意nums[i]需大于0。 否则加一个值为负的nums[j]，结果就不如你所愿了。



**solution 1**

```c++
class Solution {
public:
    int threeSumSmaller(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int res = 0;
        for (int i = 0; i < int(nums.size())-2; i++) {
            if (nums[i]+nums[i+1]+nums[i+2] >= target) break;
            int j = i+1, k = nums.size()-1;
            while (j < k) {
                while (j<k && nums[i]+nums[j]+nums[k] >= target) k--;
                res += k-j;
                j++;
            }
        }
        return res;
    }
};
```

