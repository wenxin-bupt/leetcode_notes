### [Third Maxium Number](https://leetcode.com/problems/third-maximum-number/M)  

Difficulty **Easy**

tags `array`

Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

**Example 1**:
```
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.
```

**Example 2**:
```
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```
**Example 3**:
```
Input: [2, 2, 3, 1]

Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.

Both numbers with value 2 are both considered as second maximum.
```
**solution 1**

beats 56.47%

```c++
int thirdMax(vector<int>& nums) {
    int  a[3] = {INT_MIN, INT_MIN, INT_MIN};
    bool f = false;
    for (int val : nums) {
        if (!f && val==INT_MIN) f = true;
        if (val > a[2]) {
            if (val < a[1])                  a[2] = val;
            else if(val>a[0])               {a[2] = a[1]; a[1] = a[0]; a[0] = val;}
            else if(val>a[1] && val<a[0])   {a[2] = a[1]; a[1] = val;}
        }
    }
    return (a[2]>INT_MIN || (a[2]!=a[1] && f)) ? a[2] : a[0];
}
```
- 问题: long long 转换为 int 如何转换? 耗时如何?
- 问题: long long 的比较耗时会比 int 的比较耗时多吗?  > 比判等耗时多吗?

**solution 2**

beats 16%. concise code. 充分利用了set是有序的特性. 利用了迭代器和逆向迭代器.
```c++
int thirdMax(vector<int>& nums) {
    set<int> top3;
    for (int num : nums)
        // 只有插入其中本来不存在的元素的时候才继续
        if (top3.insert(num).second && top3.size() > 3)  
            top3.erase(top3.begin());
    return top3.size() == 3 ? *top3.begin() : *top3.rbegin();
}
```
