[TOC]

## 1. 前言

树的存储结构如下：

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## 2. 二叉树遍历

### 2.1 前序遍历（`preOrder`）

前序遍历，即先访问根节点，然后遍历左子树，最后遍历右子树。

![前序遍历](https://img-blog.csdnimg.cn/20200701164516998.gif)

-   **递归实现**

```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();
	if(root == null){
        return list;
    }
    
    preOrder(root, list);
    return list;
}

public void preOrder(TreeNode root, List<Integer> list){
    if(root != null){
        list.add(root.val);
        preOrder(root.left, list);
        preOrder(root.right, list);
    }
}
```



### 2.2 中序遍历（`inOrder`）

中序遍历，即先遍历左子树，然后访问根节点，最后遍历右子树。

![中序遍历](https://img-blog.csdnimg.cn/20200701165235942.gif)

-   **递归实现**

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    inOrder(root, list);
    return list;
}

public void inOrder(TreeNode root, List<Integer> list){
    if(root != null){
        inOrder(root.left, list);
        list.add(root.val);
        inOrder(root.right, list);
    }
}
```



### 2.3 后序遍历（`postOrder`）

后序遍历，即先遍历左子树，然后遍历右子树，最后访问根节点。

![后序遍历](https://img-blog.csdnimg.cn/20200701165756612.gif)

-   **递归实现**

```java
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();

    postOrder(root, list);
    return list;
}

public void postOrder(TreeNode root, List<Integer> list){
    if(root != null){
        // 左子树
        postOrder(root.left, list);
        // 右子树
        postOrder(root.right, list);
        // 根节点
        list.add(root.val);
    }
}
```

### 2.4 层序遍历（levelOrder）

层序遍历，即从根节点开始，一层一层的遍历。

![层序遍历](https://img-blog.csdnimg.cn/20200703144627913.gif#pic_center)

-   **递归实现**

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> lists = new ArrayList<List<Integer>>();
    // 根节点为 null，直接返回
    if(root == null){            
        return lists;
    }

    // 根节点不为 null，递归
    dfs(1, root, lists);

    return lists;
}

// index : 层数
public void dfs(int index, TreeNode root, List<List<Integer>> lists){

    // 若 lists 中序列数小于层数，则将 lists 中加入一个空的序列
    if(lists.size() < index){
        lists.add(new ArrayList<Integer>());
    }

    // 然后将当前节点加入 lists 的子序列中
    lists.get(index - 1).add(root.val);

    // 以上就处理完 root 节点
    // 接着处理左右子树即可，处理时，层数到下一次，所以要 +1

    if(root.left != null){
        dfs(index + 1, root.left, lists);
    }

    if(root.right != null){
        dfs(index + 1, root.right, lists);
    }
}
```

## 3. 二叉树深度

![](https://gitee.com/cunyu1943/images/raw/master/ImgsUbuntu/20200510234310.png)

---
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/css/share.min.css">
<center><div class="social-share"></div></center>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/social-share.js/1.0.16/js/social-share.min.js"></script>