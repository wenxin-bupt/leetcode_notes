### [Two Sum](https://leetcode.com/problems/two-sum/)

Difficulty **Easy**

tags `two-pointers` `unordered_map` `map` `hash` `k-sum`

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**solution 1**
```c++
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> mapping;
    for(int i = 0; i < nums.size(); ++i){
      if(mapping.find((target - nums[i])) != mapping.end())
         return {mapping[target - nums[i]], i};
      mapping[nums[i]] = i;
    }
    return {-1, -1};
}
```

1.首先可以想到使用双指针的naive算法. 即用两个指针把array撸一遍. 遍历所有的组合之后，因为题设中说一个array只有一种解法组合，所以可以得出这个算法的时间平均复杂度是 `1/2*(N-1+1)*(N-1)*1/2  == 1/4*N*(N - 1)` 是 `~(N^2)`的. 这样的算法时间复杂度是不能接受哒.

2.考虑引入hash table， unordered_map的使用hash表来实现插入删除复杂度为`~1`, 而不再是map中的`~lgN`.

3.简单的想法是将元素都进入unordered_map, 然后再进行一次查表. 平均情况下找到一对需要遍历一次半. 而改进后, 最坏只需要遍历插入一次.
