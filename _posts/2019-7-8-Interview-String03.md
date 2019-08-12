---
layout: post
title:  "Leetcode String 03"
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

## 5. Longest Palindromic Substring
#### Brute Force O(N^2)
```
class Solution {
public:
    string longestPalindrome(string s) {
        string longest;
        for(int i=0;i<s.size();i++){
            int left=i,right=i;
            while(left>=0 and right < s.size() and s[left] == s[right]){
                if(longest.size() < right-left+1){
                    longest = s.substr(left,right-left+1);
                }
                left--;
                right++;
            }
        }
        for(int i=1;i<s.size();i++){
            int left=i-1,right=i;
            while(left>=0 and right < s.size() and s[left] == s[right]){
                if(longest.size() < right-left+1){
                    longest = s.substr(left,right-left+1);
                }
                left--;
                right++;
            }
        }
        return longest;
    }
};
```

#### Manacher's Algorithm 
[blog](https://blog.csdn.net/dyx404514/article/details/42061017)
[visualize](http://manacher-viz.s3-website-us-east-1.amazonaws.com/#/)


Steps:
1. Add # to the front and rear of each chacter, so a character of length n will have 2n+1 characters.
We can add different characters for rear and head. For example, we make "abcde" to "$a#b#c#d#e^"
2. The key of this alrogithm is to use the result we calculated before to accelerate all calculation later. We use one point 'mx' to keep track of the longest position we have already find as a palindrome string. We also use `lastC` to indicate the last Center. 
3. If we have calculated this pos, which means `mx > current_position`,then `P[i] = min(mx-i,P[2*lastC-i]`, the reason is because from S[lastC-P[lastC]] to S[mx], it is palidrome string, so from it can at most be mx-i or [2*lastC-i], and we need to choose the smaller one);

```
string Manacher(string s) {
    // Insert '#'
    string t = "$#";
    for (int i = 0; i < s.size(); ++i) {
        t += s[i];
        t += "#";
    }
    // Process t
    vector<int> p(t.size(), 0);
    int mx = 0, id = 0, resLen = 0, resCenter = 0;
    for (int i = 1; i < t.size(); ++i) {
        p[i] = mx > i ? min(p[2 * id - i], mx - i) : 1;
        while (t[i + p[i]] == t[i - p[i]]) ++p[i];
        if (mx < i + p[i]) {
            mx = i + p[i];
            id = i;
        }
        if (resLen < p[i]) {
            resLen = p[i];
            resCenter = i;
        }
    }
    return s.substr((resCenter - resLen) / 2, resLen - 1);
}
```

<hr>

## 9. Palindrome Number	
```
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0) return false;
        long long reverse=0;
        long long tmp = x;
        while(tmp){
            reverse*=10;
            reverse += tmp%10;
            tmp/=10;
        }
        cout<<reverse<<endl;
        return reverse == x;
    }
};
```

<hr>

## 214. Shortest Palindrome
#### Brute force: TLE
```
class Solution {
public:
    bool isPalindrome(const string&s,int left,int right){
        while(left<right){
            if(s[left++] != s[right--]) return false;
        }
        return true;
    } 
    
    string shortestPalindrome(string s) {
        if(s.size() == 0) return s;
        int resultPos = 0;
        for(int i=s.size()-1;i>=0;i--){
            if(isPalindrome(s,0,i)){
                resultPos=i;
                break;
            }
        }
        if(resultPos == s.size()-1) return s;
        string leftPart = s.substr(resultPos+1);
        reverse(leftPart.begin(),leftPart.end());
        return leftPart+s;
    }
};
```

#### KMP
reverse s stored to r, concatanate r+"$"+s, then use KMP, the reason to use "$" to to seperate thouse two strings, so the next of the latter part will start from 0

The reason to reverse it is that if this is a palindrome string, then r == s.所以从t的最后一个位置的最长前缀就是palindrome的长度，因为前缀==后缀是回文串的条件 and when we find the next of t = r+"$"+s, then `next[t.size()-1]` is the length of the overlapping part.
```
class Solution {
public:
    vector<int> findPrefix(const string&t){
        vector<int> next(t.size(),-1);
        int j = -1,i = 0;
        while(i<t.size()-1){
            if(j == -1 or t[i] == t[j]){
                i++;j++;
                next[i] = j;
            }
            else{
                j = next[j];
            }
        }
        return next;
    }
    string shortestPalindrome(string s) {
        string r = s;
        reverse(r.begin(), r.end());
        string t = s + "#" + r;
        auto next = findPrefix(t);
        cout<<endl;
        return r.substr(0, s.size()-next.back()-1) + s;
    }
};
```

<hr>

## 336. Palindrome Pairs
#### Brute force:TLE
```
class Solution {
public:
    bool isPalindrome(const string &s){
        int l=0,r=s.size()-1;
        while(l<r){
            if(s[l++] != s[r--]) return false;
        }
        return true;
    }
    vector<vector<int>> palindromePairs(vector<string>& words) {
        vector<vector<int>> result;
        unordered_map<string,bool> uMap;
        for(int i=0;i<words.size();i++){
            for(int j=0;j<words.size();j++){
                if(j == i) continue;
                string tmp = words[i]+words[j];
                if(uMap[tmp] or isPalindrome(tmp)){
                    uMap[tmp] = true;
                    result.push_back(vector<int>{i,j});
                }
            }
        }   
        return result;
    }
};
```

#### Using Hashmap
Use a set to prevent duplicates. Otherwise for  ` ["abcd","dcba","lls","s","sssll"]`, the result would be ` [[0,1],[1,0],[0,1],[1,0],[3,2],[2,4]] `
```
class Solution {
public:
    bool isPalindrome(const string&s){
        int l = 0, r = s.size() - 1;
        while(l < r){
            if(s[l++] != s[r--]) return false;
        }
        return true;
    }
    vector<vector<int>> palindromePairs(vector<string>& words) {
        unordered_map<string,int> uMap;
        for(auto i = 0 ; i < words.size() ; i++){
            uMap[words[i]] = i;
        }
        vector<vector<int>> result;
        set<pair<int,int>> tmpSet;
        for(auto i = 0 ; i < words.size() ; i++){
            //j <= to make sure consider both parts of ""
            for(int j=0;j<=words[i].size();j++){
                string left = words[i].substr(0,j);
                string right = words[i].substr(j);
                if(isPalindrome(left)){
                    string Rright = right;
                    reverse(Rright.begin(),Rright.end());
                    auto it = uMap.find(Rright);
                    if(it != uMap.end() and it->second != i) tmpSet.insert({it->second,i});
                }
                if(isPalindrome(right)){
                    string Rleft = left;
                    reverse(Rleft.begin(),Rleft.end());
                    auto it = uMap.find(Rleft);
                    if(it != uMap.end() and it->second != i) tmpSet.insert({i,it->second});
                }
            }
        }
        for(auto x:tmpSet){
            result.push_back(vector<int>{x.first,x.second});
        }
        return result;
    }
};
```

<hr>

## 131. Palindrome Partitioning
#### DFS
```
class Solution {
public:
    using vs = vector<string>;
    using vvs = vector<vs>;
    bool isPalindrome(const string &s){
        int l = 0, r = s.size() - 1;
        while(l < r){
            if(s[l++] != s[r--]) return false;
        }
        return true;
    }
    void DFS(vvs&result, vs&cur,int index,const string&s){
        if(index >= s.size()){
            if(index > s.size());
            result.push_back(cur);
            return;
        }
        for(int i = 1; i <= s.size()-index ; i++){
            if(isPalindrome(s.substr(index,i))){
                cur.push_back(s.substr(index,i));
                DFS(result,cur,index+i,s);
                cur.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        vvs result;
        vs tmp;
        DFS(result,tmp,0,s);
        return result;
    }
};
```

<hr>

## 132. Palindrome Partitioning II
#### Brute force:TLE
Even with prune branches and Hashmap, TLE
```
class Solution {
public:
    bool isPalindrome(const string&s){
        int l = 0 , r = s.size() - 1;
        while(l < r) if(s[l++] != s[r--]) return false;
        return true;
    }
    void DFS(int &result,vector<bool>& tmp, int index,const string& s,unordered_map<string,bool> &uMap){
        if(int(tmp.size()) - 1 >= result){
            return;
        }
        if(index == s.size()){
            if(int(tmp.size())-1 < result){
                result = tmp.size() - 1;
            }
        }
        for(int i=1;i<=s.size() - index ; i ++){
            auto it = uMap.find(s.substr(index,i));
            if(it == uMap.end()){
                uMap[s.substr(index,i)] = isPalindrome(s.substr(index,i));
            }
            if(uMap[s.substr(index,i)]){
                tmp.push_back(true);
                DFS(result,tmp,index+i,s,uMap);
                tmp.pop_back();
            }
        }
    }
    int minCut(string s) {
        unordered_map<string,bool> uMap;
        int result = INT_MAX;
        vector<bool> tmp;
        DFS(result,tmp,0,s,uMap);
        return result;
    } 
};
```
#### DP

```
class Solution {
public:
    int minCut(string s) {
        if (s.empty()) return 0;
        int n = s.size();
        vector<vector<bool>> p(n, vector<bool>(n));
        vector<int> dp(n);
        for (int i = 0; i < n; ++i) {
            dp[i] = i;
            for (int j = 0; j <= i; ++j) {
                if (s[i] == s[j] && (i - j < 2 || p[j + 1][i - 1])) {
                    p[j][i] = true;
                    dp[i] = (j == 0) ? 0 : min(dp[i], dp[j - 1] + 1);
                }
            }
        }
        return dp[n - 1];
    }
};
```

<hr>

## 266 Palindrome Permutation(lint 916)
```
class Solution {
public:
    bool canPermutePalindrome(string &s) {
        unordered_map<char,int> uMap;
        for(auto c:s) uMap[c]++;
        int cnt = 0;
        for(auto it = uMap.begin(); it != uMap.end();it++){
            if(it->second % 2) cnt++;
            if(cnt == 2) return false;
        }
        return true;
    }
};
```
<hr>


## 267 Palindrome Permutation II(lint 917)
Brute Force
```
class Solution {
public:
    /**
     * @param s: the given string
     * @return: all the palindromic permutations (without duplicates) of it
     */
    using umci =  unordered_map<char,int> ;
    void DFS(vector<string>&result,umci &uMap,umci&used,int cnt,int n,string &cur){
        if(cnt == n/2){
            result.push_back(cur);
            return;
        }
        for(auto it = uMap.begin();it!=uMap.end();it++){
            if(it->second == 1) continue;
            if(used[it->first] == it->second/2) continue;
            
            used[it->first]++;
            cur.push_back(it->first);
            DFS(result,uMap,used,cnt+1,n,cur);
            cur.pop_back();
            used[it->first]--;
        }
    }
    vector<string> generatePalindromes(string &s) {
       umci uMap;
        for(auto c:s) uMap[c]++;
        vector<string> result;
        char single;
        int cnt = 0;
        for(auto it = uMap.begin();it != uMap.end();it++){
            if(it->second %2){
                single = it->first;
                cnt++;
            }
            if(cnt == 2) return result;
        }
       umci used;
       string tmp;
       DFS(result,uMap,used,0,s.size(),tmp);
       for(auto i = 0;i<result.size();i++){
            tmp = result[i];
           reverse(tmp.begin(),tmp.end());
           if(cnt) result[i] += single;
           result[i] += tmp;
       }
        return result;
    }
};
```
<hr>

## 20. Valid Parentheses
Using hashmap will be better because its harder to make mistakes

```
class Solution {
public:
    bool isValid(string s) {
        stack<int> myStack;
        unordered_map<char,int> uMapl{{'(',0},{'[',1},{'{',2}};
        unordered_map<char,int> uMap{{')',0},{']',1},{'}',2}};
        for(int i=0;i<s.size();i++){
            if(s[i] == '(' or s[i] == '[' or s[i] == '{') myStack.push(uMapl[s[i]]);
            else{
                if(myStack.empty()) return false;
                int topIndex = myStack.top();
                myStack.pop();
                cout<<uMap[s[i]]<<" "<<topIndex<<endl;
                if(uMap[s[i]] != topIndex ) return false;
            }
        }
        return myStack.empty();
    }
};
```

<hr>

## 22. Generate Parentheses
#### Backtracking
O(n) space and O(2^N) time complexity
```
class Solution {
public:
    using vs = vector<string>;
    void DFS(vs&result,string&cur,int left,int right,int n){
        if(left+right == n*2){
            result.push_back(cur);
            return;
        }
        if(left<n){
            cur.push_back('(');
            DFS(result,cur,left+1,right,n);
            cur.pop_back();
        }
        if(right<left){
            cur.push_back(')');
            DFS(result,cur,left,right+1,n);
            cur.pop_back();
        }
    }
    vector<string> generateParenthesis(int n) {
        vs result;
        string tmp;
        DFS(result,tmp,0,0,n);
        return result;
    }
};
```

<hr>

## 32. Longest Valid Parentheses
Brute force: TLE
#### Stack
```
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> myStack;
        int start = 0;
        int result = 0;
        for(int i=0;i<s.size();i++){
            if(s[i] == '(') myStack.push(i);
            else{
                if(myStack.empty()) start = i+1;
                else{
                    myStack.pop();
                    result = myStack.empty()?max(result,i-start+1):max(result,i-myStack.top());
                }
            }
        }
        return result;
    }
};
```

pay attention to else, if we do
```
else{
    int topI = myStack.top(); myStack.pop();
    result = myStack.empty()?max(result,i-start+1):max(result,i-topI+1);
}
```
If the test cases is `"(()()"`, it will result 2 not 4, because the start is still 0, so we need to compare the last `(`

<hr>

## 241. Different Ways to Add Parentheses
Simple recursive
```
class Solution {
public:
    vector<int> diffWaysToCompute(string input) {
        vector<int> result;
        for(int i=0;i<input.size();i++){
            if(input[i] == '+' or input[i] == '-' or input[i] == '*'){
                auto left = diffWaysToCompute(input.substr(0,i));
                auto right = diffWaysToCompute(input.substr(i+1));
                for(int j=0;j<left.size();j++){
                    for(int k = 0 ; k < right.size() ; k++){
                        if(input[i] == '+') result.push_back(left[j]+right[k]);
                        else if(input[i] == '-') result.push_back(left[j]-right[k]);
                        else  result.push_back(left[j]*right[k]);
                    }
                }
            }
        }
        if(result.size() == 0) result.push_back(atoi(input.c_str()));
        return result;
    }
};
```

<hr>

## 301. Remove Invalid Parentheses
#### BFS 01
because use the minimum changes to achieve valid parenthesis, so we can use BFS.
```
class Solution {
public:
    bool valid(const string&s){
        int cnt = 0;
        for(auto c:s){
            if(c == '(') cnt++;
            else if(c == ')' and --cnt <0) return false;
        }
        return cnt == 0;
    }
    vector<string> removeInvalidParentheses(string s) {
        unordered_set<string> visited{{s}};
        queue<string> q{{s}};
        vector<string> result;
        bool find = false;
        while(!q.empty()){
            auto cur = q.front();q.pop();
            if(valid(cur)){
                result.push_back(cur);
                find = true;
            }
            if(find) continue;
            for(int i=0;i<cur.size();i++){
                if(cur[i]!='(' and cur[i] !=')') continue;
                string tmp = cur.substr(0,i)+cur.substr(i+1);
                if(visited.count(tmp) == 0){
                    visited.insert(tmp);
                    q.push(tmp);
                }
            }
        }
        
        return result;
    }
};
```

The ` if(find) continue;` is the line to make sure they are all in same level.

#### BFS02
using my way to do bfs
```
class Solution {
public:
    bool valid(const string&s){
        int cnt = 0;
        for(auto c:s){
            if(c == '(') cnt++;
            else if(c == ')' and --cnt <0) return false;
        }
        return cnt == 0;
    }
    vector<string> removeInvalidParentheses(string s) {
        unordered_set<string> visited{{s}};
        queue<string> q{{s}};
        vector<string> result;
        while(!q.empty()){
            int s = q.size();
            bool find = false;
            for(int i=0;i<s;i++){
                auto cur = q.front(); q.pop();
                if(valid(cur)){
                    find = true;
                    result.push_back(cur);
                }
                for(int j=0;j<cur.size();j++){
                    if(cur[j] != '(' and cur[j] != ')') continue;
                    string tmp = cur.substr(0,j)+cur.substr(j+1);
                    if(visited.count(tmp) == 0){
                        visited.insert(tmp);
                        q.push(tmp);
                    }
                }
            }
            if(find) break;
        }
        return result;
    }
};
```
#### DFS
The reason why DFS find the minimum is because it try to make `left-right=0` without any redundent steps.
```
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        vector<string> res;
        int cnt1 = 0, cnt2 = 0;
        for (char c : s) {
            cnt1 += (c == '(');
            if (cnt1 == 0) cnt2 += (c == ')');
            else cnt1 -= (c == ')');
        }
        helper(s, 0, cnt1, cnt2, res);
        return res;
    }
    void helper(string s, int start, int cnt1, int cnt2, vector<string>& res) {
        if (cnt1 == 0 && cnt2 == 0) {
            if (isValid(s)) res.push_back(s);
            return;
        }
        for (int i = start; i < s.size(); ++i) {
            if (i != start && s[i] == s[i - 1]) continue;
            if (cnt1 > 0 && s[i] == '(') {
                helper(s.substr(0, i) + s.substr(i + 1), i, cnt1 - 1, cnt2, res);
            }
            if (cnt2 > 0 && s[i] == ')') {
                helper(s.substr(0, i) + s.substr(i + 1), i, cnt1, cnt2 - 1, res);
            }
        }
    }
    bool isValid(string t) {
        int cnt = 0;
        for (int i = 0; i < t.size(); ++i) {
            if (t[i] == '(') ++cnt;
            else if (t[i] == ')' && --cnt < 0) return false;
        }
        return cnt == 0;
    }
};
```


<hr>

## 392. Is Subsequence
#### Brute Force
```
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int i=0,j=0;
        while(i < s.size()){
            while(s[i] != t[j] and j<t.size() ) j++;
            if(j == t.size()) return false;
            i++;
            j++;
        }
        return true;
    }
};
```

<hr>

## 187. Repeated DNA Sequences
#### Hash
```
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        unordered_map<string,int> uMap;
        vector<string> result;
        for(int i=0;i<=int(s.size())-10;i++){
            string cur = s.substr(i,10);
            auto it = uMap.find(cur);
            if(it != uMap.end() and it->second == 1){
                result.push_back(cur);
            }
            uMap[cur]++;
        }
        return result;
    }
};
```

<hr>