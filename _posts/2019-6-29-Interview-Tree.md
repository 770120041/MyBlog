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

## 144. Binary Tree Preorder Traversal
Don't use BFS simple soltuion! Because preorder is more like DFS, BFS will watch 
same height first. 

So the right way to simulate recursion is stack.
But remember to push the right first the push the left to stack.

```
vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> myStack;
        myStack.push(root);
        while(!myStack.empty()){
            TreeNode* cur = myStack.top(); myStack.pop();
            if(!cur) continue;
            result.push_back(cur->val);
            
            myStack.push(cur->right);
            myStack.push(cur->left);
        }
        return result;
    }
```


Resursive
```
class Solution {
public:
    void helper(vector<int> &result, TreeNode* root){
        if(!root) return;
        result.push_back(root->val);
        helper(result,root->left);
        helper(result,root->right);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        helper(result,root);
        return result;
    }
};
```


## 94. Binary Tree Inorder Traversal
The trick is add a dummy TreeNode to the stack for the middle one.

```
vector<int> inorderTraversal(TreeNode* root) {
    stack <TreeNode*> myStack;
    myStack.push(root);
    vector<int> result;
    while(!myStack.empty()){
        TreeNode* cur = myStack.top(); myStack.pop();
        if(!cur) continue;
        if(!cur->left && !cur->right){
            result.push_back(cur->val);
            continue;
        }
        TreeNode* tmp = new TreeNode(cur->val);
        myStack.push(cur->right);
        myStack.push(tmp);
        myStack.push(cur->left);
    }
    return result;
}
```

## 102. Binary Tree Level Order Traversal

If want to go over same level first, use this trick below.
```
while(!myQueue.empty()){
    int size = myQueue.size();
    for(int i=0;i<size;i++){
        ...
    }
}
```
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> myQueue;
        myQueue.push(root);
        vector<vector<int>> result;
        if(!root) return result;
        while(!myQueue.empty()){
            int size = myQueue.size();
            vector<int> curLevel;
            for(int i=0;i<size;i++){
                TreeNode* curNode = myQueue.front(); myQueue.pop();
                if(!curNode) continue;
                curLevel.push_back(curNode->val);
                if(curNode->left)
                    myQueue.push(curNode->left);
                if(curNode->right)
                    myQueue.push(curNode->right);
            }
            result.push_back(curLevel);
        }
        return result;
    }
};
```