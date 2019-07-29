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

## 87. Scramble String *
#### Recursive Solution
Easiest solution

After scramble, it just reverse a substring, so we can take out all substring and check if the result can be generated by simply reverse a substring. So we check if it is the same order or reversed order.
```
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if (s1.size() != s2.size()) return false;
        if (s1 == s2) return true;
        string str1 = s1, str2 = s2;
        sort(str1.begin(), str1.end());
        sort(str2.begin(), str2.end());
        if (str1 != str2) return false;
        for (int i = 1; i < s1.size(); ++i) {
            string s11 = s1.substr(0, i);
            string s12 = s1.substr(i);
            string s21 = s2.substr(0, i);
            string s22 = s2.substr(i);
            if (isScramble(s11, s21) && isScramble(s12, s22)) return true;
            s21 = s2.substr(s1.size() - i);
            s22 = s2.substr(0, s1.size() - i);
            if (isScramble(s11, s21) && isScramble(s12, s22)) return true;
        }
        return false;
    }
};
```

<hr>

## 179. Largest Number
#### Sort
```
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        string res;
        sort(nums.begin(), nums.end(), [](int a, int b) {
           return to_string(a) + to_string(b) > to_string(b) + to_string(a); 
        });
        for (int i = 0; i < nums.size(); ++i) {
            res += to_string(nums[i]);
        }
        return res[0] == '0' ? "0" : res;
    }
};
```
#### Get Comparator outside
```
class Solution {
public:
    static bool cmp(int a,int b){
        return to_string(a)+to_string(b) > to_string(b)+to_string(a);
    }
    string largestNumber(vector<int>& nums) {
        string res;
        sort(nums.begin(), nums.end(), cmp);
        for (int i = 0; i < nums.size(); ++i) {
            res += to_string(nums[i]);
        }
        return res[0] == '0' ? "0" : res;
    }
};
```

<hr>

## 6. ZigZag Conversion
#### Simulation
```
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows <= 1) return s;
       vector<vector<char>> tmp(numRows,vector<char>{});
        int zig = 0;
        bool increment = true;
        int Index = 0;
        for(int i=0;i<s.size();i++){
            tmp[zig].push_back(s[i]);
            if(zig == 0) increment = true;
            if(zig == numRows-1) increment = false;
            if(increment) zig++;
            else zig--;
        }
        string result;
        for(auto i=0;i<tmp.size();i++){
            for(auto j=0;j<tmp[i].size();j++){
                result.push_back(tmp[i][j]);
            }
        }
        return result;
    }
};
```

<hr>

## 161 One Edit Distance  (Lintcode 640)
```
class Solution {
public:
    bool isOneEditDistance(string &s, string &t) {
        if(s == t) return false;
        int n = s.size();
        int m = t.size();
        if(abs(n-m) > 1) return false;
        if(m == n){
            bool diff = false;
            for(int i=0;i<n;i++){
                if(s[i]!=t[i]){
                    if(!diff) diff = true;
                    //more than 1 diff
                    else return false;
                }
            }
        }
        else{
            // t is larger
            if(m > n){
                swap(m,n);
                swap(s,t);
            }
            bool del = false;
            for(int si=0,ti=0;si<n;){
                if(s[si] != t[ti]){
                    if(!del){
                        // del a char in s
                        del = true;
                        si++;
                    }
                    else{
                        return false;
                    }
                }
                else{
                    si++;
                    ti++;
                }
            }
            return true;
        }
    }
};
```

<hr>

## 38.Count and Say
[Blog](https://blog.csdn.net/xygy8860/article/details/46821417)

<hr>

## 358. Rearrange String k Distance Apart	
For each character, we can count how frequent it has been used.

The principle is that: Each time, we fill the string with the character that appeared most frequently. Then keep on fill the string until we have k positions filled or reach the end of the string. 

To get the maximum number of appearence, we can use a priority queue to do that.


```
class Solution {
public:
    string rearrangeString(string str, int k) {
        string result;
        int len = result.size();
        unordered_map<char,int> uMap;
        priority_queue<int,char> pq;
        for(auto s : str){
            uMap[s]++;
        }
        for(auto it=uMap.begin(); it!= uMap.end();it++){
            pq.push({it->second,it->first});
        }
        while(!pq.empty()){
            int cnt = min(k,len);
            vector<pair<int,char>> usedPair;
            for(int i=0;i<cnt;i++){
                if(pq.empty()) return "";
                auto t = pq.top(); pq.pop();
                result.push_back(t->second);
                if(t->first-->0) usedPair.push_back(t);
                len--;
            }
            for(auto a:usedPair) pq.push_back(a);
        }
        return result;
    }
};
```

<hr>

## 316. Remove Duplicate Letters
Push to result first, if find a possible smaller one, pop the result out and replenish the result.
```
class Solution {
public:
    string removeDuplicateLetters(string s) {
        unordered_map<char,int> uMap;
        for(auto c:s){
            uMap[c] ++;
        }
        string result;
        vector<bool> visited(26,false);
        for(int i=0;i<s.size();i++){
            char cur = s[i];
            uMap[cur]--;
            if(visited[cur-'a']) continue;
            // it is smaller than the rear and rear is going to appear again
            while(result.size() > 0 and cur < result.back() and uMap[result.back()]>0){
                visited[result.back()-'a'] = false;
                result.pop_back();
            }
            result.push_back(cur);
            visited[cur-'a'] = true;
        }
        return result;   
    }
};
```

<hr>

## 271.Encode and Decode Strings(Lintcode 659)
The trick is to use an escape character and a deliminator. If we don't use an escape but only use and deliminiator. For example, we parse ':' to "::" and use ':' to indicate the seperator of different stirng, then if the test is `["::::", "::::::::;;::;;;:;", ":", "yes"]`, it can't parse.

So the delim is only valid when it follows an escape character.
#### Solution
```
class Solution {
public:
    /*
     * @param strs: a list of strings
     * @return: encodes a list of strings to a single string.
     */
    string encode(vector<string> &strs) {
        // we need one character as the escape characeter and one deliminator
        // if we use only delim, then if the whole string is only consisted of the delim, then we can't parse it.
        char delim = ';';
        char escape = ':';
        string result;
        for(auto str:strs){
            for(auto c: str){
                result.push_back(c);
                if(c == escape){
                    result.push_back(escape);
                }
            }
            result.push_back(escape);
            result.push_back(delim);
        }
        return result;
    }

    /*
     * @param str: A string
     * @return: dcodes a single string to a list of strings
     */
    vector<string> decode(string &str) {
        char delim = ';';
        char escape = ':';
        // cout<<str<<endl;
        vector<string> result;
        string cur;
        for(int i=0;i<str.size();i++){
            if(str[i] == escape){
                i++;
                if(str[i] == escape){
                }
                else{
                    result.push_back(cur);
                    cur.clear();
                    continue;
                }
            }
            cur.push_back(str[i]);
        }
        return result;
    }
};
```

<hr>

## 168. Excel Sheet Column Title
```
class Solution {
public:
    string convertToTitle(int n) {
        string result;
        while(n){
            int a = n%26;
            if(a == 0){
                result.push_back('Z');
                n = n/26 - 1;

            }
            else{
                result.push_back(a+'A'-1);
                n /= 26;
            }
        }
        reverse(result.begin(),result.end());
        return result;
    }
};
```

<hr>

## 273. Integer to English Words
#### Brute force
```
class Solution {
public:
    string CalHundred(int& num,int base,vector<string>&str,vector<int>&val){
        int cur = num % base;
        num /= base;
        string result;
        for(int i=val.size()-1;i>=0;i--){
            if(cur >= val[i]){
                int a = cur/val[i];
                cur = cur%val[i];
                if(val[i] == 100){
                    result += str[a-1]+" Hundred";
                }
                else{
                    if(result.size()) result += " ";
                    result += str[i];
                }
            }
        }
        return result;
    }
    string numberToWords(int num) {
        if(num == 0) return "Zero";
        vector<string> str{"One","Two","Three","Four","Five","Six","Seven","Eight","Nine","Ten","Eleven","Twelve","Thirteen","Fourteen","Fifteen","Sixteen","Seventeen","Eighteen","Nineteen","Twenty","Thirty","Forty","Fifty","Sixty","Seventy","Eighty","Ninety","Hundred"};
        vector<int> val{1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,30,40,50,60,70,80,90,100};
        string result;
        int base = 1000;
        if(num){
            result = CalHundred(num,base,str,val) + result;
        }
        //thousand
        if(num){            
            string tmp = CalHundred(num,base,str,val);
            if(tmp.size()>0){
                string middle = " Thousand";
                if(result.size()) middle += " ";
                result = tmp+ middle + result;
            }
            else{;}
        }
        //million
        if(num){
            string tmp = CalHundred(num,base,str,val);
            if(tmp.size()>0){
                string middle = " Million";
                if(result.size()) middle += " ";
                result = tmp+ middle + result;
            }
            else{;}
        }
        //billion
        if(num){            
            string tmp = CalHundred(num,base,str,val);
            if(tmp.size()>0){
                string middle = " Billion";
                if(result.size()) middle += " ";
                result = tmp+ middle + result;
            }
            else{;}
        }
        return result;
    }
};
```

<hr>

## 246. Strobogrammatic Number(Lint 644)
Rotate 180 means we need to read it in reverse order. So we need to both check for each pos and check if the reverse number still get same value

For this question, pay attention to border condition, like only one number
#### Solution
```
class Solution {
public:
    /**
     * @param num: a string
     * @return: true if a number is strobogrammatic or false
     */
    void initMap(unordered_map<char,char>&uMap){
        uMap['0'] = '0';
        uMap['1'] = '1';
        uMap['6'] = '9';
        uMap['9'] = '6';
        uMap['8'] = '8';

    }
    bool isStrobogrammatic(string &num) {
        unordered_map<char,char> uMap;
        initMap(uMap);
        for(int l=0,r=num.size()-1;l<=r;l++,r--){
            if(uMap.find(num[l]) == uMap.end()
            or uMap.find(num[r]) == uMap.end()
            or uMap[num[l]] != num[r]
            ){
                cout<<l<<" "<<r<<endl;
                return false;
            }
        }
        return true;
    }
};
```

<hr>

## 247. Strobogrammatic NumberII(Lint 776)

#### DFS
```
vector<string> helper(int n,int m){
    if(n == 0) return vector<string>{""};
    if(n == 1) return vector<string>{"0","1","8"};
    vector<string> last = helper(n-2,m);
    vector<string> result;
    for(int i=0;i<last.size();i++){
        string s = last[i];
        if(n != m) result.push_back("0"+s+"0");
        result.push_back("8"+s+"8");
        result.push_back("1"+s+"1");
        result.push_back("6"+s+"9");
        result.push_back("9"+s+"6");
    }
    return result;
}
vector<string> findStrobogrammatic(int n) {
    return helper(n,n);
}
```

<hr>

