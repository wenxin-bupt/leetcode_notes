### [Strobogrammatic Number II](https://leetcode.com/problems/strobogrammatic-number-ii/)

Difficulty **Medium**

tags `recur`

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

For example,

Given n = 2, return `["11","69","88","96"]`.

思路：

自然想到需要递归。 因为左右相关，所以只递归一半， 最后把另一半对应补上即可。

**solution 1**
```c++
class Solution {
    void helper(vector<string> &res, string s, int n) {
        static string option = "01869";
        static map<char, char> m = {{'0','0'}, {'1','1'}, {'8', '8'}, {'6', '9'}, {'9', '6'}};
        if (s.size() * 2 < n) {
            for (char c : option) {
                helper(res, c+s, n);
            }
        } else if (n==1 || (n>1&&s[0]!='0')) {
             for (int i=n/2-1; i>=0; i--) {
                 s = s+m[s[i]];
             }
             res.push_back(s);
        }
    }
public:
    vector<string> findStrobogrammatic(int n) {
        vector<string> res;
        if (n%2) {
            helper(res, "1", n);
            helper(res, "8", n);
            helper(res, "0", n);
        } else {
            helper(res, "", n);
        }
        return res;
    }
};
```
