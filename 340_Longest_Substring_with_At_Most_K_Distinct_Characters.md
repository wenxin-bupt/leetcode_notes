### [Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters)

Difficulty **hard**

tags `two_pointers` `unordered_map`

Given a string, find the length of the longest substring T that contains at most k distinct characters.

For example, Given s = `“eceba”` and k = 2,

T is "ece" which its length is 3.

这里很自然想到了两个指针来进行操作。 f作为前指针， e作为后指针， [e, f]之间的所有字符所有字符都满足k distinct character的条件。

正确性易证明， f作为迭代前指针， 每次循环我们都寻找了以f作为结尾的最长的满足k不同字符的序列， 那么最长的序列一定包含在其中。

失误的corner case: k==0 的情况疏忽。

unordered_map的使用。 

**solution 1**

```c++
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        unordered_map<char, int> map;
        int f = 0, e = 0; // f --> front, e --> end
        int max = 0;
        if (k==0) return 0;
        while (f < s.size()) {
            if (map.find(s[f]) != map.end()) {
                map[s[f]]++;
            } else {
                pair<char, int> p(s[f], 1);
                map.insert(p);
                while (map.size()>k && e<f) {
                    map[s[e]]--;
                    if (map[s[e]] == 0) map.erase(s[e]);
                    e++;
                }
            }
            max = max(f - e + 1 : max);
            f++;
        }
        return max;
    }
};
```
