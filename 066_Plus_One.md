### [Plus One](https://leetcode.com/problems/plus-one/)

Difficulty **Easy**

tags `adding big numbers` `array`

Given a non-negative integer represented as a **non-empty** array of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.


我觉得这道题的描述很不清晰。 应该给给几个列子比如199的表示为`[1, 9, 9]`这样，就很容易理解了。

思路：

只需考虑一个头部需要进位的情形。

根据题目可以简化case，我本来分两部分讨论， 即最后一位加1和剩下的位考虑进位的情况。 后来发现可以统一起来。

**solution 1**
```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int carry = 1;
        for (int i = digits.size()-1; i>=0; i--) {
            int d = digits[i];
            digits[i] = (d + carry) % 10; 
            carry = (d + carry) / 10;
        }
        if (carry == 1) 
            digits.insert(digits.begin(), 1);
        return digits;
    }
};
```

