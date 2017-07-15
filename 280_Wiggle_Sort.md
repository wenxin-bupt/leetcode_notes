### [Wiggle Sort](https://leetcode.com/problems/wiggle-sort/)   

Difficulty **Medium**

tags `quick sort` `array`

Given an unsorted array nums, reorder it **in-place** such that `nums[0] <= nums[1] >= nums[2] <= nums[3]...`.

For example, given `nums = [3, 5, 2, 1, 6, 4]`, one possible answer is `[1, 6, 2, 5, 3, 4]`.

思路:

一看规律，就想到了用排序来解决。 这种题目一般都有trick，当然可以用心观察，但是用排序这种暴力法来解决算是比较省脑筋和时间的方法。 

考虑到效率和**in place**, 一般我们会选用`quick sort`。 这里有一个问题是, `quick sort`真的是一个原地算法吗？ 实际上在空间上，它在最坏的情况下(数组已有序)， 会生成一个`~N`级别递归栈， 这么来讲它不能被看做原地算法。 但是，在平均上， 它生成的是`~logN`级别的递归栈。 最重要的是， 它并没有使用一个辅助数组来做排序。 考虑到我们探讨一个算法的效率的时候，通常都是在`N`极大的情况下来思考的。 那么相比于`N`, `~logN`级别的空间占用是非常接近于常数级别的。 所以， 把`quick sort`看作一个原地算法是说得通的。

失误和要注意的case:

1. 推公式和计算机计算的区别：

    考虑整型`len`, `(len-1)/2*2` 和 `len-1`的结果是不同的。 使用float计算数学题目的时候也遇到过题目无论如何都无法通过的情况。 **不要把化简了的数学公式直接扔上去， 要考虑具体的计算方法**。

2. 这里用了一个3-way-quicksort， 用来提升数组中有连续相同数字时的效率。

**solution 1**

```c++
class Solution {
    int len;
    void quicksort(vector<int> &nums, int lo, int hi) {
        if (hi <= lo) return;
        int l = lo, r = hi, pivot = nums[locate(lo)], i = lo;
        while (i <= r) {
            int cmp = nums[locate(i)] - pivot;
            if     (cmp < 0) swap(nums[locate(l++)], nums[locate(i++)]);
            else if(cmp > 0) swap(nums[locate(i)], nums[locate(r--)]);
            else             i++;
        }
        
        quicksort(nums, lo, l-1);
        quicksort(nums, r+1, hi);
    }
    
    int locate(int idx) {
        if (idx <= (len - 1)/2)
            return idx*2;
        else return (idx - (len-1)/2 - 1)*2 + 1;
    }
    
public:
    void wiggleSort(vector<int>& nums) {
        len = nums.size();
        quicksort(nums, 0, len-1);
    }
};
```

思路:

规律是: 对于奇数下标的项，要求它不大于之前的项。 对于偶数下标的项，要求它不小于之前的项。 且对不符合规律的项进行调整时(交换前后项)， 并不会打破前面已经处理好的数组的规律。 

**solution 2**
```c++
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        for (int i=1; i<nums.size(); i++) 
            if ((i&1 && nums[i]<nums[i-1]) || (!(i&1) && nums[i]>nums[i-1]))
                swap(nums[i], nums[i-1]);
    }
};
```
