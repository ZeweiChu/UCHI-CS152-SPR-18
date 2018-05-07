
# Binary Tree

Zewei Chu

## Binary Tree

- A node is a structure that may contain a value, a condition, or represent a separate data structure (which could be a tree of its own). 
- Each node in a tree has zero or more child nodes, which are below it in the tree (by convention, trees are drawn growing downwards). 
- A node that has a child is called the child's parent node. A node has at most one parent.
- An internal node or inner node is any node of a tree that has child nodes. Similarly, an external node (also known as an outer node, leaf node, or terminal node), is any node that does not have child nodes.
- The topmost node in a tree is called the root node. Being the topmost node, the root node will not have a parent. It is the node at which operations on the tree commonly begin (although some algorithms begin with the leaf nodes and work up ending at the root). All other nodes can be reached from it by following edges or links. (In the formal definition, each such path is also unique). In diagrams, it is typically drawn at the top
- The height of a node is the length of the longest downward path to a leaf from that node. The height of the root is the height of the tree. The depth of a node is the length of the path to its root (i.e., its root path).
- A subtree of a tree T is a tree consisting of a node in T and all of its descendants in T. The subtree corresponding to the root node is the entire tree;
- A binary tree is a tree data structure in which each node has at most two child nodes, usually distinguished as "left" and "right"


```c
#include<stdio.h>
#include<stdlib.h>
#include<limits.h>

struct _node{
    int value; //float
//     void* value;
    struct _node* left;
    struct _node* right;
};
typedef struct _node node;

// dynamically allocate memory to a new node
node* new_node(int v){
    node* p = (node*)malloc(sizeof(node));
    p->left = p->right = NULL;
    p->value = v;
    return p;
}

// compute the sum of all node values
int tree_sum(node* root){
    if (root == NULL){
        return 0;
    }
    return root->value + tree_sum(root->left) + tree_sum(root->right);
}

// get the height of the tree
int tree_height(node* root){
    if (root == NULL || (root->left == NULL && root->right == NULL)){
        return 0;
    } 
    int lheight = tree_height(root->left);
    int rheight = tree_height(root->right);
//     int 
    if (lheight > rheight){
        return lheight + 1;
    } else {
        return rheight + 1;
    }
}

// get the maximum value of the tree
int tree_max(node* root){
    if (root == NULL)
        return INT_MIN;
    int lmax = tree_max(root->left);
    int rmax = tree_max(root->right);
    int max = lmax>rmax ? lmax : rmax;
    max = root->value>max?root->value:max;
    return max;
}

void print_node(node* n){
    printf("%d\t", n->value);
}

void print_inorder_traverse(node* t){
    if (t == NULL){
        return;
    }
    print_inorder_traverse(t->left);
    print_node(t);
    print_inorder_traverse(t->right);
}

int main(){
    node* root = new_node(5);
    root->left = new_node(4);
    root->left->left = new_node(3);
    root->right = new_node(8);
    root->right->left = new_node(6);
    root->right->right = new_node(9);
    
    printf("sum of all tree nodes: %d\n", tree_sum(root));
    printf("height of the tree: %d\n", tree_height(root));
    printf("%d\n", INT_MIN);
    printf("%d\n", INT_MAX);
    printf("max value of the tree: %d\n", tree_max(root));
    
    
//     print_inorder_traverse(root);
    return 0;
}
```

    sum of all tree nodes: 35
    height of the tree: 2
    -2147483648
    2147483647
    max value of the tree: 9


###  Tree Traversals

A tree traversal is the order in which you visit the nodes. Depending on what data is in the tree and how it is organized, different orders make sense.


## Binary Search Tree

Binary search tree is an abstract data type. It has a defined interface with specific time  guarantees, though it can be implemented as either an array or a linked list. 

A binary search tree is a node based binary tree data structure. Each node contains two pointers pointing to other nodes, one to the left child node, the other to the right child node. 

- The left subtree of a node only contains nodes with keys less than (or equal to) the current node. 
- The right subtree of a node only contains nodes with keys greater than (or equal to) the current node. 
- Both the left and right subtrees must also be binary search trees. 

Based on the above definition of binary search tree, you need a definition of node and a comparison function of nodes to create a binary search tree. 


```c
#include<stdio.h>
#include<stdlib.h>

typedef struct _node{
    int value;
    struct _node* left;
    struct _node* right;
}node;

// dynamically allocate memory to a new node
node* new_node(int v){
    node* p = (node*)malloc(sizeof(node));
    p->left = p->right = NULL;
    p->value = v;
    return p;
}

// insert value into a subtree with root root
// return the root of the new tree
node* insert(node* root, int value){
    if (root == NULL){
        return new_node(value);
    }
    if (root->value < value){
        root->right = insert(root->right, value);
    } else {
        root->left = insert(root->left, value);
    }
    return root;
}

void print_node(node* n){
    if (!n){
        printf("the node is NULL\n");
        return ;
    }
    printf("%d\t", n->value);
}

void pretty_print_node(node* n){
    printf("pretty: %d\n", n->value);
}

void inorder_traverse(node* t, void(*func)(node*)){
    if (t == NULL){
        return;
    }
    inorder_traverse(t->left, func);
    func(t);
    inorder_traverse(t->right, func);
}

node* search(node* root, int value){
    if (!root || value == root->value){
        return root;
    } else if (value < root->value){
        return search(root->left, value);
    } else {
        return search(root->right, value);
    }
}

node* delete(node* root, int value){
    if (root == NULL){
        return NULL;
    }
    if (root->value < value){
        root->right = delete(root->right, value);
    } else if (root->value > value){
        root->left = delete(root->left, value);
    } else { // root->value == value
        if (root->left == NULL){
            node* tmp = root->right;
            free(root);
            return tmp;
        } else if (root->right == NULL){
            node* tmp = root->left;
            free(root);
            return tmp;
        } else {
            node* tmp = root->left;
            while (tmp->right != NULL) tmp = tmp->right;
            root->value = tmp->value;
            root->left = delete(root->left, tmp->value);
        }
    }
    return root;
}

// node* insert(node* root, int value){
//     if (root == NULL){
//         return new_node(value);
//     }
//     if (value <= root->value){
//         root->left = insert(root->left, value);
//     } else {
//         root->right = insert(root->right, value);
//     }
//     return root;
// }

// node* delete(node* root, int value){
//     if (root == NULL){
//         return NULL;
//     }
//     if (value < root->value){
//         root->left = delete(root->left, value);
//     } else if (value > root->value) {
//         root->right = delete(root->right, value);
//     } else {
//         if (root->left == NULL){
//             node* tmp = root->right;
//             free(root);
//             return tmp;
//         } else if (root->right == NULL){
//             node* tmp = root->left;
//             free(root);
//             return tmp;
//         } else {
//             node* tmp = root->left;
//             while (tmp->right != NULL) tmp = tmp->right;
//             root->value = tmp->value;
//             root->left = delete(root->left, tmp->value);
//         }
//     }
//     return root;
// }

int main(){
    node* root = new_node(12);
    root = insert(root, 10);
    root = insert(root, 2);
    root = insert(root, 0);
    root = insert(root, 22);
    root = insert(root, 18);
    root = insert(root, 5);
    root = insert(root, 5);
    root = insert(root, 5);
    root = insert(root, 5);
    root = delete(root, 22);
//     root = delete(root, 5);
    
    node* tmp = search(root, 18);
    print_node(tmp);
    
    inorder_traverse(root, pretty_print_node);
}
```

    18	pretty: 0
    pretty: 2
    pretty: 5
    pretty: 5
    pretty: 5
    pretty: 5
    pretty: 10
    pretty: 12
    pretty: 18

