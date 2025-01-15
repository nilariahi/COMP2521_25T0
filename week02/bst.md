##### Binary Search Trees

For the questions below, use the following data type:

```c
struct node {
    int value;
    struct node *left;
    struct node *right;
};
```

1.  For each of the sequences below

    -   start from an initially empty binary search tree
    -   show the tree resulting from inserting values in the order given

    1.  4 6 5 2 1 3 7

        ```
             4
            / \
           /   \
          2     6
         / \   / \
        1   3 5   7
        ```
    2.  5 2 3 4 6 1 7

        ```
             5
            / \
           /   \ 
          2     6
         / \     \
        1   3     7
             \
              4
        ```

    3.  7 6 5 4 3 2 1

        ```
                    7
                   /
                  6
                 /
                5
               /
              4
             /
            3 
           /
          2
         /
        1
        ```


    Assume new values are always inserted as new leaf nodes.

2.  Consider the following tree and its values displayed in different output orderings:

    ![img](https://cgi.cse.unsw.edu.au/~cs2521/24T2/tut/4/bst-traversals/bst-traversals.svg)

    What kind of trees have the property that their in-order traversal is the same as their pre-order traversal?

    -   Right-deep trees that don't have any non-empty left subtrees.

    Are there any kinds of trees for which all output orders will be the same?

    -   Empty tree or just the root.

3.  Implement the following function that counts the number of odd values in a tree. Use the following function interface:

    ```c
    int bstCountOdds(struct node *t) {
        if (!t) return 0;
        
        return (t->value % 2) + bstCountOdds(t->left) 
            + bstCountOdds(t->right);
    }
    ```

4.  Implement the following function to count number of internal nodes in a given tree. An internal node is a node with at least one child node. Use the following function interface:

    ```c
    int bstCountInternal(struct node *t) {
        if (!t) return 0;
        if (!t->left && !t->right) return 0;
        
        return 1 + bstCountInternal(t->left)
            + bstCountInternal(t->right);
    }
    ```

5.  Implement the following function that returns the level of the node containing a given key if such a node exists, otherwise the function returns -1 (when a given key is not in the binary search tree). The level of the root node is zero. Use the following function interface:

    ```c
    int bstNodeLevel(struct node *t, int key) {
        if (!t) return -1;
        if (t->value == key) return 0;
        
        int level;
        if (t->value > key) {
            level = bstNodeLevel(t->left, key);
        } else {
            level = bstNodeLevel(t->right, key);
        }
        if (level == -1) return -1;
        return level + 1;
    }
    ```

6.  Implement the following function that counts the number of values that are greater than a given value. This function should avoid visiting nodes that it doesn't have to visit. Use the following function interface:

    ```c
    int bstCountGreater(struct node *t, int val) {
        if (!t) return 0;
        
        if (t->value > val) {
            return 1 + bstCountGreater(t->right, val)
                + bstCountGreater(t->left, val);
        } else { // t->value <= val
            return bstCountGreater(t->right, val);
        }
    }
    ```