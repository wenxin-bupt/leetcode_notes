### [Sentence Screen Fitting](https://leetcode.com/problems/sentence-screen-fitting)

Difficulty **medium**

tags `unordered_map`

Given a `rows x cols` screen and a sentence represented by a list of **non-empty** words, find **how many times** the given sentence can be fitted on the screen.

**Note**:

A word cannot be split into two lines.
The order of words in the sentence must remain unchanged.
Two consecutive words **in a line** must be separated by a single space.
Total words in the sentence won't exceed 100.
Length of each word is greater than 0 and won't exceed 10.
1 ≤ rows, cols ≤ 20,000.

**Example 1:**
```
Input:
rows = 2, cols = 8, sentence = ["hello", "world"]

Output:
1

Explanation:
hello---
world---

The character '-' signifies an empty space on the screen.
```
**Example 2:**
```
Input:
rows = 3, cols = 6, sentence = ["a", "bcd", "e"]

Output:
2

Explanation:
a-bcd-
e-a---
bcd-e-

The character '-' signifies an empty space on the screen.
```

**Example 3:**
```
Input:
rows = 4, cols = 5, sentence = ["I", "had", "apple", "pie"]

Output:
1

Explanation:
I-had
apple
pie-I
had--

The character '-' signifies an empty space on the screen.
```

看起来简单的题，如果直接按照暴力思路来做会发现有些case不了。 下面这种方法总结了排列方式， 有相同字符串开头的行的排列是相同的。 所以，存储了开头字符串对应的行的特征就能在之后直接调用， 从而节省时间。 是一个观察和提炼的题目。 思维不要完全被题目所提示的DP所江化啊哈哈。

lesson & corner case:
1. 在scale比较大的情况下， 暴力解法不能满足需求
2. unordered_map 中的 count 和 emplace 竟然之前没有用过. 以及考察下她的实现吧， 记得是红黑树， 难道是桶排序？

**solution 1**

```c++
class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        int num = 0, n = sentence.size();
        unordered_map<int, int> map;
        while (rows--) {
            int p = num % n;
            if (map.count(p) == 0) {
                int cnt = 0,  len = -1;
                // there is a trick
                // we can take words in screen as a ' ' + 'word' block
                // except that the first world doesn't has a ' '(whitespace)
                // before it.
                // So we set len = -1 initially and take all the words the same
                while ((len = len + 1 + sentence[(p+cnt)%n].size()) <= cols)
                    cnt++;
                map.emplace(p, cnt);
            }
            num += map[p];
        }
        return num / n;
    }
};
```
