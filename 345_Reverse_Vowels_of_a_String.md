### [Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)

Difficulty **Easy**

tags `string` 

Write a function that takes a string as input and reverse only the vowels of a string.

**Example 1**:

Given s = "hello", return "holle".

**Example 2**:

Given s = "leetcode", return "leotcede".

**Note**:

The vowels does not include the letter "y".


**solution 1**

```c++
class Solution {
public:
    string reverseVowels(string s) {
        int l = 0, r = s.size()-1;
        while (l < r) {
            while (l<r && !isVowel(tolower(s[l]))) l++;
            while (l<r && !isVowel(tolower(s[r]))) r--;
            swap(s[l], s[r]);
            l++;
            r--;
        }
        return s;
    }
private:
    bool isVowel(char c){
        if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u')
            return true;
        return false;
    }
};
```