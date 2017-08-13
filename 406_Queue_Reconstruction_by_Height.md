### [Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/description/)

Difficulty **Medium**

tags `stable sort` 

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers `(h, k)`, where `h` is the height of the person and `k` is the number of people in front of this person who have a height greater than or equal to `h`. Write an algorithm to reconstruct the queue.

**Note:**

The number of people is less than 1,100.

**Example**

```
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

思路：

观察后发现用两次排序，然后依次插入即可。 为了插入的时候节省时间使用了`list`.

**solution 1**
```c++
class Solution {
    static bool comp1(const pair<int, int>& p1, const pair<int, int>& p2) {
        return p1.first > p2.first;
    }
    static bool comp2(const pair<int, int>& p1, const pair<int, int>& p2) {
        return p1.second < p2.second;
    }
public:
    vector<pair<int, int>> reconstructQueue(vector<pair<int, int>>& people) {
        stable_sort(people.begin(), people.end(), comp2);
        stable_sort(people.begin(), people.end(), comp1);
        list<pair<int, int>> l;
        for (pair<int, int> p : people) {
            l.push_back(p);
        }
        auto iter = l.begin();
        while (iter != l.end()) {
            auto pnt = l.begin();
            int cnt = 0;
            while (cnt < (*iter).second) {
                pnt++;
                cnt++;
            }
            l.insert(pnt, *iter);
            iter++;
            l.erase(prev(iter));
        }
        vector<pair<int, int>> res;
        for (auto p : l) {
            res.push_back(p);
        }
        return res;
    }
};
```
