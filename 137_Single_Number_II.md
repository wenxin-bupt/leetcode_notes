### [Single Number II](https://leetcode.com/problems/single-number-ii/)

Difficulty **Medium**

tags  `Bit Manipulation`

Given an array of integers, every element appears three times except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**solution 1**
```c++
int singleNumber(vector<int>& nums) {
    int one = 0, two = 0, old_one;
    for (int n : nums) {
        one = (one ^ n) & (~two);
        two = (two ^ n) & (~one);
    }
    return one;
}
```

it's a tricky code!!

核心理念是构造一个mod 3加法，one， two对应第一位，第二位（第二位是高位）

before [two, one]| input |after [two, one]
----------------|--------|----
[0, 0]          | 1      | [0, 1]
[0, 1]          | 1      | [1, 0]
[1, 0]          | 1      | [0, 0]
[0, 0]          | 0      | [0, 0]  
[0, 1]          | 0      | [0, 1]    
[1, 0]          | 0      | [1, 0]   

然后就是用位操作来进行， 本来核心代码应该是
```c++
old_one = one;
one = bit process... // based on one, two, input  
two = bit process // 按照正常的逻辑应该是基于before的one, 但是before的one已经修改了， 所以前面存了old_one.  
```
solultion 1的代码是蛮tricky的！因为并不是基于一般的思维，而是找到了规律。

否则核心代码为
**solution 2**
```c++
int singleNumber(vector<int>& nums) {
    int one = 0, two = 0, old_one;
    for (int n : nums) {
        int old_one = one;
        one = (one ^ n) & (~two);
        two = (n&old_one) | (~n&two) ;
    }
    return one;
}
```
基于观察，输入1时，two的结果其实刚好对应old_one, 输入0时, two的结果刚好对应之前的two。 23333
