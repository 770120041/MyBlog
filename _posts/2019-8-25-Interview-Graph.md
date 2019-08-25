---
layout: post
title:  "Graph"
categories: Interview
---

## 133. Clone Graph
#### Brute force
Using hash table to set up a coorespondence between origin node and new node. Then DFS again to connect nodes in new graph.
```
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;

    Node() {}

    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/
class Solution {
public:
    Node* cloneNodes(Node* node, unordered_map<Node*,Node*>& uMap){
        if(!node) return NULL;
        Node* curNode = new Node(node->val);
        auto it =uMap.find(node);
        if(it != uMap.end()) return it->second;
        uMap[node] = curNode;
        for(int i=0;i<node->neighbors.size();i++){
            auto it = uMap.find(node->neighbors[i]);
            if(it == uMap.end()){
                 cloneNodes(node->neighbors[i],uMap);
            }
        }
        return curNode;
    }
    void connect(Node* node,Node* curNode,unordered_map<Node*,Node*>& uMap,unordered_set<Node*>& uSet){
        if(!node) return;
        if(uSet.count(node)) return;
        uSet.insert(node);
        curNode->neighbors = vector<Node*>(node->neighbors.size(),NULL);
        for(int i=0;i<node->neighbors.size();i++){
            curNode->neighbors[i] = uMap[node->neighbors[i]];
            connect(node->neighbors[i],curNode->neighbors[i],uMap,uSet);
        }
    }
    Node* cloneGraph(Node* node) {
        unordered_map<Node*,Node*> uMap;
        auto result = cloneNodes(node,uMap);
        unordered_set<Node*> uSet;
        connect(node,result,uMap,uSet);
        return result;    
    }
};
```

<hr>

