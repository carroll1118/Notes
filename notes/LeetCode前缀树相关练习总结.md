> 一直在补充中....

#  [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

```java
class TrieNode {
    public char val;
    public boolean isWord;
    TrieNode[] child = new TrieNode[26];
    public TrieNode() {}
    TrieNode(char c) {
        TrieNode node = new TrieNode();
        node.val = c;
    }
}

class Trie {
    /** Initialize your data structure here. */
    private TrieNode root;
    public Trie(){
        root = new TrieNode();
        root.val = ' ';
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode ws = root;
        for(int i = 0;i<word.length();i++) {
            char c = word.charAt(i);
            if(ws.child[c-'a']==null){
                ws.child[c-'a'] = new TrieNode(c);
            }
            ws = ws.child[c-'a'];
        }
        ws.isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode ws = root;
        for(int i = 0;i<word.length();i++) {
            char c = word.charAt(i);
            if(ws.child[c-'a']==null){
                return false;
            }
            ws = ws.child[c-'a'];
        }
        return ws.isWord;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode ws = root;
        for(int i = 0;i<prefix.length();i++) {
            char c = prefix.charAt(i);
            if(ws.child[c-'a']==null){
                return false;
            }
            ws = ws.child[c-'a'];
        }
        return true;
    }
}
```
#  [211. 添加与搜索单词 - 数据结构设计](https://leetcode-cn.com/problems/add-and-search-word-data-structure-design/)

```java
class WordDictionary {

    class TrieNode {
        char val;
        boolean isWord;
        TrieNode[] child = new TrieNode[26];
        public TrieNode(){}
        TrieNode(char c){
            TrieNode node = new TrieNode();
            node.val = c;
        }
    }

    private TrieNode root;
    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }
    
    /** Adds a word into the data structure. */
    public void addWord(String word) {
        TrieNode ws = root;
        for(int i = 0;i<word.length();i++){
            char c = word.charAt(i);
            if(ws.child[c-'a']==null){
                ws.child[c-'a'] = new TrieNode(c);
            }
            ws = ws.child[c-'a'];
        }
        ws.isWord = true;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        return searchHelp(word,root);
    }

    public boolean searchHelp(String word,TrieNode root){
        TrieNode ws = root;
        for(int i = 0;i<word.length();i++){
            char c = word.charAt(i);
            if(c=='.') {
               for(int j = 0;j < 26; j++){
                    if(ws.child[j] != null){
                        if(searchHelp(word.substring(i + 1),ws.child[j])){
                            return true;
                        }
                    }
                }
                return false;
            }
            if(ws.child[c-'a']==null){
                return false;
            }
            ws = ws.child[c-'a'];
        }
        return ws.isWord;
    }
}
```
#  [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)

```java
在这里插入代码片
```
#  [677. 键值映射](https://leetcode-cn.com/problems/map-sum-pairs/)

```java
class MapSum {

    class TrieNode{
        int val;
        TrieNode[] nodes = new TrieNode[26];
        public TrieNode(){}
    }

    private TrieNode root;
    private HashMap<String,Integer> map;
    /** Initialize your data structure here. */
    public MapSum() {
        root = new TrieNode();
        map = new HashMap<>();
    }
    
    public void insert(String key, int val) {
        TrieNode ws = root;
        boolean flag = map.containsKey(key);
        for(int i = 0;i < key.length();i++){
            int k = key.charAt(i) - 'a';
            if(ws.nodes[k] == null){
                ws.nodes[k] = new TrieNode();
            } 
            if(flag){
                //如果key存在map中,
                ws.nodes[k].val -= map.get(key);
            }      
            ws.nodes[k].val += val;
            ws = ws.nodes[k]; 
        }
        map.put(key,val);
    }
    
    public int sum(String prefix) {
        TrieNode ws = root;
        for(int i = 0;i < prefix.length();i++){
            int k = prefix.charAt(i) - 'a';
            if(ws.nodes[k] == null){
                return 0;
            }
            ws = ws.nodes[k];
        }
        return ws.val;
    }
}
```
#  [820. 单词的压缩编码](https://leetcode-cn.com/problems/short-encoding-of-words/)

```java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        int len = 0;
        Trie trie = new Trie();
        Arrays.sort(words,(s1,s2)->s2.length()-s1.length());
        for(String word:words){
            len += trie.insert(word);
        }
        return len;
    }
}

class Trie {
    TrieNode root;
    public Trie() {
        root = new TrieNode();
    }

    public int insert(String word){
        TrieNode cur = root;
        boolean isNew = false;

        //倒着插入单词
        for(int i = word.length()-1;i>=0;i--){
            int c = word.charAt(i) - 'a';
            if(cur.children[c]==null){
                isNew = true;
                cur.children[c] = new TrieNode();
            }
            cur = cur.children[c];
        }
        return isNew?word.length()+1:0;
    }
}

class TrieNode {
    char val;
    TrieNode[] children = new TrieNode[26];
    public TrieNode() {}
}
```

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
