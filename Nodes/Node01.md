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







