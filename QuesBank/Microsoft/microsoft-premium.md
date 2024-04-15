## Inorder Successor in BST
![alt text](/QuesBank/Microsoft/images/image1a.png)

Solution : the big case distinction is unnecessary. Just search from root to bottom, trying to find the smallest node larger than p and return the last one that was larger. Here's ternary style for C++:
```
TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
    TreeNode* candidate = NULL;
    while (root)
        root = (root->val > p->val) ? (candidate = root)->left : root->right;
    return candidate;
}
```
![alt text](/QuesBank/Microsoft/images/image1b.png)

