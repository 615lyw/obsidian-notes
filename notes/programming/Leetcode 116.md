# Leetcode 116

![[Pasted image 20220331170819.png]]

层次遍历解法：

在 [[二叉树的层次遍历框架]] 上进行改动，下面两种解法的区别在于代码实现放在什么位置。

解法一在 for 遍历每一层节点时进行 next 连接；
解法二在获得每层所有节点后，单独循环一遍进行连接处理。

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) return null;
        levelTraverse(root);
        return root;
    }

    private void levelTraverse(Node root) {
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        // 外层的 while 循环迭代的是层数
        while (!queue.isEmpty()) {
            // size 即每一层的节点数
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Node p = queue.poll();
                if (i < size - 1) {
                    // 最后一个节点不用设置 next
                    p.next = queue.peek();
                }
                if (p.left != null) {
                    queue.offer(p.left);
                }
                if (p.right != null) {
                    queue.offer(p.right);
                }
            }
        }
    }
}

class Solution {
    public Node connect(Node root) {
        if (root == null) return null;
        levelTraverse(root);
        return root;
    }

    private void levelTraverse(Node root) {
        LinkedList<Node> queue = new LinkedList<>();
        queue.offer(root);
        // 外层的 while 循环迭代的是层数
        while (!queue.isEmpty()) {
            // size 即每一层的节点数
            int size = queue.size();
            // 此时链表里装着同一层所有节点
            for (int i = 0; i < size - 1; i++) {
                Node n = queue.get(i);
                n.next = queue.get(i + 1);
            }

            // 下面的 for 中链表里装的节点会动态变化，会同时装有相邻两层的节点
            for (int i = 0; i < size; i++) {
                Node p = queue.poll();
                if (p.left != null) {
                    queue.offer(p.left);
                }
                if (p.right != null) {
                    queue.offer(p.right);
                }
            }
        }
    }
}
```

遍历解法：

前序遍历从上往下操作，对下一层进行更新。针对每个节点对子节点操作跨几层时需要看具体规律。[[Leetcode 101]]

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) return null;
        if (root.left != null) {
            root.left.next = root.right;
            // 本层 next 已由上层设置好
            if (root.next != null) {
                // 设置下一层的 next
                root.right.next = root.next.left;
            }
        }
        connect(root.left);
        connect(root.right);
        return root;
    }
}
```

## 参考

- [116. 填充每个节点的下一个右侧节点指针 - 力扣（LeetCode）](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)


