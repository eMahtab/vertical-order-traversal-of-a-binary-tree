# Vertical Order Traversal of a binary tree
## https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree

Given a binary tree, return the vertical order traversal of its nodes values.

For each node at position (X, Y), its left and right children respectively will be at positions (X-1, Y-1) and (X+1, Y-1).

Running a vertical line from X = -infinity to X = +infinity, whenever the vertical line touches some nodes, we report the values of the nodes in order from top to bottom (decreasing Y coordinates).

**Important 😉:** If two nodes have the same position, then the value of the node that is reported first is the value that is smaller.

Return an list of non-empty reports in order of X coordinate.  Every report will have a list of values of nodes.


![Binary Tree Vertical Order Traversal](vertical-order-traversal-ii.JPG?raw=true "Binary Tree Vertical Order Traversal")

## Implementation :
```java
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
    class Node {
        TreeNode treeNode;
        int horizontalDistance, verticalDistance;
        Node(TreeNode treeNode, int horizontalDistance, int verticalDistance) {
            this.treeNode = treeNode;
            this.horizontalDistance = horizontalDistance;
            this.verticalDistance = verticalDistance;
        }
    }
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if(root == null)
            return res;
        Map<Integer, List<Node>> map = new HashMap<>();
        Queue<Node> q = new ArrayDeque<>();
        q.add(new Node(root, 0, 0));
        int minHd = Integer.MAX_VALUE, maxHd = Integer.MIN_VALUE;
        while(!q.isEmpty()) {
            Node node = q.poll();
            map.putIfAbsent(node.horizontalDistance, new ArrayList<Node>());
            map.get(node.horizontalDistance).add(node);
            minHd = Math.min(minHd, node.horizontalDistance);
            maxHd = Math.max(maxHd, node.horizontalDistance);
            if(node.treeNode.left != null)
                q.add(new Node(node.treeNode.left, node.horizontalDistance - 1, node.verticalDistance - 1));
            if(node.treeNode.right != null)
                q.add(new Node(node.treeNode.right, node.horizontalDistance + 1, node.verticalDistance - 1));
        }
        
        for(int i = minHd; i <= maxHd; i++) {
            Collections.sort(map.get(i), (n1, n2) -> {
                if(n1.verticalDistance == n2.verticalDistance)
                    return n1.treeNode.val - n2.treeNode.val;
                else
                    return n2.verticalDistance - n1.verticalDistance;
            });
            List<Integer> l = new ArrayList<>();    
            for(Node node : map.get(i)) 
                l.add(node.treeNode.val);
            res.add(l);    
        }
        return res;
    }
}
```


# References :
https://www.youtube.com/watch?v=QWbVCqIhTO4
