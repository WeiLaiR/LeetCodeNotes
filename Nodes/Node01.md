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



## [1604. 警告一小时内使用相同员工卡大于等于三次的人](https://leetcode.cn/problems/alert-using-same-key-card-three-or-more-times-in-a-one-hour-period/)



```JAVA
/* 
    执行用时： 42 ms , 在所有 Java 提交中击败了 97.92% 的用户
    内存消耗： 59.7 MB , 在所有 Java 提交中击败了 77.91% 的用户
    通过测试用例： 77 / 77
*/
class Solution {
    public List<String> alertNames(String[] keyName, String[] keyTime) {
        Map<String, List<Integer>> timeMap = new HashMap<String, List<Integer>>();
        int n = keyName.length;
        for (int i = 0; i < n; i++) {
            String name = keyName[i];
            String time = keyTime[i];
            timeMap.putIfAbsent(name, new ArrayList<Integer>());
            int hour = (time.charAt(0) - '0') * 10 + (time.charAt(1) - '0');
            int minute = (time.charAt(3) - '0') * 10 + (time.charAt(4) - '0');
            timeMap.get(name).add(hour * 60 + minute);
        }
        List<String> res = new ArrayList<String>();
        Set<String> keySet = timeMap.keySet();
        for (String name : keySet) {
            List<Integer> list = timeMap.get(name);
            Collections.sort(list);
            int size = list.size();
            for (int i = 2; i < size; i++) {
                int time1 = list.get(i - 2), time2 = list.get(i);
                int difference = time2 - time1;
                if (difference <= 60) {
                    res.add(name);
                    break;
                }
            }
        }
        Collections.sort(res);
        return res;
    }
}

```



## [1233. 删除子文件夹](https://leetcode.cn/problems/remove-sub-folders-from-the-filesystem/)



```JAVA
/* 
    执行用时： 52 ms , 在所有 Java 提交中击败了 53.85% 的用户
    内存消耗： 50.2 MB , 在所有 Java 提交中击败了 88.17% 的用户
    通过测试用例： 32 / 32
*/
public class Solution {
    public List<String> removeSubfolders(String[] folder) {
        Arrays.sort(folder);
        List<String> result = new ArrayList<>();
        result.add(folder[0]);
        for (int i = 1; i < folder.length; i++) {
            StringBuilder sb = new StringBuilder(result.get(result.size() - 1)).append("/");
            if (!folder[i].startsWith(sb.toString())) result.add(folder[i]);
        }
        return result;
    }
}
```



## [1797. 设计一个验证系统](https://leetcode.cn/problems/design-authentication-manager/)



```JAVA
/* 
    执行用时： 26 ms , 在所有 Java 提交中击败了 98.82% 的用户
    内存消耗： 42.8 MB , 在所有 Java 提交中击败了 67.06% 的用户
    通过测试用例： 90 / 90
*/
class AuthenticationManager {

    private int timeToLive;
    // 存储id的最后一次更新的时间
    private final HashMap<String, Integer> map = new HashMap<>();
    // 使用两个列表分别储存所有记录的id和time
    List<String> idList = new ArrayList<>();
    List<Integer> timeList = new ArrayList<>();
    // 表示上一次查询未过时验证码时，第一个未过时的验证码在列表中的索引
    int start = 0;

    public AuthenticationManager(int timeToLive) {
        this.timeToLive = timeToLive;
    }

    public void generate(String tokenId, int currentTime) {
        map.put(tokenId, currentTime);
        idList.add(tokenId);
        timeList.add(currentTime);
    }

    public void renew(String tokenId, int currentTime) {
        if (map.containsKey(tokenId)) {
            // 若该id存在且未过时，则更新map并给列表添加记录
            if (map.get(tokenId) > currentTime - timeToLive) {
                map.put(tokenId, currentTime);
                idList.add(tokenId);
                timeList.add(currentTime);
            }
        }
    }

    public int countUnexpiredTokens(int currentTime) {
        int n = idList.size();
        int i = start;
        for (; i < n; i++) {
            String id = idList.get(i);
            Integer time = timeList.get(i);
            // 遇到没超时的记录就停止遍历
            if (time > currentTime - timeToLive) break;
            // 如果这一id的最后一次更新时间和该次记录的时间相同，则说明这一id已经过时
            if (map.get(id).equals(time)) {
                map.remove(id);
            }
        }
        // 因为下一次时间肯定不早于这次的时间，所以下一次查询从start开始即可
        start = i;
        return map.size();
    }
}

/**
 * Your AuthenticationManager object will be instantiated and called as such:
 * AuthenticationManager obj = new AuthenticationManager(timeToLive);
 * obj.generate(tokenId,currentTime);
 * obj.renew(tokenId,currentTime);
 * int param_3 = obj.countUnexpiredTokens(currentTime);
 */
```



## [1223. 掷骰子模拟](https://leetcode.cn/problems/dice-roll-simulation/)



```JAVA
class Solution {
    private static final long MOD = (long) 1e9 + 7;

    public int dieSimulator(int n, int[] rollMax) {
        int m = rollMax.length; // 6
        var f = new int[n][m][15];
        for (int j = 0; j < m; ++j)
            Arrays.fill(f[0][j], 1);
        for (int i = 1; i < n; ++i)
            for (int last = 0; last < m; ++last)
                for (int left = 0; left < rollMax[last]; ++left) {
                    long res = 0;
                    for (int j = 0; j < m; ++j)
                        if (j != last) res += f[i - 1][j][rollMax[j] - 1];
                        else if (left > 0) res += f[i - 1][j][left - 1];
                    f[i][last][left] = (int) (res % MOD);
                }
        long ans = 0;
        for (int j = 0; j < m; ++j)
            ans += f[n - 1][j][rollMax[j] - 1];
        return (int) (ans % MOD);
    }
}
```





## [2335. 装满杯子需要的最短总时长](https://leetcode.cn/problems/minimum-amount-of-time-to-fill-cups/)



```JAVA
/* 
    执行用时： 1 ms , 在所有 Java 提交中击败了 77.90% 的用户
    内存消耗： 39.6 MB , 在所有 Java 提交中击败了 7.25% 的用户
    通过测试用例： 280 / 280
*/
class Solution {
    public int fillCups(int[] amount) {
        Arrays.sort(amount);
        if (amount[0] + amount[1] <= amount[2]) {
            return amount[2];
        }
        return (amount[0] + amount[1] + amount[2] + 1) / 2;
    }
}
```



## [1138. 字母板上的路径](https://leetcode.cn/problems/alphabet-board-path/)



```JAVA
/* 
    执行用时： 1 ms , 在所有 Java 提交中击败了 54.76% 的用户
    内存消耗： 39.6 MB , 在所有 Java 提交中击败了 48.81% 的用户
    通过测试用例： 61 / 61
*/
class Solution {
    public String alphabetBoardPath(String target) {
        var ans = new StringBuilder();
        int x = 0, y = 0;
        for (var c : target.toCharArray()) {
            int nx = (c - 'a') / 5, ny = (c - 'a') % 5; // 目标位置
            var v = nx < x ? "U".repeat(x - nx) : "D".repeat(nx - x); // 竖直
            var h = ny < y ? "L".repeat(y - ny) : "R".repeat(ny - y); // 水平
            ans.append(c != 'z' ? v + h : h + v).append('!');
            x = nx;
            y = ny;
        }
        return ans.toString();
    }
}
```



## [1234. 替换子串得到平衡字符串](https://leetcode.cn/problems/replace-the-substring-for-balanced-string/)



```JAVA
/* 
    执行用时： 5 ms , 在所有 Java 提交中击败了 90.98% 的用户
    内存消耗： 41.7 MB , 在所有 Java 提交中击败了 33.61% 的用户
    通过测试用例： 40 / 40
*/
class Solution {
    public int balancedString(String S) {
        var s = S.toCharArray();
        var cnt = new int['X']; // 也可以用哈希表，不过数组更快一些
        for (var c : s) ++cnt[c];
        int n = s.length, m = n / 4;
        if (cnt['Q'] == m && cnt['W'] == m && cnt['E'] == m && cnt['R'] == m)
            return 0; // 已经符合要求啦
        int ans = n, left = 0;
        for (int right = 0; right < n; right++) { // 枚举子串右端点
            --cnt[s[right]];
            while (cnt['Q'] <= m && cnt['W'] <= m && cnt['E'] <= m && cnt['R'] <= m) {
                ans = Math.min(ans, right - left + 1);
                ++cnt[s[left++]]; // 缩小子串
            }
        }
        return ans;
    }
}
```



## [1124. 表现良好的最长时间段](https://leetcode.cn/problems/longest-well-performing-interval/)



```JAVA
/* 
    执行用时： 6 ms , 在所有 Java 提交中击败了 92.00% 的用户
    内存消耗： 41.8 MB , 在所有 Java 提交中击败了 78.75% 的用户
    通过测试用例： 98 / 98
*/
class Solution {
    public int longestWPI(int[] hours) {
        int n = hours.length, ans = 0;
        var s = new int[n + 1]; // 前缀和
        var st = new ArrayDeque<Integer>();
        st.push(0); // s[0]
        for (int j = 1; j <= n; ++j) {
            s[j] = s[j - 1] + (hours[j - 1] > 8 ? 1 : -1);
            if (s[j] < s[st.peek()]) st.push(j); // 感兴趣的 j
        }
        for (int i = n; i > 0; --i)
            while (!st.isEmpty() && s[i] > s[st.peek()])
                ans = Math.max(ans, i - st.pop()); // [栈顶,i) 可能是最长子数组
        return ans;
    }
}
```



## [1250. 检查「好数组」](https://leetcode.cn/problems/check-if-it-is-a-good-array/)

> 数据范围过大，硬想也没用什么好的思路。实际是应用了裴蜀定理，在这个题之前我感觉我并没有听说过这个定理。



```JAVA
/* 
    执行用时： 2 ms , 在所有 Java 提交中击败了 100.00% 的用户
    内存消耗： 50.4 MB , 在所有 Java 提交中击败了 49.29% 的用户
    通过测试用例： 47 / 47
*/
class Solution {
    public boolean isGoodArray(int[] nums) {
        int g = 0;
        for (int x : nums) {
            g = gcd(x, g);
        }
        return g == 1;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```



## [2341. 数组能形成多少数对](https://leetcode.cn/problems/maximum-number-of-pairs-in-array/)



```JAVA
/* 
    执行用时： 1 ms , 在所有 Java 提交中击败了 54.00% 的用户
    内存消耗： 40.2 MB , 在所有 Java 提交中击败了 55.20% 的用户
    通过测试用例： 128 / 128
*/
class Solution {
    public int[] numberOfPairs(int[] nums) {
        Map<Integer, Boolean> cnt = new HashMap<Integer, Boolean>();
        int res = 0;
        for (int num : nums) {
            cnt.put(num, !cnt.getOrDefault(num, false));
            if (!cnt.get(num)) {
                res++;
            }
        }
        return new int[]{res, nums.length - 2 * res};
    }
}
```



## [1139. 最大的以 1 为边界的正方形](https://leetcode.cn/problems/largest-1-bordered-square/)



```JAVA
/* 
    执行用时： 5 ms , 在所有 Java 提交中击败了 79.44% 的用户
    内存消耗： 42.8 MB , 在所有 Java 提交中击败了 9.86% 的用户
    通过测试用例： 84 / 84
*/
class Solution {
    public int largest1BorderedSquare(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] left = new int[m + 1][n + 1];
        int[][] up = new int[m + 1][n + 1];
        int maxBorder = 0;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (grid[i - 1][j - 1] == 1) {
                    left[i][j] = left[i][j - 1] + 1;
                    up[i][j] = up[i - 1][j] + 1;
                    int border = Math.min(left[i][j], up[i][j]);
                    while (left[i - border + 1][j] < border || up[i][j - border + 1] < border) {
                        border--;
                    }
                    maxBorder = Math.max(maxBorder, border);
                }
            }
        }
        return maxBorder * maxBorder;
    }
}
```





## [1237. 找出给定方程的正整数解](https://leetcode.cn/problems/find-positive-integer-solution-for-a-given-equation/)



```JAVA
/* 
    执行用时： 135 ms , 在所有 Java 提交中击败了 12.94% 的用户
    内存消耗： 39.5 MB , 在所有 Java 提交中击败了 32.89% 的用户
    通过测试用例： 45 / 45
*/
/*
 * // This is the custom function interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 *     // Returns f(x, y) for any given positive integers x and y.
 *     // Note that f(x, y) is increasing with respect to both x and y.
 *     // i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 *     public int f(int x, int y);
 * };
 */

class Solution {
    public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        for (int x = 1; x <= 1000; x++) {
            for (int y = 1; y <= 1000; y++) {
                if (customfunction.f(x, y) == z) {
                    List<Integer> pair = new ArrayList<Integer>();
                    pair.add(x);
                    pair.add(y);
                    res.add(pair);
                }
            }
        }
        return res;
    }
}
```





## [1792. 最大平均通过率](https://leetcode.cn/problems/maximum-average-pass-ratio/)



```JAVA
/* 
    执行用时： 486 ms , 在所有 Java 提交中击败了 64.00% 的用户
    内存消耗： 98.4 MB , 在所有 Java 提交中击败了 42.67% 的用户
    通过测试用例： 87 / 87
*/
class Solution {
    public double maxAverageRatio(int[][] classes, int extraStudents) {
        PriorityQueue<double[]> pq = new PriorityQueue<>((a, b) -> {
            double x = (a[0] + 1) / (a[1] + 1) - a[0] / a[1];
            double y = (b[0] + 1) / (b[1] + 1) - b[0] / b[1];
            return Double.compare(y, x);
        });
        for (var e : classes) {
            pq.offer(new double[] {e[0], e[1]});
        }
        while (extraStudents-- > 0) {
            var e = pq.poll();
            double a = e[0] + 1, b = e[1] + 1;
            pq.offer(new double[] {a, b});
        }
        double ans = 0;
        while (!pq.isEmpty()) {
            var e = pq.poll();
            ans += e[0] / e[1];
        }
        return ans / classes.length;
    }
}

```



## [2347. 最好的扑克手牌](https://leetcode.cn/problems/best-poker-hand/)



```JAVA
/* 
    执行用时： 0 ms , 在所有 Java 提交中击败了 100.00% 的用户
    内存消耗： 39.2 MB , 在所有 Java 提交中击败了 43.37% 的用户
    通过测试用例： 98 / 98
*/
class Solution {
    public String bestHand(int[] ranks, char[] suits) {
        boolean flush = true;
        for (int i = 1; i < 5 && flush; ++i) {
            flush = suits[i] == suits[i - 1];
        }
        if (flush) {
            return "Flush";
        }
        int[] cnt = new int[14];
        boolean pair = false;
        for (int x : ranks) {
            if (++cnt[x] == 3) {
                return "Three of a Kind";
            }
            pair = pair || cnt[x] == 2;
        }
        return pair ? "Pair" : "High Card";
    }
}
```



## [1326. 灌溉花园的最少水龙头数目](https://leetcode.cn/problems/minimum-number-of-taps-to-open-to-water-a-garden/)



```JAVA
/* 
    执行用时： 11 ms , 在所有 Java 提交中击败了 29.49% 的用户
    内存消耗： 42 MB , 在所有 Java 提交中击败了 20.51% 的用户
    通过测试用例： 37 / 37
*/
class Solution {
    public int minTaps(int n, int[] ranges) {
        int[][] intervals = new int[n + 1][];
        for (int i = 0; i <= n; i++) {
            int start = Math.max(0, i - ranges[i]);
            int end = Math.min(n, i + ranges[i]);
            intervals[i] = new int[]{start, end};
        }
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for (int[] interval : intervals) {
            int start = interval[0], end = interval[1];
            if (dp[start] == Integer.MAX_VALUE) {
                return -1;
            }
            for (int j = start; j <= end; j++) {
                dp[j] = Math.min(dp[j], dp[start] + 1);
            }
        }
        return dp[n];
    }
}

```



## [2168. 每个数字的频率都相同的独特子字符串的数量](https://leetcode.cn/problems/unique-substrings-with-equal-digit-frequency/)



```JAVA
/* 
    执行用时： 383 ms , 在所有 Java 提交中击败了 64.71% 的用户
    内存消耗： 43.5 MB , 在所有 Java 提交中击败了 29.41% 的用户
    通过测试用例： 127 / 127
*/
class Solution {
    public int equalDigitFrequency(String s) {
        Set<String> set = new HashSet<>();
        char[] chs = s.toCharArray();

        for (int i = 0;i < chs.length;i ++) {
            int[] arr = new int[10];
            arr[chs[i] - '0'] ++;
            set.add(chs[i] + "");
            for (int j = i + 1;j < chs.length;j ++) {
                boolean sign = true;
                arr[chs[j] - '0'] ++;
                int max = 0;
                for (int val : arr) {
                    if (val == 0) {
                        continue;
                    }
                    if (max == 0) {
                        max = val;
                    }else {
                        if (max != val) {
                            sign = false;
                            break;
                        }
                    }
                }
                if (sign) {
                    set.add(s.substring(i,j + 1));
                }
            }
        }
        return set.size();
    }
}

```



## [1096. 花括号展开 II](https://leetcode.cn/problems/brace-expansion-ii/)



```JAVA
/* 
    执行用时： 8 ms , 在所有 Java 提交中击败了 80.95% 的用户
    内存消耗： 42.3 MB , 在所有 Java 提交中击败了 41.27% 的用户
    通过测试用例： 115 / 115
*/
class Solution {
    String expression;
    int idx;

    public List<String> braceExpansionII(String expression) {
        this.expression = expression;
        this.idx = 0;
        Set<String> ret = expr();
        return new ArrayList<String>(ret);
    }

    // item . letter | { expr }
    private Set<String> item() {
        Set<String> ret = new TreeSet<String>();
        if (expression.charAt(idx) == '{') {
            idx++;
            ret = expr();
        } else {
            StringBuilder sb = new StringBuilder();
            sb.append(expression.charAt(idx));
            ret.add(sb.toString());
        }
        idx++;
        return ret;
    }

    // term . item | item term
    private Set<String> term() {
        // 初始化空集合，与之后的求解结果求笛卡尔积
        Set<String> ret = new TreeSet<String>() {{
            add("");
        }};
        // item 的开头是 { 或小写字母，只有符合时才继续匹配
        while (idx < expression.length() && (expression.charAt(idx) == '{' || Character.isLetter(expression.charAt(idx)))) {
            Set<String> sub = item();
            Set<String> tmp = new TreeSet<String>();
            for (String left : ret) {
                for (String right : sub) {
                    tmp.add(left + right);
                }
            }
            ret = tmp;
        }
        return ret;
    }

    // expr . term | term, expr
    private Set<String> expr() {
        Set<String> ret = new TreeSet<String>();
        while (true) {
            // 与 term() 求解结果求并集
            ret.addAll(term());
            // 如果匹配到逗号则继续，否则结束匹配
            if (idx < expression.length() && expression.charAt(idx) == ',') {
                idx++;
                continue;
            } else {
                break;
            }
        }
        return ret;
    }
}
```



## [314. 二叉树的垂直遍历](https://leetcode.cn/problems/binary-tree-vertical-order-traversal/)



```JAVA
/* 
    执行用时： 2 ms , 在所有 Java 提交中击败了 85.43% 的用户
    内存消耗： 41.9 MB , 在所有 Java 提交中击败了 44.94% 的用户
    通过测试用例： 214 / 214
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
        public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> result = new LinkedList<>();
        if(root == null) return result;
        // 层序遍历的结点队列
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        // 结点对应的位置队列
        Queue<Integer> posQueue = new LinkedList<>();
        posQueue.offer(0);
        // key : position | value: node.val list
        HashMap<Integer, List<Integer>> map = new HashMap<>();
        // 最左侧的位置
        int minPos = Integer.MAX_VALUE;

        while(!queue.isEmpty()){
            TreeNode node = queue.poll();
            int pos = posQueue.poll();
            List<Integer> list = map.getOrDefault(pos, new LinkedList<>());
            list.add(node.val);
            map.put(pos, list);

            if(node.left != null){
                queue.offer(node.left);
                posQueue.offer(pos - 1);
            }
            if(node.right != null){
                queue.offer(node.right);
                posQueue.offer(pos + 1);
            }
            minPos = Math.min(minPos, pos);//维护最左侧位置
        }

        for(int i = minPos; i < minPos + map.size(); i++){
            result.add(map.get(i));
        }
        return result;
    }
}
```



## [剑指 Offer 47. 礼物的最大价值](https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/)



```JAVA
/* 
    执行用时： 4 ms , 在所有 Java 提交中击败了 3.89% 的用户
    内存消耗： 44.3 MB , 在所有 Java 提交中击败了 13.67% 的用户
    通过测试用例： 61 / 61
*/
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] f = new int[2][n];
        for (int i = 0; i < m; ++i) {
            int pos = i % 2;
            for (int j = 0; j < n; ++j) {
                f[pos][j] = 0;
                if (i > 0) {
                    f[pos][j] = Math.max(f[pos][j], f[1 - pos][j]);
                }
                if (j > 0) {
                    f[pos][j] = Math.max(f[pos][j], f[pos][j - 1]);
                }
                f[pos][j] += grid[i][j];
            }
        }
        return f[(m - 1) % 2][n - 1];
    }
}
```





## [1365. 有多少小于当前数字的数字](https://leetcode.cn/problems/how-many-numbers-are-smaller-than-the-current-number/)



```JAVA
/* 
    执行用时： 1 ms , 在所有 Java 提交中击败了 98.79% 的用户
    内存消耗： 41.8 MB , 在所有 Java 提交中击败了 37.55% 的用户
    通过测试用例： 103 / 103
*/
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] cnt = new int[101];
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            cnt[nums[i]]++;
        }
        for (int i = 1; i <= 100; i++) {
            cnt[i] += cnt[i - 1];
        }
        int[] ret = new int[n];
        for (int i = 0; i < n; i++) {
            ret[i] = nums[i] == 0 ? 0 : cnt[nums[i] - 1];
        }
        return ret;
    }
}
```





## [482. 密钥格式化](https://leetcode.cn/problems/license-key-formatting/)



```JAVA
/* 
    执行用时： 9 ms , 在所有 Java 提交中击败了 87.96% 的用户
    内存消耗： 41.2 MB , 在所有 Java 提交中击败了 88.09% 的用户
    通过测试用例： 38 / 38
*/
class Solution {
    public String licenseKeyFormatting(String s, int k) {
        StringBuilder ans = new StringBuilder();
        int cnt = 0;

        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) != '-') {
                cnt++;
                ans.append(Character.toUpperCase(s.charAt(i)));
                if (cnt % k == 0) {
                    ans.append("-");
                }
            }
        }
        if (ans.length() > 0 && ans.charAt(ans.length() - 1) == '-') {
            ans.deleteCharAt(ans.length() - 1);
        }
        
        return ans.reverse().toString();
    }
}
```



## [1617. 统计子树中城市之间最大距离](https://leetcode.cn/problems/count-subtrees-with-max-distance-between-cities/)



```JAVA
/* 
    执行用时： 25 ms , 在所有 Java 提交中击败了 61.54% 的用户
    内存消耗： 42.2 MB , 在所有 Java 提交中击败了 11.54% 的用户
    通过测试用例： 31 / 31
*/
class Solution {
    int mask;
    int diameter;

    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {
        List<Integer>[] adj = new List[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<Integer>();
        }
        for (int[] edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].add(y);
            adj[y].add(x);
        }

        int[] ans = new int[n - 1];
        for (int i = 1; i < (1 << n); i++) {
            int root = 32 - Integer.numberOfLeadingZeros(i) - 1;
            mask = i;
            diameter = 0;
            dfs(root, adj);
            if (mask == 0 && diameter > 0) {
                ans[diameter - 1]++;
            }
        }
        return ans;
    }

    public int dfs(int root, List<Integer>[] adj) {
        int first = 0, second = 0;
        mask &= ~(1 << root);
        for (int vertex : adj[root]) {
            if ((mask & (1 << vertex)) != 0) {
                mask &= ~(1 << vertex);
                int distance = 1 + dfs(vertex, adj);
                if (distance > first) {
                    second = first;
                    first = distance;
                } else if (distance > second) {
                    second = distance;
                }
            }
        }
        diameter = Math.max(diameter, first + second);
        return first;
    }
}

```



