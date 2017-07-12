### [Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/)

Difficulty **Medium**

tags `string` `encode`

Design an algorithm to encode **a list of strings** to **a string**. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:

```
string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
```

Machine 2 (receiver) has the function:

```
vector<string> decode(string s) {
  //... your code
  return strs;
}
```

So Machine 1 does:

```
string encoded_string = encode(strs);
```

and Machine 2 does:

```
vector<string> strs2 = decode(encoded_string);
```

`strs2` in Machine 2 should be the same as `strs` in Machine 1.


Implement the `encode` and `decode` methods.

Note:

- The string may contain any possible characters out of 256 valid ascii characters. Your algorithm should be generalized enough to work on any possible characters.

- Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.

- Do not rely on any library method such as eval or serialize methods. You should implement your own encode/decode algorithm.


这只是一个分行问题，使用特殊字符来分行会有潜在的bug， 且复杂。 所以这里提取出每行的长度， 进行注入，不再考虑行中的内容。


**solution 1**

```c++
class Codec {
public:

    // Encodes a list of strings to a single string.
    string encode(vector<string>& strs) {
        string res;
        for (int i = 0; i < strs.size(); i++) 
            res = res + to_string(strs[i].size()) + "-"+ strs[i];
        return res;
    }

    // Decodes a single string to a list of strings.
    vector<string> decode(string s) {
        vector<string> res;
        int i = 0;
        while (i < s.size()) {
            int p = i;
            while (i < s.size() && s[i++]!='-');
            int n = stoi(s.substr(p, i-p-1));
            string ns = s.substr(i, n);
            res.push_back(ns);
            i = i + n;
        }
        return res;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.decode(codec.encode(strs));
```
