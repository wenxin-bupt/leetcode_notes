### [Binary Tree Longest Consecutive Sequence](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/)

Difficulty **Medium**

tags `binary_tree` `dfs`

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

For example,
```
   1
    \
     3
    / \
   2   4
        \
         5
```         
Longest consecutive sequence path is `3-4-5`, so return `3`.
```
   2
    \
     3
    /
   2    
  /
 1
```
Longest consecutive sequence path is `2-3`,not`3-2-1`, so return `2`.

思路： 简单的DFS

Trick： naive想法是DFS一遍整个树，遇到需要统计长度的结点(val值与其父结点不同的结点)，用DFS计数统计一下。 这是自顶向下的思路，但是稍微修改就可以简化为遍历和统计同时进行的版本。

**solution 1**
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
    int dfsCount(TreeNode *node, int acu, int expect) {
        if (node == NULL) return 0;
        if (node->val != expect) acu = 1;
        return max(max(acu, dfsCount(node->left, acu+1, node->val+1)),
                   dfsCount(node->right, acu+1, node->val+1));
    }
public:
    int longestConsecutive(TreeNode* root) {
        if (root == NULL) return 0;
        // set arg: expect != root->val
        return dfsCount(root, 1, root->val-1);
    }
};
```
