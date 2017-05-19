### [License Key Formatting](https://leetcode.com/problems/license-key-formatting/#/description)

Difficulty **Medium**

tags `string`

Now you are given a string S, which represents a software license key which we would like to format. The string S is composed of alphanumerical characters and dashes. The dashes split the alphanumerical characters within the string into groups. (i.e. if there are M dashes, the string is split into M+1 groups). The dashes in the given string are possibly misplaced.

We want each group of characters to be of length K (except for possibly the first group, which could be shorter, but still must contain at least one character). To satisfy this requirement, we will reinsert dashes. Additionally, all the lower case letters in the string must be converted to upper case.

So, you are given a non-empty string S, representing a license key to format, and an integer K. And you need to return the license key formatted according to the description above.

**Example 1:**
```
Input: S = "2-4A0r7-4k", K = 4

Output: "24A0-R74K"

Explanation: The string S has been split into two parts, each part has 4 characters.
```

**Example 2:**
```
Input: S = "2-4A0r7-4k", K = 3

Output: "24-A0R-74K"

Explanation: The string S has been split into three parts, each part has 3 characters except the first part as it could be shorter as said above.
```
**Note:**

    The length of string S will not exceed 12,000, and K is a positive integer.

    String S consists only of alphanumerical characters (a-z and/or A-Z and/or 0-9) and dashes(-).

    String S is non-empty.

同样很简单的题， 但是考量了提炼和分解能力。 考虑一个Corner case的时候要分别考虑这种Corner case是否适合数组的开始， 中部， 结尾， 三个部位。 这里的难点就是我们虽然习惯从头到尾进行遍历， 然而题目却要求把特殊项放在头部。 灵活思路， 从尾到头进行遍历， 然后再逆向即可。

了解了 string 的 rbegin, rend 逆向迭代器的用法

upper case, lower case 的转换直接用toupper() [int -> int] 就好。 同样我们也有  tolower()

**solution 1**
```c++
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
        string res;
        for (auto p = S.rbegin(); p != S.rend(); p++)
            if (*p != '-') {
                res = res.size() % (K+1) == K ? res + '-' : res;
                res += toupper(*p);
            }
        reverse(res.begin(), res.end());
        return res;

    }
};
```
