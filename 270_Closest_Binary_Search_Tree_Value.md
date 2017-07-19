### [Closest Binary Search Tree Value](https://leetcode.com/problems/closest-binary-search-tree-value/)   

Difficulty **Easy**

tags `BST`

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

**Note:**

- Given target value is a floating point.
- You are guaranteed to have only one unique value in the BST that is closest to the target.

既然是找最接近的，自然的想法是先找到比这个小的最接近的，然后再找到比这个值大的最接近的， 然后比较两者给出结果。 这样就需要写BST的`floor`和`ceiling` 函数。 对于接近平衡的BST, 时间复杂度是~2logN， 空间复杂度是~logN

但是这里是有trick的:

规律: **BST中和target值最接近的结点，必然位于BST查找target的路径上**。 易证明: 假设target查找失败，且存在BST中最接近target的结点不在查找路径上。 我们取三个结点进行分析，一个是整个BST中最接近target的结点A(A不在查找路径上)， 一个是在查找路径上最接近target的结点B。 可知，这两个点必有一公共祖先结点C, 且C必然在查找路径上。 根据查找时的选择规则， C的值是比A更接近target的。 这样就与A是BST中最接近target的结点相矛盾。 所以，值最接近target的结点必然位于查找路径上。

有了以上规律，我们就可以知道，只要在查找的过程中更新距离target最近的结点， 只要一次查找就可以找到最接近target的结点，且可以用~1空间完成。

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
public:
    int closestValue(TreeNode* root, double target) {
        int res = root->val;
        while (root) {
            if (abs(root->val-target) < abs(res-target)) res = root->val;
            if (root->val > target) root = root->left;
            else if (root->val < target) root = root->right;
            else break;
        }
        return res;
    }
};
```

思路：

用ceiling和floor的方法

**solution 2**

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
    TreeNode* findFloor(TreeNode* node, double target) {
        if (node == NULL) return NULL;
        if (node->val == target) return node;
        if (node->val > target) return findFloor(node->left, target);
        TreeNode* t = findFloor(node->right, target);
        if (t==NULL) return node;
        else return t;
    }
    
    TreeNode* findCeiling(TreeNode *node, double target) {
        if (node == NULL) return NULL;
        if (node->val == target) return node;
        if (node->val < target) return findCeiling(node->right, target);
        TreeNode* t = findCeiling(node->left, target);
        if (t==NULL) return node;
        else return t;
    }
public:
    int closestValue(TreeNode* root, double target) {
        TreeNode* floor = findFloor(root, target);
        TreeNode* ceiling = findCeiling(root, target);
        if (floor == NULL) return ceiling->val;
        if (ceiling == NULL) return floor->val;
        if (ceiling->val - target > target - floor->val) 
            return floor->val;
        else
            return ceiling->val;
        
    }
};
```