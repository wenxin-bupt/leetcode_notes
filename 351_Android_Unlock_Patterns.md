### [Android Unlock Patterns](https://leetcode.com/problems/android-unlock-patterns/)

Difficulty **Medium**

tags `DP` `bit mask`

Given an Android **3x3** key lock screen and two integers **m** and **n**, where 1 ≤ m ≤ n ≤ 9, count the total number of unlock patterns of the Android lock screen, which consist of minimum of **m** keys and maximum **n** keys.

**Rules for a valid pattern**:

1. Each pattern must connect at least **m** keys and at most **n** keys.
2. All the keys must be distinct.
3. If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
4. The order of keys used matters.

![](http://oq5gcgc8d.bkt.gdipper.com/351_Android_Unlock_Patterns/1.png)

Explanation:

```
| 1 | 2 | 3 |
| 4 | 5 | 6 |
| 7 | 8 | 9 |
```

**Invalid move:** `4 - 1 - 3 - 6` 

Line 1 - 3 passes through key 2 which had not been selected in the pattern.

**Invalid move:** `4 - 1 - 9 - 2`

Line 1 - 9 passes through key 5 which had not been selected in the pattern.

**Valid move:** `2 - 4 - 1 - 3 - 6`

Line 1 - 3 is valid because it passes through key 2, which had been selected in the pattern

**Valid move:** `6 - 5 - 4 - 1 - 9 - 2`

Line 1 - 9 is valid because it passes through key 5, which had been selected in the pattern.

Example:

Given **m** = 1, **n** = 1, return 9.

思路：

这道题很容易想到DFS的版本，但是提交上去之后比别人的版本慢了100倍。 

然后发现这道题是有DP版本的，相比于DFS版本的效率提升非常大。 DP算法的核心是要提炼出状态和状态之间的关系，我常常把DP和自动机做对比。 考虑到题目中说考虑顺序，如果要严格死板地提取状态可以生成9^9个状态。 但是这里，我们把这些状态归类， 把被选择的数字和最后一位被选中的数字结合在一起作为一个状态单元（数字被选择的可能一共有1<<9次种，这里我们用bit mask来描述） 这样状态之间就可以通过添加最后一位数字互相转化，这样就是DP的基础。

正确性易证明，下面的dp[state][end]中的state是从0开始增加的，直到最后一个状态。 所以每次都在state中加一位， dp的下一步生成的state比此时的state大。 循环到某个state的时候，可以确定它不会被它之后的state更新， 也确定它已经获取了它所有的更新。

有益的`Reference`: [leetcode 338 Counting Bits](https://leetcode.com/problems/counting-bits/)

**solution 1**

```c++
class Solution {
    int dp[1<<9][10];
    int skip[10][10];
    int count[1<<9];
    // 这是一个trick，考虑一个对象建立以后会重复测试， 所以初始化的过程就不重复了
    bool inited;

	void initSkipArray() {
	    memset(skip, 0, sizeof(skip));
	    skip[1][3] = skip[3][1] = 2;
	    skip[1][7] = skip[7][1] = 4;
	    skip[3][9] = skip[9][3] = 6;
	    skip[7][9] = skip[9][7] = 8;
	    skip[1][9] = skip[9][1] = skip[3][7] = skip[7][3] = skip[2][8] = skip[8][2] = skip[4][6] = skip[6][4] = 5;
	}
    
    // Reference: [leetcode 338 Counting Bits](https://leetcode.com/problems/counting-bits/)
    // 内含一个trick， 统计一个数的二进制表示中的1的个数 
	void initCountArray() {
		count[0] = 0;
		for (int i = 1; i < 1<<9; i++)
		    count[i] = count[i&(i-1)] + 1;
	}


	void initDp() {
	    memset(dp, 0, sizeof(dp));
	    # 初始化
	    for (int i=1; i<10; i++) 
	        dp[1<<(i-1)][i] = 1;
	    
	    for (int state = 0; state < 1<<9; state++) 
	        for (int end = 1; end < 10; end++) 
	            if (dp[state][end])
	                for (int newend = 1; newend < 10; newend++) 
	                    if (!(state & 1<<(newend-1))) 
	                        if (!skip[end][newend] || (state & 1<<(skip[end][newend]-1)))
	                            dp[state|1<<(newend-1)][newend] += dp[state][end];
	}

public:
    int numberOfPatterns(int m, int n) {
        if (!inited) {
            initSkipArray();
    	    initCountArray();
    	    initDp();
            inited = true;
        }
    	int res = 0;
    	for (int state = 0; state < 1<<9; state++)
    	    if (count[state] >= m && count[state] <= n)
                for (int end = 1; end < 10; end++) 
                    res += dp[state][end];
        return res;
    }
};
```