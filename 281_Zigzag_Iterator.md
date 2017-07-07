### [Zigzag Iterator](https://leetcode.com/problems/zigzag-iterator/)

Difficulty **Medium**

tags `design pattern`

Given two 1d vectors, implement an iterator to return their elements alternately.

For example, given two 1d vectors:

```
v1 = [1, 2]
v2 = [3, 4, 5, 6]
```

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: `[1, 3, 2, 4, 5, 6]`.

Follow up: What if you are given k 1d vectors? How well can your code be extended to such cases?

The "Zigzag" order is not clearly defined and is ambiguous for k > 2 cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic". For example, given the following input:

```
[1,2,3]
[4,5,6,7]
[8,9]
```

It should return `[1,4,8,2,5,9,3,6,7]`.


思路：

复习了最基本的design pattern: iterator.

想象成一个二维矩阵来处理即可

**solution 1**
```c++
class ZigzagIterator {
public:
    ZigzagIterator(vector<int>& v1, vector<int>& v2) {
        list.push_back(v1);
        list.push_back(v2);       
        iter = 0;
    }

    int next() {
        if (!hasNext()) throw "Has no next node";
        iter++;
        return cur;
    }

    bool hasNext() {
        for (int i = 0; i <= list.size(); i++) {
            int pos = (iter + i) / list.size();
            int idx = (iter + i) % list.size();
            if (list[idx].size() > pos) {
                iter = i + iter;
                cur  = list[idx][pos];
                return true;
            }
        }
        return false;
    }
private:
    vector<vector<int>> list;
    int iter;
    int cur;
};
```