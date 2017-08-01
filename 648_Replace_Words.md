### [Replace Words](https://leetcode.com/problems/replace-words/)

Difficulty **Medium**

tags `trie` `string`

In English, we have a concept called `root`, which can be followed by some other words to form another longer word - let's call this word `successor`. For example, the root `an`, followed by `other`, which can form another word `another`.

Now, given a dictionary consisting of many roots and a sentence. You need to replace all the `successor` in the sentence with the `root` forming it. If a `successor` has many `roots` can form it, replace it with the root with the shortest length.

You need to output the sentence after the replacement.

**Example 1:**
```
Input: dict = ["cat", "bat", "rat"]
sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
```

**Note:**

1. The input will only have lower-case letters.
2. 1 <= dict words number <= 1000
3. 1 <= sentence words number <= 1000
4. 1 <= root length <= 100
5. 1 <= sentence words length <= 1000

思路：

用字典树即可， 稍加改动。 用了自用的数据结构[字典树](http://www.cnblogs.com/whensean/p/trie.html)

要注意和失误的case：

1. 最早用了char数组的，结果char数组明显没有用string来处理简洁。
2. 对原有数据结构改动的时候出了问题。 想统计某个结点在树上是第几层，从而获得子字符串的长度。 所以增加了一个新属性k, k的初始值设定有问题， 后来改正为从父结点的k值加1。
3. 这个算法是可以删除结点的，所以有parent属性用于回溯。

**solution 1**
```c++
#define ALPHABETS 26
#define CASE 'a'
#define MAX_WORD_SIZE 25

using namespace std;

struct node
{
    struct node * parent;
    struct node * children[ALPHABETS];
    int k; // 
    vector<int> occurrences;
};


void insertWord(struct node * trieTree, string word, int index = 0) {
    struct node * traverse = trieTree;
    for (char c : word) {     
        if (traverse->children[c - CASE] == NULL) {
            traverse->children[c - CASE] = (struct node *) calloc(1, sizeof(struct node));
            traverse->children[c - CASE]->parent = traverse;  
            traverse->children[c - CASE]->occurrences = {};
            traverse->children[c - CASE]->k =  traverse->k+1; // 
        }
        traverse = traverse->children[c - CASE];
    }
    traverse->occurrences.push_back(index);      
}

struct node * searchWord(struct node * treeNode, string word) {
    for (char c : word) {
        if (treeNode->children[c - CASE] != NULL) {
            treeNode = treeNode->children[c - CASE];
            if (treeNode->occurrences.size() > 0) {
                
                return treeNode;
            }
        } else 
            return NULL;
    }
    if (treeNode->occurrences.size() != 0) 
        return treeNode;
    else 
        return NULL;
}

void removeWord(struct node * trieTree, string word) {
    struct node * trieNode = searchWord(trieTree, word);
    if (trieNode == NULL) return;
    trieNode->occurrences.pop_back();  
    bool noChild = true;
    int childCount = 0;
    int i;
    for (i = 0; i < ALPHABETS; ++i) {
        if (trieNode->children[i] != NULL) {
            noChild = false;
            ++childCount;
        }
    }
    if (!noChild) return;
    struct node * parentNode; 
    while (trieNode->occurrences.size() == 0 && trieNode->parent != NULL && childCount == 0) {
        childCount = 0;
        parentNode = trieNode->parent;
        for (i = 0; i < ALPHABETS; ++i) {
            if (parentNode->children[i] != NULL) {
                if (trieNode == parentNode->children[i]) {
                    parentNode->children[i] = NULL;
                    free(trieNode);
                    trieNode = parentNode;
                } else {
                    ++childCount;
                }
            }
        }
    }
}
    

class Solution {
public:
    string replaceWords(vector<string>& dict, string sentence) {
        struct node * trieTree = (struct node *) calloc(1, sizeof(struct node));
        trieTree->k = 0;
        for (string word : dict) {
            insertWord(trieTree, word);
        }
        string res;
        int i, k;
        for (i=0; i<sentence.size(); ) {
            int k = i;            
            while (k<sentence.size() && sentence[k]!=' ') k++;
            string word = sentence.substr(i, k-i);
            struct node * node = searchWord(trieTree, word);
            if (node != NULL) word = word.substr(0, node->k);
            res += word;
            while (k<sentence.size() && sentence[k]==' ') {
                res += sentence[k];
                k++;
            }
            i = k;
        }
        return res;
    }
};
```
