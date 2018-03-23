# [Binary Tree Level Order Traversal][title]

## Description

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

## 思路一

使用一个队列。先压入根节点和一个空白节点，然后依次弹出节点：
* 如果节点不为空，把当前值加到列表，然后把节点的左右孩子节点压入队列尾部（如果孩子节点不为空）
* 如果节点为空，并且队列不为空，则添加当前列表

## [完整代码][src]

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
  public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> list = new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);
    queue.offer(null);

    TreeNode node;
    List<Integer> level = new ArrayList<>();
    while ((node = queue.poll()) != null || queue.peek() != null) {
      if (node == null) {
        queue.offer(null);
        list.add(level);
        level = new ArrayList<>();
        continue;
      }
      level.add(node.val);
      if (node.left != null) {
        queue.offer(node.left);
      }
      if (node.right != null) {
        queue.offer(node.right);
      }
    }
    if (level.size() > 0) {
      list.add(level);
    }

    return list;
  }
}
```

## 思路二

在思路一的基础上进行优化。思路一通过往队列里添加空白节点作为每一层的分隔。
其实当前队列的长度就是该层节点的个数，通过 for 循环往队列里压入下一层节点。

## [完整代码][src2]

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
  public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> list = new ArrayList<>();
    if (root == null) return list;
    
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);

    while (!queue.isEmpty()) {
      List<Integer> temp = new ArrayList<>();
      int size = queue.size();
      for (int i = 0; i < size; i++) {
        TreeNode node = queue.peek();
        if (node.left != null) {
          queue.offer(node.left);
        }
        if (node.right != null) {
          queue.offer(node.right);
        }
        temp.add(queue.poll().val);
      }
      list.add(temp);
    }

    return list;
  }
}
```

[title]: https://leetcode.com/problems/binary-tree-level-order-traversal
[src]: https://github.com/andavid/leetcode-java/blob/master/src/com/andavid/leetcode/_102/Solution.java
[src2]: https://github.com/andavid/leetcode-java/blob/master/src/com/andavid/leetcode/_102/Solution2.java