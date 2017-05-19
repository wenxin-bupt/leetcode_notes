### [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Diffculty **Easy**

tags `array` `vector` `dynamic programming` `DP`

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Example 1
```
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
```

Example 2
```
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.
```

**solution 1**
```c++
int maxProfit(vector<int>& prices) {
    int min_price = INT_MAX, max_profit = 0;
    for (int i = 0; i < prices.size(); i++) {
        min_price = min(prices[i], min_price);
        max_profit = max(max_profit, prices[i]-min_price);
    }
    return max_profit;
}
```
这个方法吼棒! `min_price` 表示遍历位置以前最小的值. 是~N的算法

naive 算法就是直接找出所有对, 可以通过找出局部最小值规避一些, 但是无法避免最坏的情况下 ~N^2 的情况.

**与 max subarray problem 的关系**

当我们把 [1, 7, 4, 11] 变成 [0, 6, -3, 7] 这样的差值序列时, 我们发现所谓的买股票问题变成了max subarray problem

对于此的解, 我们有`Kadane's Algorithm`. 核心思想是和最大的数组的任意前缀序列的和一定不是负数.
```c++
public int maxProfit(int[] prices) {
  int maxCur = 0, maxSoFar = 0;
  for (int i = 1; i < prices.length; i++) {
    maxCur = Math.max(0, maxCur += prices[i] - prices[i-1]); // 一旦局部和小于0, 就清0
    maxSoFar = Math.max(maxCur, maxSoFar);
  }
  return maxSoFar;
}
```
