@[toc]
#  76. 最小覆盖子串
* [原题地址](https://leetcode-cn.com/problems/minimum-window-substring/)

```java
class Solution {

    public String minWindow(String s, String t) {

        //needs存储t的<字符,出现次数>,windows存储<s中与t中字符相同的字符,出现次数>
        HashMap<Character,Integer> needs = new HashMap<>();
        HashMap<Character,Integer> windows = new HashMap<>();

        //初始化needs
        for(int i=0;i<t.length();i++){
            //needs.getOrDefault(t.charAt(i),0)+1 含义是：needs如果包含t.charAt(i)，
            //则取出值+1;不包含取0+1
            needs.put(t.charAt(i),needs.getOrDefault(t.charAt(i),0)+1);
        }

        int left=0;
        int right=0;
        int count=0;
        int minLen= Integer.MAX_VALUE;
        int start=0;
        int end=0;
        while(right <s.length()){
            //获取字符
            char temp=s.charAt(right);
            //如果是t中字符，在windows里添加，累计出现次数
            if(needs.containsKey(temp)){
                windows.put(temp,windows.getOrDefault(temp,0)+1);
                //注意：Integer不能用==比较，要用compareTo
                if(windows.get(temp).compareTo(needs.get(temp))==0 ){
                    //字符temp出现次数符合要求，count代表符合要求的字符个数
                    count++;
                }
            }
            //优化到不满足情况，right继续前进找可行解
            right++;
            //符合要求的字符个数正好是t中所有字符，获得一个可行解
            while(count==needs.size()){
                //更新结果
                if(right-left<minLen){
                    start=left;
                    end=right;
                    minLen=end-left;
                }
                //开始进行优化，即缩小区间，删除s[left],
                char c=s.charAt(left);
                
                //当前删除的字符包含于t,更新Windows中对应出现的次数，如果更新后的次数<t中出现的次数，符合要求的字符数减1，下次判断count==needs.size()时不满足情况，
                if(needs.containsKey(c)){
                    windows.put(c,windows.getOrDefault(c,1)-1);
                    if(windows.get(c)<needs.get(c)){
                        count--;
                    }
                }
                left++;
            }    
        }
        //返回覆盖的最小串
        return minLen==Integer.MAX_VALUE ? "":s.substring(start,end);

    }
}
```
#  438. 找到字符串中所有字母异位词
* [原题地址](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> list = new ArrayList<>();
        int left = 0 , right = 0;
        char[] needs = new char[26];             //相当于hashMap，用于记录每个字符的个数
        char[] windows = new char[26];
        for (int i = 0; i < p.length(); i++) {
            needs[p.charAt(i)-'a']++;
        }
        //统计符合要求字符个数
        int count =0;
        while(right < s.length()){

            int ch = s.charAt(right)-'a';
            windows[ch]++;
            if (needs[ch] > 0 && needs[ch] >= windows[ch]){
                count++;
            }
            //长度满足条件
            while( count == p.length()){
                //加入符合要求的结果
                if (right + 1 - left == p.length()){
                    list.add(left);
                }
                //不符合要求，缩小区间
                int ch1 = s.charAt(left)-'a';
                if (needs[ch1] > 0){
                    windows[ch1]--;
                    if ( windows[ch1] < needs[ch1]){
                        count--;
                    }
                }
                left++;
            }
            right++;
        }
        return list;
    }
}
```
#  3. 无重复字符的最长子串
* [原题地址](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length()==0) return 0;
        HashMap<Character,Integer> map = new HashMap<>();
        int left = 0;
        int max = 0;//记录最长子串长度
        for(int i = 0;i<s.length();i++){
            if(map.containsKey(s.charAt(i))){
                left = Math.max(left,map.get(s.charAt(i))+1);
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-left+1);
        }
        return max;
    }
}
```
#  567. 字符串的排列
* [原题地址](https://leetcode-cn.com/problems/permutation-in-string/)

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int[] S1 = new int[26];
        int[] S2 = new int[26];
        for(int i = 0;i<s1.length();i++){
            S1[s1.charAt(i)-'a']++;
        }
        int left = 0;
        int right = 0;
        int count = 0;
        while(right<s2.length()){
            int ch = s2.charAt(right) - 'a';
            S2[ch]++;
            if(S1[ch]>0 && S1[ch]>=S2[ch]){
                count++;
            }

            while(count==s1.length()){
                if(s1.length()==right-left+1){
                    return true;
                }

                int ch1 = s2.charAt(left) - 'a';
                if(S1[ch1]>0){
                    S2[ch1]--;
                    if(S2[ch1]<S1[ch1]){
                        count--;
                    }
                }
                left++;
            }
            right++;
        }
        return false;
    }
}
```
**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
