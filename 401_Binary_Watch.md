### [Binary Watch](https://leetcode.com/problems/binary-watch/)   

Difficulty **Easy**

tags `bit mask`

A binary watch has 4 LEDs on the top which represent the **hours (0-11)**, and the 6 LEDs on the bottom represent the **minutes (0-59)**.

Each LED represents a zero or one, with the least significant bit on the right.

![](http://oq5gcgc8d.bkt.gdipper.com/401_Binary_Watch/Binary_clock_samui_moon.jpg)

For example, the above binary watch reads "3:25".

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

**Example**:
```
Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```

**Note**:

- The order of output does not matter.

- The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".

- The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

思路：

用了CountBits

**solution 1**

```c++
class Solution {
public:
    bool validUtf8(vector<int>& data) {
        int count = 0;
        for (auto c : data) {
            if (count == 0) {
                if ((c >> 5) == 0b110) count = 1;
                else if ((c >> 4) == 0b1110) count = 2;
                else if ((c >> 3) == 0b11110) count = 3;
                else if ((c >> 7)) return false;
            } else {
                if ((c >> 6) != 0b10) return false;
                count--;
            }
        }
        return count == 0;
    }
};
```
