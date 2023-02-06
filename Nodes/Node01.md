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



## [1798. 你能构造出连续值的最大数目](https://leetcode.cn/problems/maximum-number-of-consecutive-values-you-can-make/)

> 贪心题，想了半天没想上来



```JAVA
/* 
    执行用时: 17 ms
    内存消耗: 49.4 MB
*/
class Solution {
    public int getMaximumConsecutive(int[] coins) {
        int m = 0; // 一开始只能构造出 0
        Arrays.sort(coins);
        for (int c : coins) {
            if (c > m + 1) // coins 已排序，后面没有比 c 更小的数了
                break; // 无法构造出 m+1，继续循环没有意义
            m += c; // 可以构造出区间 [0,m+c] 中的所有整数
        }
        return m + 1; // [0,m] 中一共有 m+1 个整数
    }
}
```



## [359. 日志速率限制器](https://leetcode.cn/problems/logger-rate-limiter/)





```JAVA
/* 
    执行用时: 28 ms
    内存消耗: 49.6 MB
*/
class Logger {

    Map<String, Integer> map;

    public Logger() {
        map = new HashMap<>();
    }
    
    public boolean shouldPrintMessage(int timestamp, String message) {
        int val = map.getOrDefault(message, -20);
        if (timestamp - val >= 10) {
            map.put(message, timestamp);
            return true;
        }
        return false;
    }
}

/**
 * Your Logger object will be instantiated and called as such:
 * Logger obj = new Logger();
 * boolean param_1 = obj.shouldPrintMessage(timestamp,message);
 */
```



## [346. 数据流中的移动平均值](https://leetcode.cn/problems/moving-average-from-data-stream/)



```JAVA
/* 
    执行用时： 36 ms , 在所有 Java 提交中击败了 95.58% 的用户
    内存消耗： 45.5 MB , 在所有 Java 提交中击败了 75.14% 的用户
    通过测试用例： 11 / 11
*/
class MovingAverage {
    Queue<Integer> queue = null;
    int len = 0;
    double sum = 0;

    public MovingAverage(int size) {
        len = size;
        queue = new LinkedList<>();
    }
    
    public double next(int val) {
        queue.add(val);
        sum += val;
        if (queue.size() > len) {
            sum -= queue.poll();
        }
        
        return  sum / queue.size();
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```



## [1210. 穿过迷宫的最少移动次数](https://leetcode.cn/problems/minimum-moves-to-reach-target-with-rotations/)



```JAVA
/* 
    执行用时： 15 ms , 在所有 Java 提交中击败了 51.28% 的用户
    内存消耗： 41.8 MB , 在所有 Java 提交中击败了 76.92% 的用户
    通过测试用例： 42 / 42
*/
class Solution {
    private int n;
    private int[][] grid;
    private boolean[][] vis;
    private Deque<int[]> q = new ArrayDeque<>();

    public int minimumMoves(int[][] grid) {
        this.grid = grid;
        n = grid.length;
        vis = new boolean[n * n][2];
        int[] target = {n * n - 2, n * n - 1};
        q.offer(new int[] {0, 1});
        vis[0][0] = true;
        int ans = 0;
        while (!q.isEmpty()) {
            for (int k = q.size(); k > 0; --k) {
                var p = q.poll();
                if (p[0] == target[0] && p[1] == target[1]) {
                    return ans;
                }
                int i1 = p[0] / n, j1 = p[0] % n;
                int i2 = p[1] / n, j2 = p[1] % n;
                // 尝试向右平移（保持身体水平/垂直状态）
                move(i1, j1 + 1, i2, j2 + 1);
                // 尝试向下平移（保持身体水平/垂直状态）
                move(i1 + 1, j1, i2 + 1, j2);
                // 当前处于水平状态，且 grid[i1 + 1][j2] 无障碍，尝试顺时针旋转90°
                if (i1 == i2 && i1 + 1 < n && grid[i1 + 1][j2] == 0) {
                    move(i1, j1, i1 + 1, j1);
                }
                // 当前处于垂直状态，且 grid[i2][j1 + 1] 无障碍，尝试逆时针旋转90°
                if (j1 == j2 && j1 + 1 < n && grid[i2][j1 + 1] == 0) {
                    move(i1, j1, i1, j1 + 1);
                }
            }
            ++ans;
        }
        return -1;
    }

    private void move(int i1, int j1, int i2, int j2) {
        if (i1 >= 0 && i1 < n && j1 >= 0 && j1 < n && i2 >= 0 && i2 < n && j2 >= 0 && j2 < n) {
            int a = i1 * n + j1, b = i2 * n + j2;
            int status = i1 == i2 ? 0 : 1;
            if (!vis[a][status] && grid[i1][j1] == 0 && grid[i2][j2] == 0) {
                q.offer(new int[] {a, b});
                vis[a][status] = true;
            }
        }
    }
}
```



## [2331. 计算布尔二叉树的值](https://leetcode.cn/problems/evaluate-boolean-binary-tree/)



```JAVA
/* 
    执行用时： 0 ms , 在所有 Java 提交中击败了 100.00% 的用户
    内存消耗： 41.4 MB , 在所有 Java 提交中击败了 70.42% 的用户
    通过测试用例： 75 / 75
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
    public boolean evaluateTree(TreeNode root) {
        return dfs(root);
    }

    boolean dfs(TreeNode node) {
        if (node.val == 0) {
            return false;
        }else if (node.val == 1) {
            return true;
        }else if (node.val == 2) {
            return dfs(node.left) | dfs(node.right);
        }else {
            return dfs(node.left) & dfs(node.right);
        }
    }
}
```



## [832. 翻转图像](https://leetcode.cn/problems/flipping-an-image/)



```JAVA
/* 
    执行用时： 0 ms , 在所有 Java 提交中击败了 100.00% 的用户
    内存消耗： 41.9 MB , 在所有 Java 提交中击败了 16.09% 的用户
    通过测试用例： 82 / 82
*/
class Solution {
    public int[][] flipAndInvertImage(int[][] image) {
        int[][] arr = new int[image.length][image.length];

        for (int i = 0;i < arr.length;i ++) {
            for (int j = 0;j < arr.length;j ++) {
                if (image[i][j] == 1) {
                    arr[i][arr.length - j - 1] = 0;
                }else {
                    arr[i][arr.length - j - 1] = 1;
                }
            }
        }
        return arr;
    }
}
```



## [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)



```JAVA
/* 
    执行用时： 22 ms , 在所有 Java 提交中击败了 69.66% 的用户
    内存消耗： 42.1 MB , 在所有 Java 提交中击败了 61.85% 的用户
    通过测试用例： 106 / 106
*/
class Solution {
    public String removeDuplicates(String s) {
        StringBuffer sb = new StringBuffer();
        int top = -1;
        for (int i = 0; i < s.length(); ++i) {
            char ch = s.charAt(i);
            if (top >= 0 && sb.charAt(top) == ch) {
                sb.deleteCharAt(top);
                --top;
            } else {
                sb.append(ch);
                ++top;
            }
        }
        return sb.toString();
    }
}
```

