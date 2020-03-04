# 最小覆盖子串 解题思路

## **题目：**

给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字母的最小子串。

示例：

输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"
说明：

如果 S 中不存这样的子串，则返回空字符串 ""。
如果 S 中存在这样的子串，我们保证它是唯一的答案。

 * ## **解题**

   ### 1.自己的解法

   ​		目的：字符串s中找出能包含t所有字母的最短串
   

   **自己的解法代码：**

```java
public class Solution {

  public static String minWindow(String s, String t) {
        if (s.length() < t.length()) {
            return "";
        }
        int[] map_s = new int[128];
        int[] map_t = new int[128];
        int begin = 0;
        char[] chars_s = s.toCharArray();
        char[] chars_t = t.toCharArray();
        for (int i = 0; i < t.length(); i++) {
            map_t[chars_t[i]] += 1;
        }
        String result = "";
        for (int next = 0; next < s.length(); next++) {
            map_s[chars_s[next]]++;
            while (begin < next) {
                if (map_t[chars_s[begin]] == 0) {
                    begin++;
                    //不是t中元素
                } else if (map_s[chars_s[begin]] > map_t[chars_s[begin]]) {
                    map_s[chars_s[begin]]--;
                    begin++;
                } else {
                    break;
                }
            }
            boolean windowOk = isWindowOk(map_s, map_t);
            if (windowOk) {
                String temp = s.substring(begin, next + 1);
                if (result.equals("") || result.length() > temp.length()) {
                    result = temp;
                }
            }
        }

        return result;
    }

    private static boolean isWindowOk(int[] chars_s, int[] chars_t) {
        for (int i = 0; i < chars_t.length; i++) {
            if (chars_t[i] > 0) {
                if (chars_s[i] < chars_t[i]) {
                    return false;
                }
            }
        }
        return true;
    }

}
```

## **2.优秀解法**

**思路：**

两个指针，移动右指针使得满足条件，移动左指针缩短距离。用hashmap存储进行判断是否满足条件。

```java
public class Solution {

  public static String minWindow(String s, String t) {
        HashMap<Character,Integer> mp = new HashMap();
        for (int i = 0; i < t.length() ; i++) { // 统计每个字符出现的个数
            char ch = t.charAt(i);
            if(mp.containsKey(ch))
                mp.put(ch,mp.get(ch)+1);
            else
                mp.put(t.charAt(i),1);
        }
        int right = 0;
        int left = 0;
        int count = 0;
        int res_left = 0;
        int res_len = s.length()+1;
        while(right<s.length()){
            // 移动右指针,到能够覆盖t
            char ch_r = s.charAt(right);
            if(mp.containsKey(ch_r)){
                mp.put(ch_r,mp.get(ch_r)-1);
                if(mp.get(ch_r)>=0) // <0说明重复了
                    count++;
            }
            while(count==t.length()){//右移左指针，注意这个判定条件
                if(right-left+1<res_len){ //更新结果
                    res_left = left;
                    res_len = right-left+1;
                }
                char ch_l = s.charAt(left);
                if(mp.containsKey(ch_l)){
                    mp.put(ch_l,mp.get(ch_l)+1);
                    if(mp.get(ch_l)>0)
                        count--;
                }
                left++;
            }
            right++;
        }
        if(res_len==s.length()+1)
            return "";
        return s.substring(res_left,res_left+res_len);
    }

    public String minWindow2(String s, String t) {
        HashMap<Character, Integer> hm = new HashMap();
        char[] t_arr = t.toCharArray();
        for(int i=0; i<t_arr.length; i++){
            hm.put(t_arr[i], hm.getOrDefault(t_arr[i], 0)+1);
        }
        int left = 0;
        int right = 0;
        int count = t.length();
        char[] s_arr = s.toCharArray();
        int res = Integer.MAX_VALUE;
        int res_left = 0;
        int res_right = 0;
        while(right<s.length()){    //一共两个while
            if(hm.containsKey(s_arr[right])){
                hm.put(s_arr[right], hm.get(s_arr[right])-1);
                if(hm.get(s_arr[right])>=0) count--;    //别忘了这一层的判断
            }
            right++;    //在if外边
            while(count==0){    //注意判断条件
                if(right-left+1<res){   //记录结果
                    res = right-left+1;
                    res_left = left;
                    res_right = right;
                }
                if(hm.containsKey(s_arr[left])){
                    hm.put(s_arr[left], hm.get(s_arr[left])+1);
                    if(hm.get(s_arr[left])>0) count++;
                }
                left++;
            }
        }
        return s.substring(res_left, res_right);
    }

}
```

