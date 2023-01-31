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

