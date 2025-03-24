<font color="#7f7f7f"><em>Friday, 01-11-2024 - 15:32</em></font>

## Index
- [[#Theoretical Fundaments]]
	- [[#Implementation]]
	- [[#Traversal Techniques - DFS (Depth-First Search)]]
	- [[#Traversal Techniques - BFS (Breath-First Search)]]
	

## Theoretical Fundaments
Binary Tree: Inside a node there can only exist at maximum two children. These are non-linear data structures.

Root: First node.
Children: The 1 or 2 nodes derived from the node above.
Leaf Node: Does not have children.
Sub-Tree: A "tree" inside of the original tree.
Ancestor: Reference to the node prior to the children.

Types of binary trees:

. 1. Fill Binary Tree
- Each node has 0 or 2 children.

### Implementation
```java
public class BinaryTree {

	public static class TreeNode {
		int val;
		TreeNode left, right;

		public TreeNode(int val){
			this.val = val;
			this.left = null;
			this.right = null;
		}
	}

	// Root of the binary tree
	private TreeNode root;

	public BinaryTree(){
		root = null;
	}

    // Insert a value into the tree
    public void insert(int val) {
        // TODO: implement insert logic
    }

    // Search for a value in the tree
    public boolean search(int val) {
        // TODO: implement search logic
        return false;
    }

    // Delete a value from the tree
    public void delete(int val) {
        // TODO: implement delete logic
    }
    
    // Level-order traversal (BFS)
    public void levelOrderTraversal() {
        // TODO: implement traversal logic
    }

    // Getter for the root (optional)
    public TreeNode getRoot() {
        return root;
    }

    // Setter for the root (optional)
    public void setRoot(TreeNode root) {
        this.root = root;
    }
}
```


### Traversal Techniques - DFS (Depth-First Search)

Note: left always comes before right
Go to the sub-tree apply the formula, go to the sub-tree apply the formula until reaching the end of the left side. 
##### In-order Traversal (Left Root Right)
Go to the extreme left -> left/root/right -> go to extreme right -> left/root/right

4 2 5 1 6 3 7 

in: in between (middle) 

```java
// In-order traversal (Left, Root, Right)
public void inorder(TreeNode node) {
	if (node == null){
		return ;
	}

	inorder(node.left);
	System.out.print(node.data + " -> ");
	inorder(node.right);
}    
```
##### Pre-order Traversal (Root Left Right)
Go to the extreme left -> root/left/right -> go to extreme right -> root/left/right

1 2 4 5 3 6 7 

pre: root first

```java
// Pre-order traversal (Root, Left, Right)
public void preorder(TreeNode node) {
	if (node == null){
		return ;
	}

	System.out.print(node.data + " -> ");
	preorder(node.left);
	preorder(node.right);
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

##### Post-order Traversal (Left Right Root)
Go to the extreme left -> left/right/root -> go to extreme right -> left/right/root

4 5 2 6 7 3 1

post: root after

```java
// Post-order traversal (Left, Right, Root)
public void postorder(TreeNode node) {
	if (node == null){
		return ;
	}

	postorder(node.left);
	postorder(node.right);
	System.out.println(node.data + " -> ");
}
```
<u>TIME COMPLEXITY</u>:  O(N)
<u>SPACE COMPLEXITY</u>: O(N)

### Traversal Techniques - BFS (Breath-First Search)

Kind of traverses through levels on at a time 

1 2 3 4 5 6 7 8 9 10


## Exercises