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
Same trick for post order 


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

<hr>

## 113. Path Sum II

Brute Force
```
class Solution {
public:
    void DFS(TreeNode* root,vector<vector<int>> &result,vector<int> &path,int curSum,int target){
        if(!root) return;
        curSum += root->val;
        path.push_back(root->val);
        if(curSum == target and !root->left and !root->right){
            // substitute tmp
            result.push_back(path);
        }
        else{
            DFS(root->left,result,path,curSum,target);
            DFS(root->right,result,path,curSum,target);
        }
        // pop current node
        path.pop_back();
    }
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<vector<int>> result;
        vector<int> path;
        DFS(root,result,path,0,sum);        
        return result;
    }
};
```

<hr>

## 298 Binary Tree Longest Consecutive Sequence

```
class Solution {
public:
    /**
     * @param root: the root of binary tree
     * @return: the length of the longest consecutive sequence path
     */
    void DFS(int &result, TreeNode* root, int lastCnt,int lastValue){
        if(!root){
            return ;
        }
        if(root->val == lastValue + 1){
            lastCnt++;
        }
        else{
            lastCnt = 1;
        }
        result = max(result,lastCnt);
        lastValue = root->val;
        DFS(result,root->left,lastCnt,lastValue);
        DFS(result,root->right,lastCnt,lastValue);
    }
    int longestConsecutive(TreeNode * root) {
        int result = 0;
        if(!root) return 0;
        DFS(result,root,0,root->val-1);
        return result;
        // write your code here
    }
};
```

<hr>

## 235. Lowest Common Ancestor of a Binary Search Tree
Find left and right, mark the first found node as the result.

`If(curState.first), then TreeNode p exists. If(curState.second), then node q exits in current State`
Our goal is to find the first node that have both curState.first and curState.scond is true.

```
class Solution {
public:
    pair<bool,bool> DFS(TreeNode* root,TreeNode*p,TreeNode* q,TreeNode** Result){
        if(!root) return {false,false};
        auto left = DFS(root->left,p,q,Result);
        auto right = DFS(root->right,p,q,Result);
        bool containsP = root == p or left.first or right.first;
        bool containsQ = root == q or left.second or right.second;
        //if not find the target yet, then this is the result, otherwise skip this.
        if((*Result) == NULL and containsP and containsQ ){
            *Result = root;
        }
        return {containsP,containsQ};
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* result = NULL;
        DFS(root,p,q,&result);
        return result;
    }
};
```

<hr>

## 614. Binary Tree Longest Consecutive Sequence II
Firstly, use `pair<int,int>` to denote the number of incresing nodes and decresing nodes.

And get the situation of two sub trees.

If `current->val == parent->val +1`, then return the correct increasing nodes values.
Else if `current->val == parent->val-1`, find the correct decreasing node values;
Else this node will not help the parent to form a sequence, just return `{0,0}`

```
class Solution {
public:
    /**
     * @param root: the root of binary tree
     * @return: the length of the longest consecutive sequence path
     */
     
    pair<int,int> helper(TreeNode*root,TreeNode*parent,int &res){
        if(!root){
            return pair<int,int>{0,0};
        }
        auto leftState = helper(root->left,root,res);
        auto rightState = helper(root->right,root,res);
        res = max(res,leftState.first+rightState.second+1);
        res = max(res,leftState.second+rightState.first+1);
        int inc=0,rec=0;
        if(root->val==parent->val + 1){
            inc = max(leftState.first,rightState.first)+1;
        }
        else if (root->val == parent->val -1){
            rec = max(leftState.second,rightState.second)+1;
        }
        return {inc,rec};
    }
    int longestConsecutive2(TreeNode * root) {
        // write your code here
        int result = 0;
        helper(root,root,result);
        return result;
    }
};
```

<hr>

## 108. Convert Sorted Array to Binary Search Tree
```
class Solution {
public:
    TreeNode* convert(vector<int>&nums,int left,int right){
        if(left>right) return NULL;
        int mid = (left+right)/2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = convert(nums,left,mid-1);
        root->right = convert(nums,mid+1,right);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode* root;
        int left=0,right = nums.size()-1;
        root = convert(nums,left,right);
        return root;
    }
};
```

<hr>

## 124. Binary Tree Maximum Path Sum
Careful about the condition, easier than the 614 problem
```
int CalRootPathSum(TreeNode* root,int &result){
    if(!root) return 0;
    int leftVal = CalRootPathSum(root->left,result);
    int rightVal = CalRootPathSum(root->right,result);
    //cal max possible childVal
    int maxChildVal = max(leftVal,rightVal);
    int calResultValue = max(maxChildVal,leftVal+rightVal);
    result =max( max(calResultValue+root->val,root->val),result);
    
    if(maxChildVal>0) return maxChildVal+root->val;
    return root->val;
}
int maxPathSum(TreeNode* root) {
    int result = INT_MIN ;
    CalRootPathSum(root,result);
    return result;
}
```

<hr>

## 250.	Count Univalue Subtrees(LintCode 921)
Brute force, record all child values and put it in a set

#### Brute force  
```
unordered_set<int>* DFS(int&result, TreeNode* root,TreeNode*parent){
    unordered_set<int> *returnSet = new unordered_set<int>;
    if(!root) returnSet->insert(parent->val);
    
    else{
        unordered_set<int>* leftValue = DFS(result,root->left,root);
        unordered_set<int>* rightValue = DFS(result,root->right,root);
        //it is pure
        if(leftValue->size() == 1 and leftValue->count(root->val) == 1 and rightValue->size() == 1 and rightValue->count(root->val) == 1){
            // cout<<root->val<<endl;
            result ++;
        }
        for(auto it = leftValue->begin(); it!=leftValue->end();it++){
            // cout<<"inserted:"<<*it<<endl;
            returnSet->insert(*it);
        }
        for(auto it = rightValue->begin(); it!=rightValue->end();it++){
            returnSet->insert(*it);
            // cout<<"inserted:"<<*it<<endl;

        }
        returnSet->insert(root->val);
        
        delete(leftValue);
        delete(rightValue);
    }
    
    return returnSet;
}
int countUnivalSubtrees(TreeNode * root) {
    // write your code here
    if(!root) return 0;
    int result = 0;
    DFS(result,root,root);
    return result;
}
```
### Brute force without pointer
```
unordered_set<int> DFS(TreeNode* root, int&result){
    unordered_set<int> empty;
    if(!root){
        return empty;
    }
    auto left = DFS(root->left,result);
    auto right = DFS(root->right,result);
    if( (left.size() ==0 or (left.size() == 1 and left.count(root->val) == 1)) and ((right.size() == 1 and right.count(root->val) == 1) or right.size()==0)){
        result++;
    }
    for(auto it = left.begin();it!=left.end();it++){
        empty.insert(*it);
    }
    for(auto it = right.begin();it!=right.end();it++){
        empty.insert(*it);
    }
    empty.insert(root->val);
    return empty;
}
int countUnivalSubtrees(TreeNode * root) {
    // write your code here
    int result = 0;
    DFS(root,result);
    return result;
}
```

#### Simplified Version
```
//return if all subtree from current node is uni-value
bool DFS(int&result, TreeNode* root,TreeNode*parent){
    if(!root) return true;
    bool left = DFS(result,root->left,root);
    bool right = DFS(result,root->right,root);
    if(left and right){
        result++;
    }
    if(!left or !right or root->val != parent->val){
        return false;
    }
    return true;
}
int countUnivalSubtrees(TreeNode * root) {
    // write your code here
    if(!root) return 0;
    int result = 0;
    DFS(result,root,root);
    return result;
}
```

<hr>

## 366.Find Leaves of Binary Tree(Lintcode 650)
Using Hash, O(n) time and O(N) space for a tree with N nodes
#### Brute Force
```
int calHeight(TreeNode*root,unordered_map<int,vector<int>>& uMap){
    if(!root) return 0;
    int left = calHeight(root->left,uMap);
    int right = calHeight(root->right,uMap);
    int height = max(left,right)+1;
    auto it = uMap.find(height);
    if(it == uMap.end()){
        vector<int> tmp;
        tmp.push_back(root->val);
        uMap[height] = tmp;
    }
    else{
        uMap[height].push_back(root->val);
    }
    return height;
}
vector<vector<int>> findLeaves(TreeNode * root) {
    vector<vector<int>> result;
    unordered_map<int,vector<int>> uMap;
    calHeight(root,uMap);
    for(int i=1;i<=uMap.size();i++){
        result.push_back(uMap[i]);
    }
    return result;
}
```

<hr>

## 337 House Robber III	
[Sample solution](https://www.cnblogs.com/grandyang/p/5275096.html)

This problem is don't matter for layer, don't be cheated by the test example!!!

This problem matters about directly connected! So we need to split all directly connected nodes. And do a combiniation sum. 

#### Don't work!

Don't work because this cannot find maximum in differnt sub-trees and different layer, for example 
`[2,1,3,null,4]`
```
void DFS(int&result, int curSum,TreeNode*root, bool canSteal){
    if(!root) return;
    //Steal current node, then next child we can't add
    if(canSteal){
        DFS(result,curSum+root->val,root->left,false);
        DFS(result,curSum+root->val,root->left,false);
        result = max(curSum+root->val,result);
    }
    //don't steal current node
    DFS(result,curSum,root->left,true);
    DFS(result,curSum,root->right,true);
    result = max(curSum,result);
}
int rob(TreeNode* root) {
    int result = 0;
    DFS(result,0,root,true);
    return result;
}
```

#### Brute Force
The return value of DFS here is tricky, it means that from current subtree(including the root node), the maximum possible value. Remember to use a hash map here otherwise it will TLE.

Time O(N), space O(N)
```
class Solution {
public:
    // don't steal current node or steal current node
    // return the maximum number of that from current subtree node can steal
    int DFS(TreeNode* root,unordered_map<TreeNode*,int>& uMap){
        if(!root) return 0;
        int val = 0;
        auto it = uMap.find(root);
        if(it == uMap.end()){
            if(root->left){
                val += DFS(root->left->left,uMap) + DFS(root->left->right,uMap);
            }
            if(root->right){
                val += DFS(root->right->left,uMap) + DFS(root->right->right,uMap);
            }
            val = max(val+root->val, DFS(root->left,uMap)+DFS(root->right,uMap));
            uMap[root] = val;
        }
        else{
            val = uMap[root];
        }
        return val;
    }
    int rob(TreeNode* root) {
        unordered_map<TreeNode*,int> uMap;
        return DFS(root,uMap);
    }
};
```

#### Tricky Solution
We can imitate Leetcode 235, return a pair, the first numebr is including this node, the second number is don't include this node.

The tricky part is that it don't steal this node. You can steal child or not steal child, choose the maximum solution.

Time:O(N), Space:O(1)
```
 pair<int,int> DFS(TreeNode* root){
    if(!root) return {0,0};
    auto left = DFS(root->left);
    auto right = DFS(root->right);
    int stealThis = root->val + left.second + right.second;
    // if don't steal this node. For the child, you can steal child or not, select the maximum value here
    int notStealThis = max(left.first,left.second)+max(right.first,right.second);
    // printf("%d %d %d %d %d\n",root->val,left.first,left.second,right.first,right.second);
    return {stealThis,notStealThis};
}
int rob(TreeNode* root) {
    auto result = DFS(root);
    // printf("%d %d\n",result.first,result.second);
    return max(result.first,result.second);
}
```

## 103 Binary Tree Zigzag Level Order Traversal	
One idea is to not reverse the whole vector, but push from right to left. But his is a big mistake
becasue `[1,2,3,4,null,null,5]`, because 2 was pushed to queue prior to 3, so 4 will be pushed to queue earlier than 5. Which is a big mistake for this solution
#### failed solution
```
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> myQueue;
        myQueue.push(root);
        bool right = true;
        while(!myQueue.empty()){
            vector<int> tmp;
            int n = myQueue.size();
            for(int i=0;i<n;i++){
                TreeNode* curNode = myQueue.front(); myQueue.pop();
                if(!curNode) continue;
                if(right){
                    myQueue.push(curNode->right);
                    myQueue.push(curNode->left);
                }
                else{
                    myQueue.push(curNode->left);
                    myQueue.push(curNode->right);
                }
                tmp.push_back(curNode->val);
            }
            right = !right;
            if(tmp.size()){
                result.push_back(tmp);            
            }
        }
        return result;
    }
};
```

#### passed easy solution
```
 vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> result;
    queue<TreeNode*> myQueue;
    myQueue.push(root);
    bool right = false;
    while(!myQueue.empty()){
        vector<int> tmp;
        int n = myQueue.size();
        for(int i=0;i<n;i++){
            TreeNode* curNode = myQueue.front(); myQueue.pop();
            if(!curNode) continue;
            myQueue.push(curNode->left);
            myQueue.push(curNode->right);
            tmp.push_back(curNode->val);
        }
        cout<<right<<endl;
        if(right){
            reverse(tmp.begin(),tmp.end());
        }
        else{;}
        if(tmp.size()) result.push_back(tmp);
        right = !right;
        
    }
    return result;
}
```

## 199. Binary Tree Right Side View
Bad idea is to recursion search the tree, if there is a right node, then travel right node,else 
travel left node. But if the left tree's height is higher than right tree, this method will fail.
We must use BFS to travel the whole tree
```
 void helper(vector<int> & result,TreeNode* root){
    if(!root) return;
    result.push_back(root->val);
    if(!root->right) helper(result,root->left);
    else helper(result,root->right);
}
vector<int> rightSideView(TreeNode* root) {
    vector<int> result;
    helper(result,root);
    return result;
}
```

#### BFS
Time O(N), Space O(1)(if we exclude the necessary O(h) space for result)
```
vector<int> rightSideView(TreeNode* root) {
    vector<int> result;
    queue<TreeNode*> myQueue;
    myQueue.push(root);
    while(!myQueue.empty()){
        int n = myQueue.size();
        int lastVal = INT_MIN;
        bool exist = false;
        for(int i=0;i<n;i++){
            TreeNode* curNode = myQueue.front(); myQueue.pop();
            if(!curNode) continue;
            exist = true;
            lastVal = curNode->val;
            myQueue.push(curNode->left);
            myQueue.push(curNode->right);
        }
        if(exist){
            result.push_back(lastVal); 
        }
    }
    
    return result;
}
```

## 98 Validate Binary Search Tree
#### Simple DFS
```
// each left subtree has a upper bound, and right subtree has a lower bound
bool helper(TreeNode* root,long long  lower,long long upper){
    if(!root) return true;
    return (root->val > lower && root->val < upper) and helper(root->left,lower,root->val) and helper(root->right,root->val,upper) ;
}
bool isValidBST(TreeNode* root) {
    long long minINF = INT_MIN*1e5;
    long long maxINF = INT_MAX*1e5;
    return helper(root,minINF,maxINF);
}
```

## 109 Convert Sorted List to Binary Search Tree
Convert to 108, using an array to store all values: Space:O(n), Time:O(N), it is easy

Another solution is to use a fast-slow pointer to get the midpoint of linkedlist, thus we can recursively solve this problem. Time: O(N^2), Space:O(1)

#### fast-slow pointer Solution
```
 // include head but not include tail
    TreeNode* convert(ListNode* head, ListNode* tail){
        if(head == NULL or head == tail) return NULL;
        ListNode* slow,*fast;
        slow = fast = head;
        while(fast!= tail and fast->next!= tail){
            fast = fast->next->next;
            slow = slow->next;
        }
        TreeNode* cur = new TreeNode(slow->val);
        cur->left = convert(head,slow);
        cur->right = convert(slow->next, tail);
        return cur;
    }
    TreeNode* sortedListToBST(ListNode* head) {
        return convert(head,NULL);
    }
```


## 230. Kth Smallest Element in a BST

```
int kthSmallest(TreeNode* root, int k) {
    stack<TreeNode*> myStack;
    myStack.push(root);
    int cnt = 0;
    while(!myStack.empty()){
        TreeNode* curRoot = myStack.top();
        myStack.pop();
        if(!curRoot) continue;
        if(!curRoot->left and !curRoot->right){
            cnt ++;
            if(cnt == k) return curRoot->val;
            continue;
        }
        myStack.push(curRoot->right);
        TreeNode* tmp = new TreeNode(curRoot->val);
        myStack.push(tmp);
        myStack.push(curRoot->left);
    }
    return 0;
}
```

#### Follow up:
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

Answer: maintain the rank for each node, the meaning of rank is that how much nodes are smalller than current node, which is the number of nodes of left subtree. Maintain this is O(logn) for insertion and deletion, and by using rank, it takes O(logn) to find the kth smallest element.
(Geek for Geeks answer)[https://www.geeksforgeeks.org/find-k-th-smallest-element-in-bst-order-statistics-in-bst/]

<hr>

## 297. Serialize and Deserialize Binary Tree
The key to this problem is how we process the recursion.

One trick is to remove the current value when we are proceeding. So just use a queue to do that.

First parse all strings to a queue. Then remove the token one by one from queue head, with the same order we push the token in.

**Note**: Must use queue, if use vector, it would be TLE
```
class Codec {
public:

    void inOrder(TreeNode* root, string &result){
        if(!root) result += " # ";
        else{
            result += " "+to_string(root->val)+" ";
            inOrder(root->left,result);
            inOrder(root->right,result);
        }
    }
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string result;
        inOrder(root,result);
        return result;
    }

   
    void split(string data,char delim,queue<string>&elem,bool skipEmpty){
        istringstream iss(data);
        for(string item; getline(iss,item,delim); ){
            if(skipEmpty and item.size()==0) continue;
            else elem.push(item);
        }
    }
     TreeNode* deserializeHelper(queue<string> &data){
        if(0 == data.size()){
            return NULL;
        }
        string curString = data.front();
         data.pop();
        if(curString == "#"){
            return NULL;
        }
        else{
            int x;
            sscanf(curString.c_str(),"%d",&x);
            TreeNode* cur = new TreeNode(x);
            cur->left = deserializeHelper(data);
            cur->right = deserializeHelper(data);
            return cur;
        }
    }
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> elem;
        split(data,' ',elem,true);
        return deserializeHelper(elem);
    }
};
```

<hr>

## 285. Inorder Successor in BST(Lintcode: 448)

#### Brute Force for none-BST
```
//state 0: not found, state 1: next , state2: found
void findNext(TreeNode* cur,TreeNode* p, int &state,TreeNode** result){
    if(!cur) return;
    findNext(cur->left,p,state,result);
    if(state == 1){
        state = 2;
        *result = cur;
        return ;
    }
    if(state == 0 and p == cur){
        state = 1;
    }
    findNext(cur->right,p,state,result);
}
TreeNode * inorderSuccessor(TreeNode * root, TreeNode * p) {
    // write your code here
    TreeNode* result = NULL;
    int state = 0;
    findNext(root,p,state,&result);
    return result;
}
```

 
#### If it is BST, find next larger number!
1. if right subtree exist, find min of right subtree 
2. The succssor must be an ancestor(or not exist if this node is the laregst of the whole tree).
 (because its an BST tree, this case means that p is the smallest node in a **leftpart** of a subtree, we need to find the root of this subtree)

```
TreeNode* minNode(TreeNode* root){
    if(root->left){
        return minNode(root->left);
    }
    return root;
}
TreeNode * inorderSuccessor(TreeNode * root, TreeNode * p) {
    // write your code here
    if(p->right){
        return minNode(p->right);
    }
    TreeNode* succsor = NULL;
    while(root){
        if(root->val > p->val){
            succsor = root;
            root = root->left;
        }
        else if(root->val < p->val){
            root = root->right;
        }
        else{
            break;
        }
    }
    return succsor;
}
```

<hr>

## 270. Closest Binary Search Tree Value(Lintcode: 900)
#### Brute Force 
O(N)
```
void DFS(double&diff,int& result,TreeNode*root,double target){
    if(!root) return;
    if(diff > abs(root->val-target)){
        diff = abs(root->val-target);
        result = root->val;
    }
    DFS(diff,result,root->left,target);
    DFS(diff,result,root->right,target);
}
int closestValue(TreeNode * root, double target) {
    // write your code here
    double diff = INT_MAX;
    int result = root->val;
    DFS(diff,result,root,target);
    return result;
}
```

#### Approach this node
O(h)
```
//Find succssor and predecessor
int closestValue(TreeNode * root, double target) {
    // write your code here
    TreeNode* tmpRoot = root;
    int result = root->val;
    double diff = 1e30;
    while(tmpRoot){
        if(abs(tmpRoot->val - target) < diff){
            diff = abs(tmpRoot->val - target);
            result = tmpRoot->val;
        }
        if(tmpRoot->val > target){
            tmpRoot = tmpRoot->left;
        }
        else if(tmpRoot->val < target){
            tmpRoot = tmpRoot->right;
        }
        else break;
    }
    return result;
}
```
<hr>

## 272.Closest Binary Search Tree Value II	(Lintcode 901)

#### Brute Force
```
void inOrder(vector<int>&nums,TreeNode* root){
    if(!root) return;
    inOrder(nums,root->left);
    nums.push_back(root->val);
    inOrder(nums,root->right);
}
int BinarySearch(vector<int> &nums,double target){
    int left=0,right= nums.size()-1;
    while(left+1<right){
        int mid = (left+right)/2;
        if(nums[mid]>target){
            right = mid;
        }
        else{
            left = mid;
        }
    }
    if(abs(nums[left]-target) < abs(nums[right]-target)){
        return left;
    }
    return right;
}
vector<int> closestKValues(TreeNode * root, double target, int k) {
    vector<int> nums;
    inOrder(nums,root);
    int pos = BinarySearch(nums,target);
    vector<int> result;
    while(k>0){
        result.push_back(nums[pos]);
        k--;
        int next = pos;
        if(pos-1>=0 and pos+1<=nums.size()-1){
            if(abs(target-nums[pos-1])<abs(target-nums[pos+1])){
                next = pos-1;
            }
        }
        else if(pos+1>nums.size()-1){
            next = pos-1;
        }
        nums.erase(nums.begin()+pos);
        pos = next;
    }
    return result;
}
```

#### Tricky
Create a queue size K, once its full pop the front.

When we do an inorder traversal, add nodes in the path to the queue.

<hr>

## Morris Traversal
Using Morris Traversal, we can traverse the tree without using stack and recursion. The idea of Morris Traversal is based on Threaded Binary Tree(线索二叉树). In this traversal, we first create links to Inorder successor and print the data using these links, and finally revert the changes to restore original tree.
### Threaded Binary Tree
[GfG threaded ](https://www.geeksforgeeks.org/threaded-binary-tree/)


#### How to convert a Given Binary Tree to Threaded Binary Tree?
If left don't exist, then add current node. 

Else, find the previous for this node. Then go throught the left tree. If previous already find, then `previous->next = cur`, and all the left subtree has already been visited. Visit current node and visit right subtree


Codes for inorder traversal
```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode *root) {
        vector<int> res;
        if (!root) return res;
        TreeNode *cur, *pre;
        cur = root;
        while (cur) {
            if (!cur->left) {
                res.push_back(cur->val);
                cur = cur->right;
            } else {
                pre = cur->left;
                while (pre->right && pre->right != cur) pre = pre->right;
                if (!pre->right) {
                    pre->right = cur;
                    cur = cur->left;
                } else {
                    pre->right = NULL;
                    res.push_back(cur->val);
                    cur = cur->right;
                }
            }
        }
        return res;
    }
};
```

<hr>

## 99. Recover Binary Search Tree

#### Brute Force
```
/*
class Solution {
public:
    
    void inOrder(vector<pair<int,TreeNode*>>&nums,TreeNode* root){
        if(!root) return;
        inOrder(nums,root->left);
        nums.push_back({root->val,root});
        inOrder(nums,root->right);
    }
    void recoverTree(TreeNode* root) {
        vector<pair<int,TreeNode*>> nums;
        inOrder(nums,root);
        vector<int> nums2;
        for(int i=0;i<nums.size();i++){
            nums2.push_back(nums[i].first);
        }
        sort(nums2.begin(),nums2.end());
        TreeNode* a=NULL,*b=NULL;
        for(int i=0;i<nums2.size();i++){
            if(nums2[i] != nums[i].first){
                if(!a){
                    a = nums[i].second;
                }
                else{
                   b = nums[i].second;
                    break;
                }
            }
        }
        swap(a->val,b->val);
    }
};
```


#### Morris Traversal(TODO:need review)
We need to find two nodes that are not in place, the characteristic 
of those two nodes is that they are not in order in the coresponding 
position of an array.

```
// Now O(1) space complexity
class Solution {
public:
    void recoverTree(TreeNode* root) {
        TreeNode *first = NULL, *second = NULL, *parent = NULL;
        TreeNode *cur, *pre;
        cur = root;
        while (cur) {
            if (!cur->left) {
                if (parent && parent->val > cur->val) {
                    if (!first) first = parent;
                    // those two numbers might be adjacent, might not. 
                    // So if I do else{second = parent;break}, the second might be NULL if it is adjacnet
                    second = cur;
                }
                parent = cur;
                cur = cur->right;
            } else {
                pre = cur->left;
                while (pre->right && pre->right != cur) pre = pre->right;
                if (!pre->right) {
                    pre->right = cur;
                    cur = cur->left;
                } else {
                    pre->right = NULL;
                    if (parent->val > cur->val) {
                        if (!first) first = parent;
                        second = cur;
                    }
                    parent = cur;
                    cur = cur->right;
                }
            }
        }
        swap(first->val, second->val);
    }
};
```

<hr>

## 156 Binary Tree Upside Down(Lint 649)
题意难以理解，这道题让我们把一棵二叉树上下颠倒一下，而且限制了右节点要么为空要么一定会有对应的左节点。上下颠倒后原来二叉树的最左子节点变成了根节点，其对应的右节点变成了其左子节点，其父节点变成了其右子节点，相当于顺时针旋转了一下。
#### 找右子节点
这里的tricky的部分是新的子树的最右节点就是原来根节点的左子节点，因为翻转左子树的过程中，原来左子树的根节点会变成最右节点
```
TreeNode *upsideDownBinaryTree(TreeNode *root) {
    if (!root || !root->left) return root;
    TreeNode *l = root->left, *r = root->right;
    TreeNode *res = upsideDownBinaryTree(l);
    root->left = root->right = NULL;
    TreeNode* rightest = res;
    while(rightest->right) rightest = rightest->right;
    rightest->left = r;
    rightest->right = root;
    return res;
}
```
#### 不找右子节点
```
//顺时针旋转
    TreeNode *upsideDownBinaryTree(TreeNode *root) {
        if (!root || !root->left) return root;
        TreeNode *l = root->left, *r = root->right;
        TreeNode *res = upsideDownBinaryTree(l);
        l->left = r;
        l->right = root;
        root->left = NULL;
        root->right = NULL;
        return res;
    }
```

#### 迭代
注意最后return pre，因为cur = NULL
```

class Solution {
public:
     //不断向左，翻转到最后就是根节点
    TreeNode * upsideDownBinaryTree(TreeNode * root) {
        TreeNode* pre=NULL,*tmp=NULL,*next=NULL,*cur=root;
        while(cur){
            next = cur->left;
            cur->left = tmp;
            tmp = cur->right;
            cur->right=  pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
};
```

<hr>



## 114. Flatten Binary Tree to Linked List
Preorder traversal
```
class Solution {
public:
    TreeNode* helper(TreeNode* root){
        if(!root) return NULL;
        if(root->left){
            auto tmp = root->left;
            while(tmp->right) tmp = tmp->right;
            tmp->right = root->right;
        }
        else root->left = root->right;
        root->right = helper(root->left);
        root->left = NULL;
        return root;
    }
    void flatten(TreeNode* root) {
        helper(root);
    }
};
```

<hr>

## 255 Verify Preorder Sequence in Binary Search Tree   (Lint 1307)
#### Brute Force
Find the first number that is larger than root, which means it shall be the subright tree. Then judge sub-left tree and sub-right tree.

但由于使用了递归，每个节点都被访问了一次，而且每次要寻找一次数组，因此复杂度是O(n^2)
```
class Solution {
public:
    bool helper(vector<int>&preorder,int l,int r,int upperBound,int lowerBound){
        if(l > r) return true;
        int rootVal = preorder[l];
        if(rootVal >= upperBound or rootVal <= lowerBound) return false;
        if(l == r) return true;
        int mid = l+1;
        while(mid <= r and preorder[mid] < rootVal){
            mid++;
        }
        // cout<<"l="<<l<<",r="<<r<<".mid="<<mid<<endl;
        //mid find the first number larger than root.
        //mid > r means all is left sub tree
        if(mid > r){
            return helper(preorder,l+1,r,rootVal,lowerBound);
        }
        return helper(preorder,l+1,mid-1,rootVal,lowerBound) and helper(preorder,mid,r,upperBound,rootVal);
    }
    
    bool verifyPreorder(vector<int> &preorder) {
        return helper(preorder,0,preorder.size()-1,INT_MAX,INT_MIN);
    }
};
```
#### Stack solution
```
class Solution {
public:
     // we set a lower bound, if cur->val < lower bound, then it is false.
     // root, left ,right, so we push root and left to stack ,if cur value is smaller than number in stack, it is in left-sub tree. otherwise it is in right subtree. We need to pop the stack to find the root of that right sub tree, the right subtree is exaclty larger than the root. And we push that right node to the stack. So the lower bound would be the value of the root.
    bool verifyPreorder(vector<int> &preorder) {
        int lowerBound = INT_MIN;
        stack<int> s;
        for(int i=0;i<preorder.size();i++){
            if(preorder[i] < lowerBound) return false;
            if(s.empty() or preorder[i]<s.top()) s.push(preorder[i]);
            else{
                //preorder[i] > s.top()
                while(!s.empty() and preorder[i] > s.top()){
                    lowerBound = s.top(); s.pop();
                }
                //the root of right substree
                s.push(preorder[i]);
            }
        }
        return true;
    }
};
```
#### O(1) space
直接把value存到preorder遍历过的点里面,把它作为一个栈
```
class Solution {
public:
    bool verifyPreorder(vector<int> &preorder) {
        int lowerBound = INT_MIN; int lowIndex = -1;
        for(int i=0;i<preorder.size();i++){
            if(preorder[i] < lowerBound) return false;
            //using preorder itself as a stack
            while(lowIndex >= 0 and preorder[i] > preorder[lowIndex]){
                lowerBound = preorder[lowIndex];
                lowIndex--;
            }
            //store the root of right sub tree
            preorder[++lowIndex] = preorder[i];
        }
        return true;
    }
};
```
<hr>

## 333 Largest BST Subtree	
Find the size of the largest binary seach tree
#### Wrong solution!!! 
Becasue with this input, the largest subtree is the tree with root 60. But the lower bound of it is actually not 50. other wise we can only find 70 as the result.
```
       50
     /    \
  30       60
 /  \     /  \ 
5   20   45    70
              /  \
            65    80
```

```
bool isBST(TreeNode** result,TreeNode* root,int LowerBound,int upperBound){
    if(!root) return true;
    printf("val=%d,h=%d,l=%d\n",root->val,upperBound,LowerBound);
    if(root->val < LowerBound or root->val > upperBound) return false;
    bool l = isBST(result,root->left,LowerBound,root->val);
    bool r = isBST(result,root->right,root->val,upperBound);
    if(root->val == 60){
        cout<<60<<endl;
        cout<<l<<" "<<r<<endl;
    }
    if(l and r){
        printf("candidate:%d\n",root->val);
        *result = root; return true;
    }
    return false;
}
int calSize(TreeNode* root){
    if(!root) return 0;
    int l = calSize(root->left);
    int r = calSize(root->right);
    return l+r+1;
}
int findLargestBSTTree(TreeNode* root){
    TreeNode* result = NULL;
    isBST(&result,root,INT_MIN,INT_MAX);
    if(result == NULL) return 0;
    cout<<"result Node val = "<<result->val<<endl;
    return calSize(result);
}
```

#### Brute Force
Travel for each subtree

Time Consuming, we can add a memo for quicker reference
```
bool isBST(TreeNode*root,int low,int high){
    if(!root) return true;
    if(root->val < low or root->val > high) return false;
    if(isBST(root->left,low,root->val) and isBST(root->right,root->val,high)){
        return true;
    }
    return false;
}

int calSize(TreeNode* root){
    if(!root) return 0;
    int l = calSize(root->left);
    int r = calSize(root->right);
    return l+r+1;
}
int findLargestBSTTree(TreeNode* root){
    if(isBST(root,INT_MIN,INT_MAX)) return calSize(root);
    return max(findLargestBSTTree(root->left),findLargestBSTTree(root->right));
}
```

#### Brute force with memo
```
unordered_map<TreeNode*,bool> uMap;
bool isBST(TreeNode*root,int low,int high){
    if(!root) return true;
    auto it = uMap.find(root);
    if(it!=uMap.end()) return it->second;
    bool result;
    if(root->val < low or root->val > high) result =  false;
    else if(isBST(root->left,low,root->val) and isBST(root->right,root->val,high)){
        result =  true;
    }
    else{
        result = false;
    }
    uMap[root] = result;
    return result;
}

int calSize(TreeNode* root){
    if(!root) return 0;
    int l = calSize(root->left);
    int r = calSize(root->right);
    return l+r+1;
}

int findLargestBSTTree(TreeNode* root){
    if(isBST(root,INT_MIN,INT_MAX)) return calSize(root);
    return max(findLargestBSTTree(root->left),findLargestBSTTree(root->right));
}
```
#### Full runnable C++ code
Can be run [here](https://www.onlinegdb.com/online_c++_compiler)
```
#include <iostream>
#include <stack>
#include <vector>
using namespace std;
#include <limits.h>
#include <stdio.h>
#include <unordered_map>

class TreeNode{
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int val,TreeNode* l=NULL,TreeNode* r=NULL){
        this->val = val;
        left = l;
        right = r;
    }
};


unordered_map<TreeNode*,bool> uMap;
bool isBST(TreeNode*root,int low,int high){
    if(!root) return true;
    auto it = uMap.find(root);
    if(it!=uMap.end()) return it->second;
    bool result;
    if(root->val < low or root->val > high) result =  false;
    else if(isBST(root->left,low,root->val) and isBST(root->right,root->val,high)){
        result =  true;
    }
    else{
        result = false;
    }
    uMap[root] = result;
    return result;
}

int calSize(TreeNode* root){
    if(!root) return 0;
    int l = calSize(root->left);
    int r = calSize(root->right);
    return l+r+1;
}

int findLargestBSTTree(TreeNode* root){
    if(isBST(root,INT_MIN,INT_MAX)) return calSize(root);
    return max(findLargestBSTTree(root->left),findLargestBSTTree(root->right));
}
TreeNode* constructTree(deque<int> &values){
    if(values.size() == 0) return NULL;
    if(values[0] == -1){
        values.pop_front();
        return NULL;
    }
    TreeNode* result = new TreeNode(values[0]);
    values.pop_front();
    result->left = constructTree(values);
    result->right = constructTree(values);
    return result;
}

void TravelTree(TreeNode* root){
    if(!root) return;
    cout<<root->val<<endl;
    TravelTree(root->left);
    TravelTree(root->right);
}

int main()
{
    deque<int> treeValues{50,30,5,-1,-1,20,-1,-1,60,45,-1,-1,70,65,-1,-1,80,-1,-1};
    auto head = constructTree(treeValues);
    // TravelTree(head);
    cout<<"largest BST size = "<<findLargestBSTTree(head)<<endl;
    return 0;
}

```

<hr>

## 222. Count Complete Tree Nodes
```
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(!root) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

<hr>

## 105. Construct Binary Tree from Preorder and Inorder Traversal
#### Bruteforce recursive
O(N^2) because of the process to find the root in inorder
```
class Solution {
public:
    TreeNode* buildTree(vector<int>&preorder,int pl,int pr,vector<int>&inorder,int il,int ir){
        if(pl > pr) return NULL;
        if(pl == pr) return new TreeNode(preorder[pl]);
        TreeNode* result = new TreeNode(preorder[pl]);
        int rootIndex = il;
        while(inorder[rootIndex] != preorder[pl]){rootIndex++;}
        //the size of it is rootIndex-il
        result->left = buildTree(preorder,pl+1,pl+rootIndex-il,inorder,il,rootIndex-1);
        result->right = buildTree(preorder,pl+rootIndex-il+1,pr,inorder,rootIndex+1,ir);
        return result;
    }
    
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return buildTree(preorder,0,preorder.size()-1,inorder,0,inorder.size()-1);
    }
};
```

#### Adding hash map
Add hash map to speed up the process to find the index. Make it faster than 93%
```
class Solution {
public:
    TreeNode* buildTree(vector<int>&preorder,int pl,int pr,vector<int>&inorder,int il,int ir,unordered_map<int,int>& uMap){
        if(pl > pr) return NULL;
        if(pl == pr) return new TreeNode(preorder[pl]);
        TreeNode* result = new TreeNode(preorder[pl]);
        int rootIndex = uMap[preorder[pl]];
        //the size of it is rootIndex-il
        result->left = buildTree(preorder,pl+1,pl+rootIndex-il,inorder,il,rootIndex-1,uMap);
        result->right = buildTree(preorder,pl+rootIndex-il+1,pr,inorder,rootIndex+1,ir,uMap);
        return result;
    }
    
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int,int> uMap;
        for(auto i=0;i<inorder.size();i++) uMap[inorder[i]] = i;
        return buildTree(preorder,0,preorder.size()-1,inorder,0,inorder.size()-1,uMap);
    }
};
```

<hr>

## 106. Construct Binary Tree from Inorder and Postorder Traversal
```

class Solution {
public:
    //For post order, the root is the rear
    TreeNode* buildTreeHelper(vector<int>& inorder,int il,int ir, vector<int>& postorder, int pl,int pr,unordered_map<int,int>& uMap){\
        if(il > ir) return NULL;
        TreeNode* result = new TreeNode(postorder[pr]);
        if(il == ir) return result;
        int rootIndex = uMap[postorder[pr]];
        //inorder:left size = rootIndex-il , right size = rootIndex+1 to ir;
        //postorder:left size: rootIndex-il, right size rootIndex+1 to pr-1
        result -> left = buildTreeHelper(inorder,il,rootIndex-1,postorder,pl,pl+rootIndex-il-1,uMap);
        result -> right = buildTreeHelper(inorder,rootIndex+1,ir,postorder,pl+rootIndex-il,pr-1,uMap);
        return result;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int,int> uMap;
        for(auto i=0;i<inorder.size();i++) uMap[inorder[i]] = i;
        return buildTreeHelper(inorder,0,inorder.size()-1,postorder,0,postorder.size()-1,uMap); 
    }
};
```

<hr>

## 116. Populating Next Right Pointers in Each Node
Simple level order traversal
```

class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> q;
        q.push(root);
        while(!q.empty()){
            int size = q.size();
            Node* pre = NULL;
            for(int i=0;i<size;i++){
                auto cur = q.front();q.pop();
                if(!cur) continue;
                if(i == size - 1) cur->next = NULL;
                if(pre) pre->next = cur;
                pre = cur;
                q.push(cur->left);
                q.push(cur->right);
            }
        }
        return root;
    }
};
```

<hr>

## 117. Populating Next Right Pointers in Each Node II
#### Same solution works
```

class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> q;
        q.push(root);
        while(!q.empty()){
            int size = q.size();
            Node* pre = NULL;
            for(int i=0;i<size;i++){
                auto cur = q.front();q.pop();
                if(!cur) continue;
                if(i == size - 1) cur->next = NULL;
                if(pre) pre->next = cur;
                pre = cur;
                q.push(cur->left);
                q.push(cur->right);
            }
        }
        return root;
    }
};
```

<hr>

## 314 Binary Tree Vertical Order Traversal	(Lint 651)
#### Hash
Attention: this one needs to be level order travel. Not DFS
```
class Solution {
public:
    vector<vector<int>> verticalOrder(TreeNode * root) {
        unordered_map<int,vector<int>> uMap;
        int indexMin=INT_MAX,indexMax=INT_MIN;
        queue<pair<TreeNode*,int>> q;
        q.push({root,0});
        vector<vector<int>> result;
        while(!q.empty()){
            int size = q.size();
            for(int i=0;i<size;i++){
                auto cur = q.front(); q.pop();
                if(!cur.first) continue;
                indexMin = min(indexMin,cur.second);
                indexMax = max(indexMax,cur.second);
                auto it = uMap.find(cur.second);
                if(it == uMap.end()){
                    vector<int> tmp{cur.first->val};
                    uMap[cur.second] = tmp;
                }
                else it->second.push_back(cur.first->val);
                q.push({cur.first->left,cur.second-1});
                q.push({cur.first->right,cur.second+1});
            }
        }
        for(int i=indexMin;i<=indexMax;i++){
            result.push_back(uMap[i]);
        }
        return result;
    }
};
```

<hr>

## 96. Unique Binary Search Trees
#### Brute Force TLE
```
class Solution {
public:
    int generateHelper(int l,int h){
        if(l >= h) return 1;
        int result = 0;
        for(int i=l;i<=h;i++){
            auto left = generateHelper(l,i-1);
            auto right = generateHelper(i+1,h);
            result += (left*right);
        }
        return result;
    }
    int numTrees(int n) {
        if(n == 0) return 0;
        return generateHelper(1,n);    
    }
};
```

#### Using map as memp
Don't use unordered_Map because it can' use pair as a key
```
class Solution {
public:
    int generateHelper(int l,int h,map<pair<int,int>,int>& uMap){
        if(l >= h) return 1;
        auto it = uMap.find(pair<int,int>{l,h});
        if(it != uMap.end()) return it->second;
        int result = 0;
        for(int i=l;i<=h;i++){
            auto left = generateHelper(l,i-1,uMap);
            auto right = generateHelper(i+1,h,uMap);
            result += (left*right);
        }
        return uMap[{l,h}] = result;
    }
    int numTrees(int n) {
        map<pair<int,int>,int> uMap;
        if(n == 0) return 0;
        return generateHelper(1,n,uMap);    
    }
};
```
<hr>

## 95. Unique Binary Search Trees II
#### Brute Force
左边小，右边大即可
```
class Solution {
public:
    vector<TreeNode*> generateHelper(int l,int h){
        vector<TreeNode*> result;
        if(l > h) return {NULL};
        if(l == h) return {new TreeNode(l)};
        for(int i=l;i<=h;i++){
            auto left = generateHelper(l,i-1);
            auto right = generateHelper(i+1,h);
            for(int j=0;j<left.size();j++){
                for(int k=0;k<right.size();k++){
                    TreeNode* root = new TreeNode(i);
                    root->left = left[j];
                    root->right = right[k];
                    result.push_back(root);
                }
            }
        }
        return result;
    }
    vector<TreeNode*> generateTrees(int n) {
        if(n == 0) return {};
        return generateHelper(1,n);
    }
};
```

<hr>

## 331. Verify Preorder Serialization of a Binary Tree
```
class Solution {
public:
    bool isValidSerialization(string preorder) {
        vector<string> tokens;
        string delim = ",";
        int prev = 0,pos = 0;
        do{
            pos = preorder.find(delim,prev);
            if(pos == string::npos) pos = preorder.length();
            string token = preorder.substr(prev,pos-prev);
            if(!token.empty()) tokens.push_back(token);
            prev = pos+delim.length();
        }while(pos < preorder.length() and prev < preorder.length());
        // for(auto s:tokens) cout<<s<<endl;
        stack<string> s;
        if(tokens.size() == 0) return true;
        s.push(tokens[0]);
        int curIndex = 1;
        while(!s.empty()){
            auto cur = s.top(); s.pop();
            if(cur == "#") continue;
            if(curIndex == tokens.size()) return false;
            s.push(tokens[curIndex++]);
            if(curIndex == tokens.size()) return false;
            s.push(tokens[curIndex++]);
        }
        return curIndex == tokens.size();
    }
};
```

<hr>