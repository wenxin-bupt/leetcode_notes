### [Flip Game](https://leetcode.com/problems/flip-game/)   

Difficulty **Easy**

tags `array`

You are playing the following Flip Game with your friend: Given a string that contains only these two characters: `+` and `-`, you and your friend take turns to flip two **consecutive** `"++"` into `"--"`. The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to compute all possible states of the string after one valid move.

For example, given `s = "++++"`, after one move, it may become one of the following states:

```
[
  "--++",
  "+--+",
  "++--"
]
```

If there is no valid move, return an empty list `[]`.

思路: 

greedy

失误的case:

1. 这道题存在的意义就是提醒同学们 `.size()` 返回的是一个无符号数，对它进行操作没有考虑下溢的时候，你就GG了。



**solution 1**

```c++
class Solution {
public:
    vector<string> generatePossibleNextMoves(string s) {
        vector<string> res;
        for (int i=0; i<int(s.size())-1; i++) {
            if (s[i] == '+' && s[i+1] == '+'){
                s[i] = s[i+1] = '-';
                res.push_back(s);
                s[i] = s[i+1] = '+';
            }
        }
        return res;
    }
};
```
