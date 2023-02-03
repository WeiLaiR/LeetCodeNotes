# Node01

## [2319. 判断矩阵是否是一个 X 矩阵](https://leetcode.cn/problems/check-if-matrix-is-x-matrix/)



```JAVA
/* 执行用时: 2 ms
*  内存消耗: 42.2 MB
*/
class Solution {
    public boolean checkXMatrix(int[][] grid) {
        int n = grid.length;
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                //若节点为0，则判断是否在对角线即可。
                if ((grid[i][j] == 0) == (i == j || i + j == n - 1))
                    return false;
        return true;
    }
}
```



## [1469. 寻找所有的独生节点](https://leetcode.cn/problems/find-all-the-lonely-nodes/)



```JAVA
/* 执行用时: 0 ms
*  内存消耗: 41.9 MB
*/
class Solution {
    public List<Integer> getLonelyNodes(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        dfs(list, root);
        return list;
    }
	
    //经典遍历二叉树问题，用dfs bfs均可
    void dfs(List<Integer> list, TreeNode node) {
        if (node == null) {
            return;
        }

        if (node.left != null && node.right == null) {
            list.add(node.left.val);
        }else if (node.left == null && node.right != null) {
            list.add(node.right.val);
        }

        dfs(list, node.left);
        dfs(list, node.right);
    }
}
```



## [1427. 字符串的左右移](https://leetcode.cn/problems/perform-string-shifts/)



```JAVA
/* 执行用时: 0 ms
*  内存消耗: 39.5 MB
*/
class Solution {
    public String stringShift(String s, int[][] shift) {
        int len = s.length();
        int val = 0;
        //思路相对简单，仅需计算出字符串头位置，在进行字符串拆分拼接即可
        for (int i = 0; i < shift.length; i ++) {
            if (shift[i][0] == 0) {
                val += shift[i][1];
                val %= len;
            }else {
                val -= shift[i][1];
                val %= len;
                if (val < 0) {
                    val += len;
                }
            }
        }
        return s.substring(val).concat(s.substring(0,val));
    }
}
```



## [2325. 解密消息](https://leetcode.cn/problems/decode-the-message/)



```JAVA
/* 
    执行用时: 4 ms
    内存消耗: 41.9 MB
*/
class Solution {
    public String decodeMessage(String key, String message) {
        //去重
        Set<Character> set = new HashSet<>();
        //使用数组下标查询更快
        char[] chs = new char[26];
        int index = 0;

        for (int i = 0;i < key.length();i ++) {
            if (index == chs.length) {
                break;
            }
            char temp = key.charAt(i);
            if (temp == ' ') {
                continue;
            }else if (!set.contains(temp)) {
                set.add(temp);
                chs[temp - 'a'] = (char)(index + 'a');
                index ++;
            }
        }

        //StringBuilder单线程下对字符串操作更快
        StringBuilder sb = new StringBuilder();

        for (index = 0; index < message.length(); index ++) {
            char temp = message.charAt(index);
            if (temp == ' ') {
                sb.append(' ');
            }else {
                sb.append(chs[temp - 'a']);
            }
        }

        return sb.toString();
    }
}
```



## [1426. 数元素](https://leetcode.cn/problems/counting-elements/)





```JAVA
/* 
    执行用时: 1 ms
    内存消耗: 39.5 MB
*/
class Solution {
    public int countElements(int[] arr) {
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < arr.length; i ++) {
            set.add(arr[i]);
        }
        int count = 0;
        for (int i = 0; i < arr.length; i ++) {
            if (set.contains(arr[i] + 1))
                count ++;
        }
        return count;
    }
}
```



## [1271. 十六进制魔术数字](https://leetcode.cn/problems/hexspeak/)



```JAVA
/* 
    执行用时: 2 ms
    内存消耗: 40.4 MB
*/
class Solution {
    public String toHexspeak(String num) {
        //z
        String str = Long.toHexString(Long.valueOf(num));
        StringBuilder upper = new StringBuilder(str.toUpperCase());

        for (int i = 0; i < upper.length(); i ++) {
            char ch = upper.charAt(i);
            if (ch == '0') {
                upper.setCharAt(i,'O');
            }else if (ch == '1') {
                upper.setCharAt(i,'I');
            }else if (ch >= '2' && ch <= '9') {
                return "ERROR";
            }
        }
        return upper.toString();
    }
}
```



## [1129. 颜色交替的最短路径](https://leetcode.cn/problems/shortest-path-with-alternating-colors/)

> 这个题属实搞了我一手，题目细节表达的不是很清楚，让我理解错题意了。



```JAVA
/* 
    执行用时: 3 ms
    内存消耗: 42.1 MB
*/
public class Solution{
    public int[] shortestAlternatingPaths(int n, int[][] redEdges, int[][] blueEdges){
        List<Integer>[] redList = new List[n];
        List<Integer>[] blueList = new List[n];
        
        for(int i = 0; i < n; ++i){
            redList[i] = new ArrayList<>();
            blueList[i] = new ArrayList<>();
        }
        
        for(int[] e : redEdges){
            redList[e[0]].add(e[1]);
        }
        
        for(int[] e : blueEdges){
            blueList[e[0]].add(e[1]);
        }
        
        int[] redAns = new int[n];// 最后一步为 [红色] 时到达点i的 [最小] 步数
        int[] blueAns = new int[n];// 最后一步为 [蓝色] 时到达点i的 [最小] 步数
        
        for(int i = 1; i < n; i++) {// 初始化 所有 [最小] 步数全部设置成 [MAX]
            redAns[i] = Integer.MAX_VALUE; // 从 [1] 开始因为从 [点0] 到 [点0] 需要 [0] 步
            blueAns[i] = Integer.MAX_VALUE;
        }
        
        Queue<int[]> queue = new ArrayDeque<>();// 由长度为 [2] 的数组表示每个点
                                                // 点的序号 + 下一次要走的 [颜色]
        
        queue.add(new int[]{0, 0});// [0] 表示下一次要走 [红色]
        queue.add(new int[]{0, 1});// [1] 表示下一次要走 [蓝色]
                                // 初始状态有 [2] 个因为一开始可以走 [红色] 或者 [蓝色]
        
        int level = 0;// bfs的层数 = 走的步数
                    // 在层数 [i] 到达的点 意味着从 [0] 开始走 [i] 步可以到达这个点
        //循环中会走完所有的可能
        while(!queue.isEmpty()){
            level++;// bfs的层数 [+1] 
            
            int size = queue.size();
            // 对于每个准备走的点
            for(int i = 0; i < size; i++) {
                int[] curArr = queue.poll();
                int cur = curArr[0];
                
                if(curArr[1] == 0) {// 如果这个点下一步要走 [红色]
                
                    for(int next : redList[cur]) {// 从 [红色] 的邻接表里找可以走到的下一个点'next'
                        if(level < redAns[next]) {// 如果记录的最后一步为 [红色] 时到达点'next'
                                            // 的 [最小] 步数 [大于] 当前层数
                            redAns[next] = level;// 更新 [最小] 步数
                            queue.offer(new int[]{next, 1});//将点'next'入队 并且下一次要走 [蓝色]
                        }
                    }
                }
                else{// 如果这个点下一步要走 [蓝色] 时同理
                    for(int next : blueList[cur]){
                        if(level < blueAns[next]){
                            blueAns[next] = level;
                            queue.offer(new int[]{next, 0});
                        }
                    }
                }
            }
        }
        
        // 到达一个点的最小步数为 min([红色]结尾时的[最小]步数，[蓝色]结尾时的[最小]步数)
        for(int i = 0; i < redAns.length; i++){
            if(blueAns[i] < redAns[i]){
                redAns[i] = blueAns[i];
            }
            else if(redAns[i] == Integer.MAX_VALUE){// 如果都为 [MAX] 证明无法走到
                redAns[i] = -1;
            }
        }
        
        return redAns;
    }
}
```



## [246. 中心对称数](https://leetcode.cn/problems/strobogrammatic-number/)



```JAVA
/* 
    执行用时: 0 ms
    内存消耗: 39.5 MB
*/
class Solution {
    public boolean isStrobogrammatic(String num) {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < num.length(); i ++) {
            char ch = num.charAt(i);
            if (ch == '6') 
                sb.append('9');
            else if (ch == '9')
                sb.append('6');
            else if (ch == '1' || ch == '8' || ch == '0')
                sb.append(ch);
            else
                return false;
        }

        if (num.equals(sb.reverse().toString())) 
            return true;
        return false;
    }
}
```



## [252. 会议室](https://leetcode.cn/problems/meeting-rooms/)



```JAVA
/* 
    执行用时: 4 ms
    内存消耗: 41.3 MB
*/
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (o1, o2) -> o1[0] == o2[0] ? o1[1] - o2[1] : o1[0] - o2[0]);

        for (int i = 1;i < intervals.length;i ++)
            if (intervals[i][0] < intervals[i - 1][1])
                return false;

        return true;
    }
}
```





## [1145. 二叉树着色游戏](https://leetcode.cn/problems/binary-tree-coloring-game/)





```JAVA
/* 
    执行用时: 0 ms
    内存消耗: 39.2 MB
*/
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int ltnc = 0, rtnc = 0;
    public boolean btreeGameWinningMove(TreeNode root, int n, int x) {
        nodeCount(root, x);
        return 2*ltnc > n || 2*rtnc > n || 2*(ltnc + rtnc + 1) < n;
    }
    public int nodeCount(TreeNode t, int v) {
        if (t == null) return 0;
        int lc = nodeCount(t.left, v);
        int rc = nodeCount(t.right, v);
        if (t.val == v) { // 找到x选择的node，开始记录这个node的左子树和右子树的节点个数
            ltnc = lc;
            rtnc = rc;
        }
        return lc + rc + 1;
    }
}

```

