---
layout: post
title:  "CPP Strings"
categories: CPP
---
## Init
Unlike built-in data types (int, double, etc.), when we declare a string variable without initialization (as in the example above),
we do have the guarantee that the variable will be initialized to an empty string — a string containing zero characters.

C++ strings allow you to directly initialize, assign, compare, and reassign with the intuitive operators, as well as printing and reading (e.g., from the user), as shown in the example below:

## Mutable
anything in double quotes is a string literal. This string is stored in a read-only part of memory. 
C++ string literals, i.e., something like "literal" are immutable although C++03 allowed assigning a pointer to such a literal to a char* (this privilege was deprecated and removed for C++11). Trying to change a character of a string literal is undefined behavior:
```
char* s = "literal"; // OK with C++03; illegal with C++11 and later
s[0] = 'x';          // undefined behavior
```
String is mutable unlike Java.

## Methods
* find(“string”): Searches the string for the first occurrence of the substring specified in arguments. It returns the position of the first occurrence of substring.
* find_first_of(“string”): Searches the string for the first character that matches any of the characters specified in its arguments. It returns the position of the first character that matches.
* find_last_of(“string”): Searches the string for the last character that matches any of the characters specified in its arguments. It returns the position of the last character that matches.
* rfind(“string”): Searches the string for the last occurrence of the substring specified in arguments. It returns the position of the last occurrence of substring
* insert(pos_to_begin,string_to_insert): This function inserts the given substring in the string. It takes two arguments, first the position from which you want to insert the substring and second the substring.
* clear(): This function clears all the characters from the string. The string becomes empty (length becomes 0) after this operation.
* empty(): Tests whether the string is empty. This function return a Boolean value.

## StringStream

Count Workds
```
int countWords(string str) 
{ 
    // breaking input into word using string stream 
    stringstream s(str); // Used for breaking words 
    string word; // to store individual words 
  
    int count = 0; 
    while (s >> word) 
        count++; 
    return count; 
} 
```

Count Frequency
```
void printFrequency(string st) 
{ 
    // each word it mapped to it's frequency 
    map<string, int> FW; 
    stringstream ss(st); // Used for breaking words 
    string Word; // To store individual words 
  
    while (ss >> Word) 
        FW[Word]++; 
  
    map<string, int>::iterator m; 
    for (m = FW.begin(); m != FW.end(); m++) 
        cout << m->first << " -> "
             << m->second << "\n"; 
} 
```

```
int main () {
  std::stringstream ss;
  ss << 100 << ' ' << 200;
  int foo,bar;
  ss >> foo >> bar;

  std::cout << "foo: " << foo << '\n';
  std::cout << "bar: " << bar << '\n';

  return 0;
}
```
