### [Strobogrammatic Number](https://leetcode.com/problems/strobogrammatic-number/)

Difficulty **Easy**

tags `string`

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.

思路：

有一个小小坑, 样例中没有给"0", 然而"0"也是具有这种特性的

**solution 1**

```c++
class Solution {
public:
    bool isStrobogrammatic(string num) {
        int left = (num.size()-1) / 2, right = num.size() / 2;
        while (right < num.size()) {
            switch(num[left]) {
                case '0':
                    if (num[right] != '0') return false;
                    break;
                case '1':
                    if (num[right] != '1') return false;
                    break;
                case '6':
                    if (right == left || num[right] != '9') return false;
                    break;
                case '8':
                    if (num[right] != '8') return false;
                    break;
                case '9':
                    if (right == left || num[right] != '6') return false;
                    break;
                default: return false;
            }
            left--;
            right++;
        } 
        return true;   
    }
};
```

