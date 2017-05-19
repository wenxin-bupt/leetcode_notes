### [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

Difficulty **Hard**

tags `heap` `make_heap` `merge_sort`

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
```

**solution 1**
```c++
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode h = ListNode(0);
        ListNode *p = &h;
        int n = lists.size();
        if (n==0) return NULL;
        make_heap(lists.begin(), lists.end(), compare);
        while (lists[0] != NULL) {
            p->next = lists[0];
            p = p->next;
            pop_heap(lists.begin(), lists.end(), compare);
            lists[n-1] = lists[n-1]->next;
            push_heap(lists.begin(), lists.end(), compare);
        }
        return h.next;
    }
private:
    static bool compare(ListNode *a, ListNode *b) {
        if (a == NULL) return true;
        if (b == NULL) return false;
        if (a->val > b->val) return true;
        else                 return false;
    }
};
```
要合并k个有序子数组, 自然而然就想到了. 每次遍历一遍头结点, 然后把最小的提取出来放在新的数组后面.
在这种情况下, 时间复杂度是 ~kN

可以发现, 每次都是提取一个数组中的最小值, 然后把它的继任插入, 然后再找最小值. 这个行为满足heap, 一个min-heap的典型操作. 很容易, 时间复杂度就可以提升为Nlgk.

make_heap 这里比 priority queue更有优势. 不需要实质上的删除元素, 只要在vec尾部更新元素, 就可以获得新的min.  将`NULL`指针列入比较范围可以使判断逻辑更简单. 代码更简洁.

我也注意到了其他的解法. 不需要使用heap, 反复使用merge_two即可. 虽然看起来容易, 但是这里涉及到一个精妙的设计:　Devide and conqure 可以实现Nlgk的时间复杂度, N是总的数目.

 在这种方法是, 相邻的两个成对, 对他们进行merge. merge一轮访问~N次数组, merge过之后lists数目变为1/2k; 然后我们进行第二轮merge, 同样这轮会访问~N次数组, merge过后lists长度变为1/4k....所以时间复杂度为 Nlgk

 而如果从头开始, merge头两个, 然后将结果和第三个merge... 这样下去如果每个子序列长度为n, 共有k个子序列. 那么结果会是~n*k^2 time cost 会是前两种的十倍.
