---
layout: post
title:  "Leetcode String"
categories: Interview
---
## 186 Reverse Words in a String II
If reverse word by word, very very hard, because the length of string would be different. So need to move a lot. And if scan from left and right, then we need to detect `left<= right`

So the idea is to: reverse the whole string, then reverse each word

```
class Solution {
public:
    string reverseWords(string s) {
        reverse(s.begin(),s.end());
        int lastNoneSpace = -1;
        for(int i=0;i<s.size();i++){
            if(s[i] != ' '){
                if(lastNoneSpace == -1){
                    lastNoneSpace = i;
                }
            }
            else{
                if(lastNoneSpace == -1){
                    ;
                }
                else{
                    reverse(s.begin()+lastNoneSpace , s.begin() + i );
                    lastNoneSpace = -1;
                }
            }
        }
        if(lastNoneSpace != -1){
            reverse(s.begin()+lastNoneSpace, s.end());
        }
        
        return s;
    }
};
```

<hr>

## 68. Text Justification
#### Greedy Brute Force
```
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        vector<string> result;
        int curWidth = 0;
        vector<int> curWords;
        for(int i=0;i<words.size();){
            if(curWidth + curWords.size()  + words[i].size() <= maxWidth){
                curWidth += words[i].size();
                curWords.push_back(i);
                i++;
            }
            // need to write one line
            else{
                string curLine;
                // only one word
                if(curWords.size() == 1){
                    curLine += words[curWords[0]];
                    for(int j=curLine.size()+1;j<=maxWidth;j++) curLine.push_back(' ');
                }
                else{
                    int whiteNum = (maxWidth-curWidth)/(curWords.size()-1);
                    int moreSpace =  (maxWidth-curWidth)%(curWords.size()-1);
                    for(int j=0;j<curWords.size();j++){
                        curLine += words[curWords[j]];
                        if(j == curWords.size()-1){
                            break;
                        }
                        for(int k=0;k<whiteNum;k++){
                            curLine.push_back(' ');
                        }
                        if(j<moreSpace){
                            curLine.push_back(' ');
                        }
                    }
                }
                result.push_back(curLine);
                curWidth = 0;
                curWords.clear();
            }
        }
        if(!curWords.empty()){
            string curLine;
            for(int j=0;j<curWords.size();j++){
                curLine += words[curWords[j]];
                if(j != curWords.size()-1){
                    curLine.push_back(' ');
                }      
            }
            for(int j=curLine.size()+1;j<=maxWidth;j++) curLine.push_back(' ');
            result.push_back(curLine);
        }
        return result;
    }
};
```

<hr>

## 65. Valid Number
#### Automaton
[blog](https://blog.csdn.net/u012601587/article/details/50560838)
[zhihu](https://zhuanlan.zhihu.com/p/20042325)

All input types
1. Illegal Chracter
2. e
3. dot
4. sign(`+/-`)
5. numbers
6. white space  

All kinds o f States:START, SIGN0, DIG0, DOT, DIG1, EXP, SIGN1, DIG2, UNVALID. Changing states depends on current character.
* 0 初始无输入或者只有space的状态
* 1 输入了数字之后的状态
* 2 前面无数字，只输入了dot的状态
* 3 输入了+/-状态
* 4 前面有数字和有dot的状态
* 5 'e' or 'E'输入后的状态
* 6 输入e之后输入+/-的状态
* 7 输入e后输入数字的状态
* 8 前面有有效数输入之后，输入space的状态

So we can construct a table, each line indicates one state, each column indicates one kind of Input

#### State change table
|        States        	| Illegal Char 	| White Space 	|        Numbers       	|         Dot        	|   E   	|    Sign    	| Final 	|
|:--------------------:	|:------------:	|:-----------:	|:--------------------:	|:------------------:	|:-----:	|:----------:	|-------	|
|         Start        	|      -1      	|     Self    	|          Num         	|         Dot        	|   -1  	|    Sign    	|       	|
|          Num         	|      -1      	|   Trailing  	|         Self         	| Num+Dot or Dot+Num 	| Num+E 	|     -1     	| Legal 	|
|          Dot         	|      -1      	|      -1     	| Num+Dot or Dot + Num 	|         -1         	| Num+E 	|     -1     	|       	|
| Num+Dot or Dot + Num 	|      -1      	|   Trailing  	|         Self         	|         -1         	|   -1  	|     -1     	| Legal 	|
|         Sign         	|      -1      	|      -1     	|          Num         	|         Dot        	|   -1  	|     -1     	|       	|
|         Num+E        	|      -1      	|      -1     	|       Num+E+Num      	|         -1         	|   -1  	| Num+E+Sign 	|       	|
|      Num+E+Sign      	|      -1      	|      -1     	|       Num+E+Num      	|         -1         	|   -1  	|     -1     	|       	|
|       Num+E+Num      	|      -1      	|   Trailing  	|         Self         	|         -1         	|   -1  	|     -1     	| Legal 	|
| Trailing             	| -1           	| Self        	| -1                   	| -1                 	| -1    	| -1         	| Legal 	|

#### Automaton Solution

```
class Solution {
public:
    bool isNumber(string s) {
        vector<vector<int>> stateMachine={
            {-1,0,1,2,-1,4},
            {-1,8,1,3,5,-1},
            {-1,-1,3,-1,-1,-1},
            {-1,8,3,-1,5,-1},
            {-1,-1,1,2,-1,-1},
            {-1,-1,7,-1,-1,6},
            {-1,-1,7,-1,-1,-1},
            {-1,8,7,-1,-1,-1},
            {-1,8,-1,-1,-1,-1}
        };
        int state = 0;
        for(int i=0;i<s.size();i++){
            int input = 0;
            if(s[i] == ' ') input = 1;
            else if(s[i] >= '0' and s[i] <='9') input =2;
            else if (s[i] == '.') input = 3;
            else if (s[i] == 'e') input = 4;
            else if (s[i] == '+' or s[i] == '-') input = 5;
            state = stateMachine[state][input];
            if(state == -1) return false;
        }
        return state == 1 or state == 3 or state == 7 or state == 8;
    }
};
```

<hr>

## 157.Read N Characters Given Read4  
```
// Forward declaration of the read4 API.
int read4(char *buf);

class Solution {
public:
    int read(char *buf, int n) {
        int res = 0;
        int cnt = 0;
        while(true){
            
            int cur = read4(buf+cnt);
            if(cur == 0) break;
            cnt += cur;
        }  
        if(cnt>n){
            buf[n] = '\0'
        } 
        else{
            buf[cnt] = '\0'
        }
        return min(cnt,n);
    }
};
```
<hr>

## 158 Read N Characters Given Read4 II - Call multiple times	(Lintcode 660)

#### Tricky Solution
```
int read4(char *buf);

class Solution {
public:
    int readPos = 0,writePos = 0;
    char buffer[4];
    int read(char *buf, int n) {
        for(int i=0;i<n;i++){
            if(writePos == readPos){
                writePos = read4(buffer);
                readPos = 0;
                if(writePos == 0) return i;
            }
            buf[i] = buffer[readPos++];
        }
        return n;
    }
};
```

<hr>

## *76 Minimum Window Substring    
[blog](https://www.cnblogs.com/grandyang/p/4340948.html)

Using a hash map to check if all the requirement is met.

If `target[c]<=0`, means this character is fulfilled.
#### Sliding Windows
```
class Solution {
public:
    string minWindow(string s, string t) {
        string result;
        unordered_map<char,int> target;
        for(auto c:t) target[c]++;
        for(int l=0,r=0,cnt = 0,minLen=INT_MAX;r<s.size();r++){
            if(target.find(s[r]) == target.end()) continue;
            target[s[r]]--;
            if(target[s[r]]>=0) cnt++;
            while(cnt == t.size()){
                if(target.find(s[l]) == target.end()) l++;
                else if(target[s[l]] + 1 <= 0) target[s[l++]]++;
                else{
                    if(r-l+1 < minLen){
                        minLen = r-l+1;
                        result = s.substr(l,minLen);
                    }
                    break;
                }
            }
        }
        return result;
    }
};
```

<hr>

## *30. Substring with Concatenation of All Words
#### Sliding window brute force
```
class Solution {
public:
    // must be concatenation of each words!
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> result;
        int n = words.size();
        int m = s.size();
        if(n == 0 or m == 0){
            return result;
        }
        unordered_map<string,int> uMap;
        for(auto str:words){
            uMap[str]++;
        }
        int length = words[0].size();
        int totalSize = length * n;
        for(int i=0;i<=m-totalSize;i++){
            unordered_map<string,int> tmpMap = uMap;
            bool Match = true;
            for(int j=0;j<n;j++){
                string curStr = s.substr(i+j*length,length);
                auto it = tmpMap.find(curStr);
                if(it != tmpMap.end()){
                    tmpMap[curStr]--;
                    if(tmpMap[curStr] < 0){
                        Match = false;
                        break;
                    }
                }
                else{
                    Match = false;
                    break;
                }
            }
            if(Match){
                result.push_back(i);
            }
        }
        
        return result;
    }
};
```

<hr>

## 3. Longest Substring Without Repeating Characters
#### Sliding window brute force
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char,int> uMap;
        int maxLen = 0;
        for(int l=0,r=0;r<s.size();r++){
            char cur = s[r];
            uMap[cur]++;
            while(uMap[cur]>1){
                uMap[s[l++]]--;
                if(uMap[cur] == 1){
                    break;
                }
            }
            maxLen = max(maxLen, r-l+1);
        }
        return maxLen;
    }
};
```

<hr>

## 340.Longest Substring with At Most K Distinct Characters	(Lintcode 386, )
#### Sliding Window
Space O(K+1) Time(O(N))
```
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string &s, int k) {
        unordered_map<char,int> uMap;
        int charCnt = 0;
        int maxLen = 0;
        for(int l=0,r=0; r < s.size() ; r++){
            uMap[s[r]]++;
            if(uMap[s[r]] == 1) charCnt++;
            while(charCnt > k){
                uMap[s[l]]--;
                if(uMap[s[l]] == 0) charCnt --;
                l++;
            }
            maxLen = max(maxLen , r-l+1);
        }
        return maxLen;
    }
};
```

<hr>

## 159
Can use 340's solution

<hr>

## 395 Longest Substring with At Least K Repeating Characters	
[blog](https://blog.csdn.net/u010900754/article/details/62159601)

#### 目前处理子串的方法有：
1. Sliding Windows
2. dp；
3. Divide and conqure
#### Solution 
class Solution {
public:
    int longestSubstring(string s, int k) {
        return helper(s,k,0,s.size()-1);
    }
    int helper(string& s,int k,int left,int right){
        if(left>right) return 0;
        vector<int> cnt(26,0);
        for(int i=left;i<=right;i++) cnt[s[i]-'a']++;
        for(int i=0;i<26;i++){
            if(cnt[i]>0 and cnt[i] < k){
                int pos = -1;
                for(int j=left;j<=right;j++){
                    if(s[j] == i+'a'){
                        pos = j;
                        break;
                    }
                }
                return max(helper(s,k,left,pos-1), helper(s,k,pos+1,right));
            }
        }
        return right-left+1;
    }
};

<hr>