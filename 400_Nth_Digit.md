### [Nth Digit](https://leetcode.com/problems/nth-digit/)   

Difficulty **Easy**

tags `math`

Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

**Note:**

n is positive and will fit within the range of a 32-bit signed integer (n < 231).

**Example 1:**

```
Input:
3

Output:
3
```

**Example 2:**

```
Input:
11

Output:
0

Explanation:
The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
```

**solution 1**

```c++
class Solution {
public:
    int findNthDigit(int n) {
        int cnt = 1,  sum = 0;
        while (n > sum + 9 * cnt * pow(10, cnt-1)) {
            sum += 9 * cnt * pow(10, cnt-1);
            cnt++;
        }
        int num = (n-sum-1)/cnt + pow(10, cnt-1), k = (n-sum-1)%cnt;
        return (int)(num/pow(10, cnt-k-1)) % 10;
    }
};
```
