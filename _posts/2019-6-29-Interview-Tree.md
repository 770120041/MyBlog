---
layout: post
title:  "Leetcode Tree "
categories: Interview
---
## 110. Balanced Binary Tree
#### Brute force
O(n^2) because cal height at each level
```
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
    int calHeight(TreeNode* root){
       if(!root) return 0;
       if(!root->left && !root->right) return 1;
       return max(calHeight(root->left),calHeight(root->right))+1;
    }
    bool isBalanced(TreeNode* root) {
        if(!root) return true;
        return isBalanced(root->left) && isBalanced(root->right) && abs(calHeight(root->left)-calHeight(root->right))<=1; 
    }
};
```
#### One pass
```
class Solution {
public:
    bool flag= true;
    int calHeight(TreeNode* root){
       if(!root) return 0;
       int left = calHeight(root->left);
       int right = calHeight(root->right);
        if(abs(left-right)>1) flag = false;
        return max(left,right)+1;
    }
    bool isBalanced(TreeNode* root) {
        if(!root) return true;
        calHeight(root);
        return flag;
    }
};
```


