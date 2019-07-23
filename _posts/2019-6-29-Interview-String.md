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

## 28. Implement strStr()
#### Brute Force
```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size();
        int m = needle.size();
        if(m == 0) return 0;
        if(n == 0) return -1;
        int j = 0;
        for(int i=0;i<n;){
            if(haystack[i] == needle[j]){
                j++;
                i++;
                if(j == needle.size()){
                    return i-j; 
                }
            }
            else{
                i = i-j+1;
                j = 0;
            }
        }
        return -1;
    }
};
```

#### KMP
```
class Solution {
public:
    using vi = vector<int>;
    vi findNext(string &p){
        vi next(p.size()+1,0);
        next[0] = -1;
        int k=-1,j=0;
        while(j<p.size()){
            if(k == -1 or p[k] == p[j]){
                k++;
                j++;
                next[j] = k;
            }
            else{
                k=next[k];
            }
        }
        return next;
    }
    int strStr(string haystack, string needle) {
        int m = haystack.size();
        int n = needle.size();
        if(n == 0) return 0;
        if(m == 0) return -1;
        vector<int> next = findNext(needle);
        int i=0,j=0;
        while(i<m){
            if(j== -1 or haystack[i] == needle[j]){
                i++;
                j++;
                if(j == n) return i-j;
            }
            else{
                j = next[j];
            }
        }
        return -1;
    }
};
```

<hr>

## 14. Longest Common Prefix
```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string result;
        int length = 1;
        while(true and strs.size()){
            char cur;
            bool first = true;
            for(int i=0;i<strs.size();i++){
                if(strs[i].size() < length){
                    return result;
                } 
                if(!first){
                    if(strs[i][length-1] != cur){
                        return result;
                    }
                } 
                else{
                    first = false;
                    cur = strs[i][length-1];
                }
            }
            length++;
            result.push_back(cur);
        }
        return result;
    }
};
```

<hr>

## 387. First Unique Character in a String
```
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char,int> uMap;
        for(auto c:s){
            auto it = uMap.find(c);
            if(it == uMap.end()){
                uMap[c] = 1;
            }
            else{
                uMap[c]++;
            }
        }
        for(int i=0;i<s.size();i++){
            if(uMap[s[i]] == 1){
                return i;
            }
        }
        // when s= 0, return -1
        return -1;
    }
};
```

<hr>

## 151. Reverse Words in a String
```
class Solution {
public:
    // reverse the whole string and reverse a word
    string reverseWords(string s) {
        reverse(s.begin(),s.end());
        istringstream iss(s);
        string result;
        for(string item; getline(iss,item,' ');){
            if(item.size() == 0) continue;
            else{
                reverse(item.begin(),item.end());
                result += item+" ";
            }
        }
        result.pop_back();
        return result;
    }
};
```

<hr>

## 186 Reverse Words in a String II   (Lint 927)
```
void reverseString(string&str,int l,int r){
    while(l<r){
        swap(str[l],str[r]);
        r--;
        l++;
    }
}
string reverseWords(string &str) {
    reverseString(str,0,str.size()-1);
    int startPos = -1;
    for(int i=0;i<str.size();i++){
        if(str[i] == ' '){
            reverseString(str,startPos,i-1);
            startPos = -1;
        }
        else{
            if(startPos == -1) startPos = i;
        }
    }
    if(startPos != -1){
        reverseString(str,startPos,str.size()-1);
    }
    return str;        
    // write your code here
}
```

<hr>

## 345. Reverse Vowels of a String
```
class Solution {
public:
    string reverseVowels(string s) {
        vector<int> pos;
        char vowels[] = {'a','e','i','o','u','A','E','I','O','U'};
        for(int i=0 ; i<s.size() ; i++){
            for(int j=0 ; j<10; j++){
                if(vowels[j] == s[i]){
                    pos.push_back(i);
                    continue;
                }
            }
        }
        int l=0,r=pos.size()-1;
        while(l<r){
            swap(s[pos[l]],s[pos[r]]);
            l++;
            r--;
        }
        return s;
    }
};
```

<hr>

## 205. Isomorphic Strings
```
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        int n = s.size();
        unordered_map<char,char> f1;
        unordered_map<char,char> f2;
        for(int i=0;i<n;i++){
            char s_cur = s[i],t_cur = t[i];
            auto it1 = f1.find(s_cur);
            auto it2 = f2.find(t_cur);
            if(it1!=f1.end() and it2!=f2.end()){
                if(it1->second == t_cur and it2->second == s_cur){
                    continue;
                }
                return false;
            }
            if(it1!=f1.end() or it2!=f2.end()) return false;
            f1[s_cur] = t_cur;
            f2[t_cur] = s_cur;
        }
        return true;
    }
};
```

<hr>

## 294 Flip Game II(Lintcode 913)
#### Simple DFS

Attention Here `string s` can't be `string&s`, because when sub-string returns it didn't restore the state of s.
```
bool canWin(string s) {
        for(int i=1;i<s.size();i++){
            if(s[i] == '+' and s[i-1] == '+'){
                s[i] = s[i-1] = '-';
                if(!canWin(s)){
                    return true;
                }
                s[i] = s[i-1] = '+';
            }
        }
        return false;
    }
```

<hr>

## 49. Group Anagrams
```
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> uMap;
        vector<vector<string>> result;
        string sorted;
        for(auto s: strs){
            sorted = s;
            sort(sorted.begin(),sorted.end());
            uMap[sorted].push_back(s);
        }
        for(auto e:uMap){
            result.push_back(e.second);
        }
        return result;
    }
};
```

<hr>

## 249 Group Shifted String(Lintcode 922)
#### Brute Force
```
class Solution {
public:
    /**
     * @param strings: a string array
     * @return: return a list of string array
     */
    using vs = vector<string>;
    using vvs = vector<vector<string>>;
    bool canShift(string a,string b){
        int n = a.size();
        if(a.size() != b.size()){
            return false;
        }
        int diff = b[0]-a[0];
        for(int i=0;i<n;i++){
            int target = a[i]+diff;
            if(target < 'a'){
                target+=26;
            }
            if(target > 'z'){
                target -= 26;  
            } 
            if(target != b[i]) return false;   
        }
        return true;
        
    }
    vector<vector<string>> groupStrings(vector<string> &strings) {
        vvs result;
        for(auto str:strings){
            bool exist = false;
            for(int i=0;i<result.size();i++){
                if(canShift(result[i][0],str)){
                    result[i].push_back(str);
                    exist = true;
                    break;
                }
            }
            if(!exist){
                vs tmp;
                tmp.push_back(str);
                result.push_back(tmp);
            }
        }
        return result;
    }
};
```