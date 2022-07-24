---
title: "可预见的裁员前刷穿LC/1"
date: 2022-07-17T03:12:49+08:00
draft: false
---

从tag=树开始，为面试做准备。只求多快好省。

<!--more-->

# 题目描述

[99. 恢复二叉搜索树](https://leetcode.cn/problems/recover-binary-search-tree/) 难度：中等

给你二叉搜索树的根节点 `root` ，该树中的 **恰好** 两个节点的值被错误地交换。*请在不改变其结构的情况下，恢复这棵树* 。

示例 1：

![image-20220717033345665](https://myimagee.oss-cn-beijing.aliyuncs.com/img/image-20220717033345665.png)


```
输入：root = [1,3,null,null,2]
输出：[3,1,null,null,2]
解释：3 不能是 1 的左孩子，因为 3 > 1 。交换 1 和 3 使二叉搜索树有效。
```

示例 2：

![image-20220717033738715](https://myimagee.oss-cn-beijing.aliyuncs.com/img/image-20220717033738715.png)

```
输入：root = [3,1,4,null,null,2]
输出：[2,1,4,null,null,3]
解释：2 不能在 3 的右子树中，因为 2 < 3 。交换 2 和 3 使二叉搜索树有效。
```

提示：
- 树上节点的数目在范围 [2, 1000] 内
- -231 <= Node.val <= 231 - 1

---

# 中序遍历

二叉搜索树➡中序遍历。

[1,6,3,4,5,2,7]，显然序列中有两个位置不满足，在这个序列中体现为`6 > 3`，`5 > 2`，因此只要找到这两个位置，即可找到被错误交换的两个节点。考虑需要被交换的两个位置，由题意，显然是后面某个`较大数`与前面某个`较小数`发生了交换，于是定位到`x = 6`,`y = 2`。

```java
class Solution {
    public void recoverTree(TreeNode root) {
        TreeNode pre = null;
        TreeNode x = null;
        TreeNode y = null;
        TreeNode res = root;
        Deque<TreeNode> stack = new LinkedList<>();
        while(!stack.isEmpty() || root != null){
            if(root != null){
                stack.push(root);
                root = root.left;
            }else {
                root = stack.pop();
                if(pre == null){
                    pre = root;
                }
                if(pre.val > root.val){
                    if(x == null){
                        x = pre;
                    }
                    y = root;
                }
                pre = root;
                root = root.right;
            }
        }
        int tmp = x.val;
        x.val = y.val;
        y.val = tmp;
    }
}
```

---

# 题目描述

[98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/) 难度：中等

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

# 中序遍历

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        TreeNode pre = null;
        Deque<TreeNode> stack = new LinkedList<>();
        while(!stack.isEmpty() || root != null){
            if (root != null) {
                stack.push(root);
                root = root.left;
            }else {
                root = stack.pop();
                if (pre == null) {
                    pre = root;
                }
                if (pre != root && pre.val >= root.val){
                    return false;
                }
                pre = root;
                root = root.right;
            }
        }
        return true;
    }
}
```

---

# 题目描述

[100. 相同的树](https://leetcode.cn/problems/same-tree/) 难度：简单

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

```
输入：p = [1,2,3], q = [1,2,3]
输出：true
```

**提示：**

- 两棵树上的节点数目都在范围 `[0, 100]` 内
- `-104 <= Node.val <= 104`

# 递归判断

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p  == null && q == null) return true;
        if(p == null || q == null) return false;
        if(p.val == q.val) return isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
        return false;
    }
}
```

---

# 题目描述

[101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/) 难度：简单

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

![image-20220717115638798](https://myimagee.oss-cn-beijing.aliyuncs.com/img/image-20220717115638798.png)

# 递归判断

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return helper(root.left , root.right) && helper(root.right, root.left);
    }
    public boolean helper(TreeNode p , TreeNode q){
        if(p == null && q == null) return true;
        if(p == null || q == null) return false;
        if(p.val == q.val) return helper(p.left,q.right) && helper(p.right,q.left);
        return false;
    }
}
```

---

# 题目描述

[104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) 难度：简单

给定一个二叉树，找出其最大深度。二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

**示例：**

给定二叉树 `[3,9,20,null,null,15,7]`，

# 递归判断

```java
class Solution {
    int res = 0;
    public int maxDepth(TreeNode root) {
        dfs(root , 0);
        return res;
    }
    public void dfs(TreeNode root, int deep){
        if (root == null) return;
        deep = deep + 1;
        if (root.left == null && root.right == null){
            res = Math.max(res, deep);
            return;
        }
        dfs(root.left , deep);
        dfs(root.right , deep);
    }
}
```

---

# 题目描述

[814. 二叉树剪枝](https://leetcode.cn/problems/binary-tree-pruning/) 难度：中等

给你二叉树的根结点 root ，此外树的每个结点的值要么是 0 ，要么是 1 。返回移除了所有不包含 1 的子树的原二叉树。节点 node 的子树为 node 本身加上所有 node 的后代。

![image-20220721233518932](https://myimagee.oss-cn-beijing.aliyuncs.com/img/image-20220721233518932.png)

```
输入：root = [1,null,0,0,1]
输出：[1,null,0,null,1]
解释：
只有红色节点满足条件“所有不包含 1 的子树”。 右图为返回的答案。
```

```java
//后序遍历
class Solution{
    public TreeNode pruneTree(TreeNode root){
        if(root == null) return null;
        pruneTree(root.left);
        pruneTree(root.right);
        if(root.left==null && root.right == null && root.val == 0) return null;
        return root;
    }
}
```

---

# 题目描述

[103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/) 难度：中等

给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

![image-20220717125846263](https://myimagee.oss-cn-beijing.aliyuncs.com/img/image-20220717125846263.png)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]

输入：root = [1]
输出：[[1]]

输入：root = []
输出：[]
```

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-100 <= Node.val <= 100`

# 层序遍历

Deque双端队列的应用。

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if (root == null) return res;
        Deque<TreeNode> deque = new LinkedList<>();
        deque.add(root);
        boolean flag = false;
        while(!deque.isEmpty()){
            int size = deque.size();
            List<Integer> rtmp = new LinkedList<>();
            for(int i = 0 ; i < size ; i++){
                if(flag == false){
                    TreeNode tn = deque.removeFirst();
                    rtmp.add(tn.val);
                    if(tn.left != null){
                        deque.addLast(tn.left);
                    }
                    if(tn.right != null){
                        deque.addLast(tn.right);
                    }
                }else {
                    TreeNode tn = deque.removeLast();
                    rtmp.add(tn.val);
                    if(tn.right != null){
                        deque.addFirst(tn.right);
                    }
                    if(tn.left != null){
                        deque.addFirst(tn.left);
                    }
                }
            }
            flag = !flag;
            res.add(rtmp);
        }
        return res;
    }
}
```

---

# [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

难度：中等

给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。

![image-20220722004022114](https://myimagee.oss-cn-beijing.aliyuncs.com/img/image-20220722004022114.png)·

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

```java
class Solution {
    int rootIdx = 0;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
		HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0 ; i < preorder.length ; i++){
            map.put(inorder[i] , i);
        }
        
    }
    public TreeNode build (HashMap<Integer , Integer> map , int left , int right , int[]preorder){
        if(left > right) return null;
        int rootVal = preorder[rootIdx];
        TreeNode root = new TreeNode(rootVal);
        rootIdx--;
        root.left = build(map , left , map[rootVal] -1, preorder);
        root.right = build(map, map[rootVal] + 1 , right , preorder);
        return root;
    }
}
```



---



# [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

难度：中等

给定两个整数数组 inorder 和 postorder ，其中 inorder 是二叉树的中序遍历， postorder 是同一棵树的后序遍历，请你构造并返回这颗 二叉树 。

![image-20220721235755482](https://myimagee.oss-cn-beijing.aliyuncs.com/img/image-20220721235755482.png)

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

```java
class Solution {
    int rootIdx = 0;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        HashMap<Integer , Integer> map = new HashMap<>();
        for(int i =0 ; i < inorder.length ; i++){
            map.put(inorder[i] , i);
        }
        rootIdx = postorder.length - 1;
        TreeNode root = build(map, 0 , inorder.length-1 , postorder);
        return root;
    }
    public TreeNode build(HashMap<Integer,Integer>map ,int left , int right , int[] postorder){
        if(left > right) return null;
        int rootVal = postorder[rootIdx];
        TreeNode root = new TreeNode(rootVal);
        rootIdx--;
        root.right = build(map, map.get(rootVal) + 1, right ,postorder);
        root.left = build(map , left ,map.get(rootVal) - 1 , postorder);
        return root; 
    }
}
```



---







