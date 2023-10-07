## Common Terms

- Neighbor - parent or a child of a node
- Ancestor - a node reachable by traversing its parent chain
- Descendant - node in the node's subtree
- Degree - number of a children of a node
- Degree of a tree - maximum degree of nodes in the tree
- Distance - number of edges along the shortest path between nodes
- Level/Depth - number of edges along the unique path between a node and the root node
- Width - number of nodes in a level

## Binary Tree

### Terms

- Complete Binary Tree - binary tree in which every level, except possibly the last, is completely filled, and all the nodes in the last level are as far left as possible
- Balanced Binary Tree - binary tree structure in which left and right subtrees of every node differ in height by no more than 1

### Traversals

- In-order traversal - Left -> Root -> Right
- Pre-order traversal - Root -> Left -> Right
- Post-order traversal - Left -> Right -> Root

## Binary Search Tree

In-order traversal of a BST will give you all elements in order.

### Time Complexity

| Operation | Big-O     |
| --------- | --------- |
| Access    | O(log(n)) |
| Search    | O(log(n)) |
| Insert    | O(log(n)) |
| Remove    | O(log(n)) |

### Space Complexity

O(h), where he is height of the tree. While traversing very skewed trees (which is essentially a linked list) will be O(n).

## Things to Consider

- Be familiar with both recursive and iterative solutions for traversals
- Empty tree
- Single node
- Two nodes
- Very skewed tree

## Common Routines

- insert value
- delete value
- count number of nodes in tree
- whether a value is in the tree
- calculate height of the tree
- BST
   - determine if it is a BST
   - get maximum value
   - get minimum value

## Techniques

- recursion
- traversing by level (breadth-first search)
- summation of nodes
