# 子集 解题思路

## **题目：**

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

 * ## **解题**

   ### **1.自己的解法**

   直接从前往后遍历，遇到一个数就把所有子集加上该数组成新的子集，遍历完毕即是所有子集。
   

   **回溯解法代码：**

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        //添加空集
        res.add(new ArrayList<>());
        for(int i = 0;i < nums.length;i++){
            int len = res.size();
            for(int j = 0;j < len;j++){
                List<Integer> newList = new ArrayList<>(res.get(j));
                newList.add(nums[i]);
                res.add(newList);
            }
        }
        return res;
    }
}
```

**2.回溯解法**

回溯算法关键在于:不合适就退回上一步

然后通过约束条件, 减少时间复杂度.

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        backtrack(nums,result,0,new ArrayList<>());
        return result;
    }
    public void backtrack(int[] nums,List<List<Integer>> result,int i,List<Integer> l){
        if(i>nums.length){return ;}
        if(!result.contains(l)){
            result.add(new ArrayList<>(l));
        }
        for(int start=i;start<nums.length;start++){
            l.add(nums[start]);
            backtrack(nums,result,start+1,l);
            l.remove(l.size()-1);
        }
    }
}
```

