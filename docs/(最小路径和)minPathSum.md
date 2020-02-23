# 最小路径和  解题思路

## **题目：**



给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

示例:

输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。

 * ## **思路：**

   可以使用动态规划来解决，对于每一个状态都是由于上面或者左边的值加上当前值。
**过程：**
   
   1. 创建 一个dp数组，状态转移方程，
         $$
         dp[i][j] = Math.min(dp[i][j-1]+grid[i][j],dp[i-1][j]+grid[i][j])
         $$
         
   
    2. 考虑边界状态
     2.1 由于状态转移必须来自上一个状态，因此第一个状态
   $$
   dp[0][0]初始化为grid[0][0]
     $$
       2.2 最上一行的格子的上一个状态只可能来自左边
       2.2 最左一列的格子的上一个状态只可能来自上面
     
    3. 循环，需要注意的是当i为0时候j不能为0，避免数组越界
   
   复杂度分析：
   时间复杂度 O(M \times N)O(M×N) ： 遍历整个 gridgrid 矩阵元素。
   空间复杂度 O(1)O(1) ： 直接修改原矩阵，不使用额外空间。
   
   ## **代码**:

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int[][] dp = new int[grid.length][grid[0].length];
        for(int i = grid.length-1;i>=0;i--){
            for(int j = grid[0].length-1;j>=0;j--){
                //解决参数越界问题
                //当数为最后一行且不为最后一列
                if(i == grid.length-1 && j != grid[0].length -1){
                    dp[i][j] = grid[i][j] + dp[i][j+1];
                }
                //当数不为最后一行且为最后一列
                else if(i != grid.length-1 && j == grid[0].length -1){
                    dp[i][j] = grid[i][j] + dp[i+1][j];
                }else if(i != grid.length-1 && j != grid[0].length -1){
                    dp[i][j] = grid[i][j] + Math.min(dp[i][j+1],dp[i+1][j]);
                }else{
                    dp[i][j] = grid[i][j];
                }
            }
        }
        return dp[0][0];
    }
}
```