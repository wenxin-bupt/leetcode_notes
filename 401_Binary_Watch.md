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
    vector<int> countBits(int num) {
        vector<int> count(1, 0);
        for (int i=1; i < num+1; i++)
            count.push_back(count[i&(i-1)]+1);
        return count;
     }
public:
    vector<string> readBinaryWatch(int num) {
        vector<int> hr = countBits(11);
        vector<int> min = countBits(59);
        vector<string> res;
        if (num > 8) return res;
        for (int h = 0; h < 12; h++) {
            string hs = to_string(h);
            for (int m = 0; m < 60; m++) {
                if (min[m] != num - hr[h]) continue;
                string ms = m>9 ? ":"+to_string(m) : ":0"+to_string(m);
                res.push_back(hs + ms);
            }
        }
        return res;
    }
};
```
