---
layout: tec
title: LeetCode-medium
permalink: medium.html
categories: ALG
---
### Decode String

**[Problem](https://leetcode.com/contest/3/problems/decode-string/)**: Given an encoded string, return it's decoded string.

The encoding rule is: `k[encoded_string]`, where the *encoded_string* inside the square brackets is being repeated exactly *k* times. Note that *k* is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

**Solution**: 

```cpp
class Solution {
public:
    string decodeString(string s) {
        int pos = 0;
        return getString(pos,s);
    }
private:
    string getString(int& pos,string& s){
        int num = 0;
        string res = "";
        //use pos to mark the current char. which is in progress. The whole procedure is continuous via calling recursion in a for-loop in which the identifier is pos
        for(;pos<s.size();++pos){
            char cur = s[pos];
            if(cur<='9'&&cur>='0'){
                num = num*10 + cur - '0';
            }
            else if(cur == '['){
                string curStr = getString(++pos,s);
                for(;num>0;num--) res = res+curStr;
            }
            else if(cur == ']'){
                return res;
            }
            else{
              //the normal condition
                res = res + cur;
            }
        }
        //when pos comes to s.size() - 1, stop
        return res;
    }
};
```

