### [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

Difficulty **Medium**

tags `greedy` `array` `vector`

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

Hide Company Tags.

**solution 1**
```c++
int maxProfit(vector<int>& prices) {
    int p = 0, q = p+1, sum = 0;
    while (p < prices.size()) {
        while (q < prices.size() && prices[q] >= prices[q-1]) q++;
        if (prices[q-1] - prices[p] > 0) sum += prices[q-1] - prices[p];
        p = q; q = p+1;
    }
    return sum;
}
```
经过分析发现, 只要把每个递增序列头尾相减即可.  (然而还是不是最简啊!!!

**solution 2**
```c++
public int maxProfit(int[] prices) {
    int total = 0;
    for (int i=0; i< prices.length-1; i++)
        if (prices[i+1]>prices[i])
            total += prices[i+1]-prices[i];
    return total;
}
```
可以化简成这个样子, 结果是一样的!!!
