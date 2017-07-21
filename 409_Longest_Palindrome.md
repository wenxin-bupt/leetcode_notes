### [Longest Palindrome](https://leetcode.com/problems/longest-palindrome/)

Difficulty **Easy**

tags `ascii` 

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

**Note:**

Assume the length of given string will not exceed 1,010.

**Example:**
```

Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```

要注意的Case：

1. 在ASCII表中， 小写字母是在大写字母之后的。 而且他们并不相连。
2. 运算符优先级，`+` 在`&`之前。 所以`even * 2 + odd & 1`的结果是 `(even * 2 + odd) & 1`, 就出现了错误。

**solution 1**
```c++
class Solution {
public:
    int longestPalindrome(string s) {
        vector<int> m(80, 0);
        int even = 0, odd = 0;
        for (int i=0; i<s.size(); i++) 
            m[s[i]-'A']++;
        for (int a : m) {
            odd = odd|a;
            even += a/2;
        }
        return even*2 + (odd&1);
    }
};
```

