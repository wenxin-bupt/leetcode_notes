### [Jump Game](https://leetcode.com/problems/jump-game/)

Difficulty **Medium**

tags `greedy` `array` `vector`

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

For example:

A = `[2,3,1,1,4]`, return `true`.

A = `[3,2,1,0,4]`, return `false`.

**solution 1**
```c++
bool canJump(vector<int>& nums) {
    int l, r = -1,  max = 0;  // 为了赢在起跑线上, 打赢1个元素的Corner case
    while (max > r) {
        l = r+1; r = max;
        if (r >= nums.size()-1) return true;
        max = nums[l] + l;
        for (int i = 1; l+i <= r; i++)
            if (nums[l+i] + l + i > max)   max = l + i + nums[l+i];
    }
    return false;
}
```
傻傻地用stack实现之后, 发现题主给了一个25000的巨大数组, 果然TLE了23333

然后自己写了一个这个, 思想就是把区域分成已经探索的和新扩展的. 然后迭代. WorstCase ~n

但是其实可以简化成这个样子..

**solution 2**
```c++
public boolean canJump(int[] A) {
    int canReach=0;
    for(int i=0;i < A.length && i <= canReach; i++){
        if(i+A[i] > canReach) canReach = i+A[i];
        if(canReach >= A.length-1) return true;
    }
    return (canReach >= A.length-1);
}
```

跪!
