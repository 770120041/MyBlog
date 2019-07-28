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


<hr>

## 158 Read N Characters Given Read4 II - Call multiple times	(Lintcode 660)

<hr>