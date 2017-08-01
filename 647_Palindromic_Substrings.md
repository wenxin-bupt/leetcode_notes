### [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

Difficulty **Medium**

tags `array`

iven a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

**Example 1:**
```
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```
**Example 2:**
```
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

**Note:**

1. The input string length won't exceed 1000.

**solution 1**
```c++
class Solution {
    void odd(string s, int pos, int &cnt) {
        cnt++;
        int left = pos-1, right = pos+1;
        while (left>=0 && right<s.size() && s[left]==s[right]) {
            cnt++;
            left--;
            right++;
        }
    }
    
    void even(string s, int pos, int &cnt) {
        int up = pos+1;
        while (pos>=0 && up<s.size() && s[up]==s[pos]) {
            cnt++;
            pos--;
            up++;
        }
    }
public:
    int countSubstrings(string s) {
        int res = 0;
        for (int i = 0; i < s.size(); i++) {
            odd(s,i,res);
            even(s,i,res);
        }
        return res;
    }
};
```

