## 二叉树的下一个节点

### 题目描述

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

其中树的结构代码如下：

```java
public class TreeLinkNode {
   int val;
   TreeLinkNode left = null;
   TreeLinkNode right = null;
   TreeLinkNode next = null; //指向父节点

   TreeLinkNode(int val) {
       this.val = val;
   }
}
```

### 解题思路

首先，注明了是中序遍历的下一个节点，那么需要先研究中序遍历树的特征。比如如下二叉树：

![](https://blogimage-1258928558.cos.ap-guangzhou.myqcloud.com/offer/tree-example.png)

显然中序遍历结果为：`9 5 8 2 1 3 6 7 3 4 10`

 那么将凌乱的思路整理一下，其实我们再想一想，其实我们只需要抓住一个关键点进行突破就可以了，那就是**给定的节点是否有右子树**，分为两种情况：

1. 若给定的节点有右子树，如上图的节点5，那么显然5的下一个节点的值应该是8，也就是其右子树的最后一层的左子节点的值。这个实现可以使用递归获取值即可，那么这个问题就轻而易举的解决了。
2. 另外一种情况就是给定的节点没有右子树，这个情况就比较复杂了，也很难想到（所以为啥要刷算法题，毕竟算法又不是你发明的...解决问题的思路还是得学一学的）。最简单的情况就是上面的左下角的9，显然9是没有右子树的，那么它的下一个节点实际上就是它的父节点5；最复杂的情况，就是2的右子节点1，它也没有右子树，但是它的下一个节点应该是根节点3。

所以针对上面的第二种情况，最终需要将这两种简单和复杂情况合并，那么有没有这种方法呢？

那么当然是可以有的，由于题目特别说明这个树的节点是包含指向父节点的指针的，那么根据这个特性，我们可以再思考一下：

- 实际上只要循环找到当前节点的父节点的左子节点等于当前节点，那么这个问题就被KO了

局部代码如下：

```java
while (pNode.next != null) {
     TreeLinkNode parent = pNode.next;
     if (parent.left == pNode) {
         return parent;
     }
     pNode = pNode.next;
}
```

其中`pNode`是给定的节点，我们最终是要找`pNode`的下一个节点。

根据上述代码，对于节点9，上面显然可以得其下一个节点是5，结果正确。

对于节点1，根据上面循环往上找，最终确实找到了其下一个节点是3，结果正确。

### 实现代码

综合上述思路，最终可以整理代码如下：

```java
public TreeLinkNode GetNext(TreeLinkNode pNode) {
   //如果有右子树，则找右子树的最左节点
   if (pNode.right != null) {
       TreeLinkNode node = pNode.right;
        while (node.left != null) {
             node = node.left;
        }
        return node;
   } else {
       //没有右子树，向上找第一个左链接指向的树包含该节点的祖先节点
       while (pNode.next != null) {
          TreeLinkNode parent = pNode.next;
          if (parent.left == pNode) {
              return parent;
          }
          pNode = pNode.next;
         }
     }
     return null;
}
```

## 对称二叉树

### 题目描述

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

### 解题思路

这个题其实思路上难度不是很大，只要画个图，就可以看明白一半。

![](https://blogimage-1258928558.cos.ap-guangzhou.myqcloud.com/offer/tree-sym.png)

如上图就属于题目描述的对称二叉树的一种，那么显然，只需要我们确定一个根节点下的左子节点的值和右子节点的值是否相同就可以，只要有一个不同，那么就不是对称二叉树。具体过程可以使用递归来完成。

### 实现代码

```java
public boolean isSymmetrical(TreeNode pRoot) {
    if(pRoot == null){
        return true;
    }
     return isSym(pRoot,pRoot);
}

//判断是否相等
private boolean isSym(TreeNode root1,TreeNode root2){
    //如果递归中root1为null，root2不为null，那么说明不对称
    //如果root1=root2=null，那么说明递归完成后没有返回false说明完全对称，即返回true
   if(root1 == null){
      return root2 == null;
   }
    //由于上面先判断了root1==null，所以如果进入了这一步，说明root1 != null
    //但是root2先到null了，这说明存在不对称的部分，直接返回false
   if(root2 == null){
      return false;
   }
    //判断值是否相同
   if(root1.val != root2.val){
      return false;
   }
    //递归调用
   return isSym(root1.left,root2.right) && isSym(root1.right,root2.left);
}
```

## 把二叉树打印成多行

### 题目描述

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

### 解题思路

这个题本质上就是层序遍历，不过一般我们只使用了前序遍历、中序遍历、后序遍历，对于层序遍历，涉及的不是很多。并且这个题还需要将输出结果转换成`ArrayList`存储的形式，即每行的结果都是一个`ArrayList`，这是这个题目稍微需要拐弯的地方。

那么对于树，其实绝大多数情况下都可以使用递归来解决问题。这个题需要解决的问题有：

1. 先确定这颗二叉树的深度，即有多少行，这样才可以创建`ArrayList`的个数
2. 剩下的添加节点完全可以使用递归实现

### 代码实现

基于上述思路，可以以下面代码实现：

```java
ArrayList<ArrayList<Integer>> Print(TreeNode pRoot) {
    ArrayList<ArrayList<Integer>> list = new ArrayList<>();
    depth(pRoot, 1, list);
    return list;
}

private void depth(TreeNode root, int depth, ArrayList<ArrayList<Integer>> list) {
   if (root == null) return;
   if (depth > list.size()){
       list.add(new ArrayList<>());
   }
   list.get(depth - 1).add(root.val);

   depth(root.left, depth + 1, list);  
   depth(root.right, depth + 1, list);
}
```

## 二叉搜索树的第k个节点

### 题目描述

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）中，按结点数值大小顺序第三小结点的值为4。

### 解题思路

对于一颗二叉搜索树而言，其实际结构就是这样：

```java
             5
            / \
           3   7
          / \ / \
         2  4 6  8
```

这种情况下有一个非常好的特点，那就是二叉搜索树的中序遍历的结果恰好是按照大小顺序排列的。

### 实现代码

基于上述结论，实际上可以立马写出如下代码：

```java
//思路：中序遍历二叉搜索树
TreeNode KthNode(TreeNode pRoot, int k) {
   if(pRoot == null) return null;

   ArrayList<TreeNode> arr = new ArrayList<>();
   midSeek(pRoot,arr);
    
   //获取节点的个数，并与k比较
   if(k <= 0 || k > arr.size()){
       return null;
   }

   return arr.get(k-1);
}


//中序遍历
private void midSeek(TreeNode pRoot,ArrayList<TreeNode> arr){
     if(pRoot.left != null){
        midSeek(pRoot.left,arr);
     }
    
     arr.add(pRoot);
    
     if(pRoot.right != null){
        midSeek(pRoot.right,arr);
     }
}
```

但是上面代码实际上有一些冗余，并不是最优解，因为是先将中序遍历结果全部找出来，再挑第`k`个值。那么按照老套路，是不是可以想个办法，在中序遍历的时候就直接一次性找出第`k`个值呢？答案是显然可以的。

优化后的代码如下：

```java
public class Solution {
   int index = 0; //计数器
    TreeNode KthNode(TreeNode root, int k)
    {
        if(root != null){ //中序遍历寻找第k个
            TreeNode node = KthNode(root.left,k);
            if(node != null)
                return node;
            index ++;
            if(index == k)
                return root;
            node = KthNode(root.right,k);
            if(node != null)
                return node;
        }
        return null;
    }
}
```


