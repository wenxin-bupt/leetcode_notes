### [Decode String](https://leetcode.com/problems/decode-string/#/description)

Difficulty **Medium**

tags `recur`

Given an encoded string, return it's decoded string.

The encoding rule is: `k[encoded_string]`, where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like `3a` or `2[4]`.

Examples:

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

单纯考虑本题，就是一个递归问题。 但是，如果要写得好，需要对编码了字符串的进行比较好的提炼总结。

最简单的解题方法就是按照题目的介绍进行解码就好， 但是这样代码冗杂。

trick 1: 把字符串提炼为以字母或者数字为开头， 以 ']' 或者字符串终点为结尾的子串。 就得到了递归终止条件。

trick 2: 用一个共享的递归指针，所以的子递归都会对指针造成影响。

**solution 1**

```c++
class Solution {
    string decodeString(string &s, int &p) {
        string res;
        while (p < s.size() && s[p] != ']') {
            if (isdigit(s[p]) == 0) {
                res += s[p++];
            } else {
                int cnt = 0;
                while (isdigit(s[p]) != 0)
                    cnt = cnt*10 + (s[p++] - '0');
                ++p; // jump after '['
                string sub = decodeString(s, p);
                ++p; // jump after ']'
                while (cnt--)
                    res += sub;
            }
        }
        return res;
    }
public:
    string decodeString(string s) {
        int p = 0;
        return decodeString(s, p);
    }
};
```
