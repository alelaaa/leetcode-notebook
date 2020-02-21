# 课程表 解题思路

## **题目：**

现在你总共有 n 门课需要选，记为 0 到 n-1。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]

给定课程总量以及它们的先决条件，判断是否可能完成所有课程的学习？

示例 1:

输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
示例 2:

输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
说明:

输入的先决条件是由边缘列表表示的图形，而不是邻接矩阵。详情请参见图的表示法。
你可以假定输入的先决条件中没有重复的边。
提示:

这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。
通过 DFS 进行拓扑排序 - 一个关于Coursera的精彩视频教程（21分钟），介绍拓扑排序的基本概念。
拓扑排序也可以通过 BFS 完成。

 * ## **思路：**

   拓扑排序

   1、创建一个一维数组存放顶点的入度
   
   2、将入度为0的顶点入栈
   
   3、只要栈非空，就从栈顶取出一个顶点，结果集加1，并且将这个顶点的所有邻接点的入度减1，在减1以后，发现这个邻接点的入度为0，就继续入栈。
   
   最后检查结果集的大小是否和课程数相同即可。
   
   ## **代码**:

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        //定义出度课程数组
        int len = prerequisites.length;
        int[] res = new int[numCourses];
        for(int i = 0;i<len;i++){
            res[prerequisites[i][1]]++; 
        }
        Stack stack = new Stack();
        //定义出度为0的栈
        for(int i = 0;i<numCourses;i++){
            if(res[i] == 0){
                stack.push(i);
            }
        }
        int m,count = 0;
        //弹栈
        //将出度数组--，为0则入栈
        while(!stack.isEmpty()){
            m = (int)stack.pop();
            count++;
            for(int i = 0;i<len;i++){
                if(prerequisites[i][0] == m){
                    res[prerequisites[i][1]]--;
                    if(res[prerequisites[i][1]] == 0){
                        stack.push(prerequisites[i][1]);
                    }
                }
            }
        }
        return count == numCourses;
    }
}
```