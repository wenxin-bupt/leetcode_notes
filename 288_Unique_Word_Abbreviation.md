### [Unique Word Abbreviation](https://leetcode.com/problems/unique-word-abbreviation/)

Difficulty **Medium**

tags `string`

An abbreviation of a word follows the form <first letter><number><last letter>. Below are some examples of word abbreviations:
```
a) it                      --> it    (no abbreviation)

     1
b) d|o|g                   --> d1g

              1    1  1
     1---5----0----5--8
c) i|nternationalizatio|n  --> i18n

              1
     1---5----0
d) l|ocalizatio|n          --> l10n
```
Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.

Example: 
```
Given dictionary = [ "deer", "door", "cake", "card" ]

isUnique("dear") -> 
false

isUnique("cart") -> 
true

isUnique("cane") -> 
false

isUnique("make") -> 
true
```
这道题的题目描述比较模糊，需要多次尝试test case 来找出规律. 

失误的cases:

1. dictionary 中的单词没有说是no duplicates， 一定要考虑有重复的情况。 否则，这里用计数法来统计单词是否有重复就会出错

2. 虽然在题目描述中没有给，但是在test case中给了，如果输入了和dict中相同的word， 而这个word在dict中缩写唯一， 就返回true


**solution 1**
```c++

class ValidWordAbbr {
public:
    ValidWordAbbr(vector<string> dictionary) {
        for (string word : dictionary){
            string nw = word_abbr(word);
            if (m.find(nw) == m.end()) 
                m.insert(pair<string, string>(nw, word));
            else if (m[nw] != word)
                m[nw] = "";
            
        }
    }
    
    bool isUnique(string word) {
        string nw = word_abbr(word);
        if (m.find(nw) != m.end() && m[nw] != word)
            return false;
        return true;
    }
private:
    string word_abbr(string w){
        int l = w.size();
        string nw;
        if (l>2) 
            nw = w[0] + to_string(l-2) +  w[l-1];
        else
            nw = w;

        return nw;
    }
    
    map<string, string> m;
};

/**
 * Your ValidWordAbbr object will be instantiated and called as such:
 * ValidWordAbbr obj = new ValidWordAbbr(dictionary);
 * bool param_1 = obj.isUnique(word);
 */

```