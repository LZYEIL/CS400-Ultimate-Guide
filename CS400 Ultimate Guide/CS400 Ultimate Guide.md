# CS400: Ultimate Guide

<br>

## Preface

- This guide serves to provide students with a good understanding of the materials discussed within the course **CS400: Programming III** at the *University of Wisconsin-Madison*. This being said, this book will lay a solid foundation of the knowledge you'll learn in the CS400, and ideally, you should be able to ace both the midterm and final exam by simply reading this guide. **However, this is not a encouragement to skip classes.** 

  <br>

- Students should be comfortable with the concepts discussed in the previous course (CS300) to read this book. However, this is not a strict requirement, but you do need to have basic Java programming skills and be familiar with linear data structures. If you need a refresh on the materials from CS300, feel free to check my [CS300-Final-Revision-Notes](https://lzyeil.github.io/CS300-Final-Exam-Review/) *(Note that I only covered the later sections of the course; you need to at least be comfortable with the most basic Java programming concepts)*

  <br>
  
- This guide will categorize the concepts into chapters, and each chapter will be divided into **1. Data Structures and 2. Practical Software Development Techniques.**

  <br>



## Chapter 1

<br>

### Binary Search Tree

In a **Binary Search Tree (BST)**, each node stores some information called the **key value**. A binary tree is a BST iff, for every node i:

- All keys in i's left subtree are less than the key in i
- All keys in i's right subtree are greater than the key in i
- *Note: we assume that duplicates are not allowed*

<br>

Here are some examples of a valid BST:

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Example.png" alt="BST-Example" style="zoom: 50%;" />

<br>

The **height** of a tree is **the number of nodes** from the root to the tree's deepest leaf. In the example above, both trees have a height of 3. (*Be aware that exam questions may refer to the **number of edges**. In this case, both trees above will have a height of 2 instead*)  Also, if we use the *in-order traversal* technique, the values will be visited in **the sorted order!** 

<br>

Now, let's take a look at the general operations a BST can do:

- Search (Look-Up)
- Insert
- Delete

<br>

**Visual Examples:**

**1. Searching:**

 <img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Search1.png" style="zoom: 35%;" /> <img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Search2.png" style="zoom: 35%;" /> 

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Search3.png" style="zoom:40%;" /> <img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Search4.png" style="zoom:40%;" />

<br>

**2. Inserting:**

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Insert1.png" style="zoom:33%;" /> <img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Insert2.png" style="zoom:33%;" /> 

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Insert3.png" style="zoom:33%;" /> <img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Insert4.png" style="zoom:33%;" /> 

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Insert5.png" style="zoom:33%;" /> 

<br>

**3. Deleting:**

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Delete1.png" style="zoom:38%;" /> <img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Delete2.png" style="zoom:38%;" />  

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Delete3.png" style="zoom:38%;" /> 

<br>

<br>

**Code Implementation:**

Two classes needed: *BinarySearchTreeNode* and the *BinarySearchTree*:

```java
/**
 * Generic class implementing a Binary Node of a Binary Search Tree (BST)
 *
 * @param <T> type of the data carried by this binary node
 */
public class BSTNode<T extends Comparable<T>> {


  private T data;
  private BSTNode<T> left;
  private BSTNode<T> right;

  /**
   * Creates a BSTNode with a given data value
   * 
   * @param data data carried by this binary node
   */
  public BSTNode(T data) {
    this.data = data;
  }

  /**
   * Creates a BSTNode with given data value, a reference to a left child (left subtree) and a
   * reference to a right child (right sub-tree)
   * 
   * @param data  element held by this binary node
   * @param left  reference to the left child
   * @param right reference to the right child
   */
  public BSTNode(T data, BSTNode<T> left, BSTNode<T> right) {
    this.data = data;
    this.left = left;
    this.right = right;
  }

  /**
   * Getter of left child
   * 
   * @return the left
   */
  public BSTNode<T> getLeft() {
    return left;
  }

  /**
   * Setter for left child
   * 
   * @param left the left to set
   */
  public void setLeft(BSTNode<T> left) {
    this.left = left;
  }

  /**
   * Getter of the right child
   * 
   * @return the right child of this binary node
   */
  public BSTNode<T> getRight() {
    return right;
  }

  /**
   * Setter for the right child
   * 
   * @param right the right to set
   */
  public void setRight(BSTNode<T> right) {
    this.right = right;
  }

  /**
   * Getter of data field
   * 
   * @return the data held by this node
   */
  public T getData() {
    return data;
  }
```

<br>

```java
/**
 * This is the Binary Search Tree class
 * 
 * @param <T> Represents a generic type
 */
public class BinarySearchTree<T extends Comparable<T>> {
  
    
  //The only field in this class
  //Root is used for the access of the whole tree
  private BSTNode<T> root;
  private int size;

    

 /**
 * Looks up a given value in the binary search tree.
 * 
 * @param value the value to search for
 * @return true if the value is found, false otherwise
 */
public boolean lookup(T value) {
    return lookupRecursive(root, value);
}

 
/**
 * Recursive helper method to search for a value in the BST.
 * 
 * @param node  the current node being examined
 * @param value the value to search for
 * @return true if the value is found, false otherwise
 */
private boolean lookupRecursive(BSTNode<T> node, T value) {
    if (node == null) {
        return false; // Value not found
    }
    
    int cmp = value.compareTo(node.getData());
    
    if (cmp == 0) {
        return true; // Value found
    } else if (cmp < 0) {
        return lookupRecursive(node.getLeft(), value); // Search left subtree
    } else {
        return lookupRecursive(node.getRight(), value); // Search right subtree
    }
}
  
///////////////////////////////////////////////////////////////////////////
    
  /**
 * Inserts a new value into the binary search tree.
 * 
 * @param value the value to insert
 * @return true if the value was successfully inserted, false if it already exists
 */
public boolean insert(T value) {
    if (root == null) {
        root = new BSTNode<>(value);
        size++;
        return true;
    }
    return insertRecursive(root, value);
}

    
/**
 * Recursive helper method to insert a value into the BST.
 * 
 * @param node  the current node being examined
 * @param value the value to insert
 * @return true if the value was successfully inserted, false if it already exists
 */
private boolean insertRecursive(BSTNode<T> node, T value) {
    int cmp = value.compareTo(node.getData());
    
    if (cmp == 0) {
        return false; // Value already exists
        
    } else if (cmp < 0) {  //Left Tree branch
        if (node.getLeft() == null) {
            node.setLeft(new BSTNode<>(value));
            return true;
        } else {
            return insertRecursive(node.getLeft(), value);
        }
        
    } else {  //Right tree branch
        if (node.getRight() == null) {
            node.setRight(new BSTNode<>(value));
            return true;
        } else {
            return insertRecursive(node.getRight(), value);
        }
    }
}  
  
///////////////////////////////////////////////////////////////////////////
    
/**
 * Removes a value from the binary search tree.
 * 
 * @param value the value to remove
 * @return true if the value was successfully removed, false if it was not found
 */
public boolean remove(T value) {
    if (root == null) {
        return false;
    }
    try {
        root = removeRecursive(root, value);
        size--;
        return true;
    } catch (NoSuchElementException e) {
        return false;
    }
}

    
/**
 * Recursive helper method to remove a value from the BST.
 * 
 * @param node  the current node being examined
 * @param value the value to remove
 * @return the new root of this subtree after removal
 * @throws NoSuchElementException if the value is not found
 */
private BSTNode<T> removeRecursive(BSTNode<T> node, T value) {
    if (node == null) {
        throw new NoSuchElementException("Value not found in the tree.");
    }
    
    int cmp = value.compareTo(node.getData());
    
    if (cmp < 0) {
        node.setLeft(removeRecursive(node.getLeft(), value));
    } else if (cmp > 0) {
        node.setRight(removeRecursive(node.getRight(), value));
    } else {
        // Case 1: No children
        if (node.getLeft() == null && node.getRight() == null) {
            return null;
        }
        // Case 2: One child
        else if (node.getLeft() == null) {
            return node.getRight();
        } else if (node.getRight() == null) {
            return node.getLeft();
        }
        // Case 3: Two children
        else {
            // Find the minimum value in the right subtree
            BSTNode<T> minNode = node.getRight();
            while (minNode.getLeft() != null) {
                minNode = minNode.getLeft();
            }
            // Replace the current node's data with the minimum value
            node.setData(minNode.getData());
            // Remove the node with the minimum value from the right subtree
            node.setRight(removeRecursive(node.getRight(), minNode.getData()));
        }
    }
    return node;
}    
```

<br>

Phew! That's a lot of codes up there, but now let's analyze the complexity of each operation! We will use *N* as the number of nodes in the BST and *H* as the height of a BST. The relationship between *H* and *N* is: 

```math
N = 2^H - 1
```

(Try to enumerate the combinations between *H* and *N* from different trees to prove it yourself!)

And for all **Searching, Inserting and Deleting, ** all their complexities are **O(logN)/O(H)** due to binary search! But also keep in mind that for an *unbalanced BST (The worst case is when the height is the total number of nodes, like a linked-list. But generally speaking, if the left subtree and right subtree has a significant difference in height, it is considered unbalanced)*, the complexities will degrade to *O(N)*. 

<br>

**Practices:**



<br>

### BST Rotations

It is a fundamental operation used to **rebalance** a BST while preserving the BST property. The goal of rotation is to **reduce the height of the tree** and maintain balance, ensuring that common operations  remain efficient time complexity.

<br>

**Visual Example:**

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Rotation1.png" style="zoom:50%;" />

Both trees have exactly 3 nodes. However, the left tree has a height of 3, while the right one has a height of 2. In this case, the left tree is what we call *unbalanced*. For example, we would like to access the value 10. For the left tree, we need to go through 50 -> 30 -> 10 *(3 operations)*. For the right tree, we go through 30 -> 10 *(2 operations)*. You may think there's not much difference between a 2 and a 3. However, as the dataset becomes really large, it is significantly different. 

<br>

There are two types of rotation: **Left Rotation and Right Rotation:** 

- A **left rotation** is performed when the **right subtree** of a node is **heavier** (taller) than the left subtree. It moves the right child up and the current node down to the left.
- A **right rotation** is performed when the **left subtree** of a node is **heavier** (taller) than the right subtree. It moves the left child up and the current node down to the right.

<br>

**Visual Examples:**

<img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Rotation2.png" style="zoom:33%;" /> <img src="C:\Users\d's'y\Desktop\CS400 Ultimate Guide\BST_Rotation3.jpg" alt="BST_Rotation3" style="zoom:33%;" /> 

In the above example, *GP* represents grandparent, *P* is the parent and *C* is the child. One way you can think of this as the child squeezes the parent to the either left or right and the child is now at the previous parent's location. Notice that the node *b* in the left rotation and the node *f* in the right rotation change their parent. In the left rotation, since *b* was **greater than *P*** but **smaller than *C*** before rotation, thus b now becomes the right child of *P*. Same logic applies to *f* in the right rotation, just in another direction. 

<br>





