---
layout: tec
title: LeetCode-easy
permalink: easy.html
categories: ALG
---

### Remove Duplicates from Sorted Array

**[Problem](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)**: Given a sorted array, remove the duplicates in place such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array *nums* = `[1,1,2]`,

Your function should return length = `2`, with the first two elements of *nums* being `1` and `2`respectively. It doesn't matter what you leave beyond the new length.

**Solution**:

```cpp
class Solution {
public:
    
    int removeDuplicates(vector<int>& nums) {
        int index = 0, count = 0;
        int n = nums.size();
	if (n <= 1) {
		return n;
	}
	for (int i = 1; i < n; ++i) {
		if (nums[index] != nums[i]) {
			nums[++index] = nums[i];
			count = 0;
		}
	}
	return index + 1;
    }
};
```

### Is Subsequence

**Problem** : Given a string **s** and a string **t**, check if **s** is subsequence of **t**.

You may assume that there is only lower case English letters in both **s** and **t**. **t** is potentially a very long (length ~= 500,000) string, and **s** is a short string (<=100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Solution**: 

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if(s.empty()&&t.empty()) return true;
        int index = 0;
        for(int i = 0;i<t.size();++i){
            if(s[index] == t[i]) ++index;
            if(index == s.size()) return true;
        }
        return false;
    }
};
```

### Rotate Function

**[Problem](https://leetcode.com/contest/4/problems/rotate-function/)**: Given an array of integers `A` and let *n* to be its length.

Assume `Bk` to be an array obtained by rotating the array `A` *k* positions clock-wise, we define a "rotation function" `F` on `A` as follow:

`F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]`.

Calculate the maximum value of `F(0), F(1), ..., F(n-1)`.

**TLE Solution**: 

```cpp
class Solution {
public:
    int maxRotateFunction(vector<int>& A) {
        int n = A.size();
        int f = 0,max_ = 0;
        for(int i = 0;i<n;++i){
            f+=i*A[i];
        }       
        max_ = f;
        for(int i = 1;i<n;++i){
            f = 0;
            for(int j = 0;j<n;++j){
                f+=j*A[(j+i)%n];
            }
            max_ = max_>f?max_:f;
        }
        return max_;
    }
};
```

**AC Solution**:

```cpp
class Solution {
public:
    int maxRotateFunction(vector<int>& A) {
        int n = A.size();
        if(n==0) return 0;
      //wsum<=>weighted sum
        int sum = 0,wsum = 0;
        for(int i = 0;i<n;++i){
            sum+=A[i];
            wsum+=i*A[i];
        }
        int res = wsum;
      //the special rotate way
        for(int i = 0;i<n;++i){
            wsum = wsum - sum + n*A[i];
            res = max(res,wsum);
        }
        return res;
    }
};
```



### Integer Replacement

**[Problem](https://leetcode.com/contest/4/problems/integer-replacement/)**: Given a positive integer *n* and you can do operations as follow:

1. If *n* is even, replace *n* with `*n*/2`.
2. If *n* is odd, you can replace *n* with either `*n* + 1` or `*n* - 1`.

What is the minimum number of replacements needed for *n* to become 1?

**Solution**:

```cpp
class Solution {
public:
    int integerReplacement(int n) {
        unordered_map<int,int> table;
        table[1] = 0;
      //the upper bound. INT_MAX = 2^32 - 1.
        table[INT_MAX + 1] = 31;
        return lookuptable(table,n);
    }
private:
    int lookuptable(unordered_map<int,int>& table,int n){
        if(table.find(n)!=table.end()){
            return table[n];
        }
        if(n%2 == 0){
            table[n] = lookuptable(table,n/2) + 1;
            return table[n];
        }
        table[n] = min(lookuptable(table,n-1),lookuptable(table,n+1)) + 1;
        return table[n];
    }
};
```



### Two Sum

**[Problem](https://leetcode.com/problems/two-sum/)**: Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**Solution**:

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> table;
        for(int i = 0;i<nums.size();++i){
            table[nums[i]] = i;
        }
        for(int i = 0;i<nums.size();++i){
            int gap = target - nums[i];
            unordered_map<int,int>::iterator it = table.find(gap);
          //note that i<it->second. when it comes to nums[i], only the numbers after it should be searched.
            if(it!=table.end()&&i<it->second){
                return vector<int>({i,it->second});
            }
        }
        return vector<int>();
    }
};
```

