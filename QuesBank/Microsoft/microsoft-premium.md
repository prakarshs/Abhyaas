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

## Optimal Account Balancing
![alt text](/QuesBank/Microsoft/images/image2a.png)

Solution :
With all the given transactions, in the end, each person with ID = id will have an overall balance bal[id]. Note that the id value or any person coincidentally with 0 balance is irrelevant to debt settling count, so we can simply use an array debt[] to store all non-zero balances, where

debt[i] > 0 means a person needs to pay $ debt[i] to other person(s); * debt[i] < 0 means a person needs to collect $ debt[i] back from other person(s).
Starting from first debt debt[0], we look for all other debts debt[i] (i>0) which have opposite sign to debt[0]. Then each such debt[i] can make one transaction debt[i] += debt[0] to clear the person with debt debt[0]. From now on, the person with debt debt[0] is dropped out of the problem and we recursively drop persons one by one until everyone's debt is cleared meanwhile updating the minimum number of transactions during DFS.

The question can be transferred to a 3-partition problem, which is NP-Complete.


```
public:
    int minTransfers(vector<vector<int>>& trans) 
	{
        unordered_map<int, long> bal; // each person's overall balance
        for(auto& t: trans) {
		  bal[t[0]] -= t[2];
		  bal[t[1]] += t[2];
		}
		
        for(auto& p: bal) // only deal with non-zero debts
		  if(p.second) debt.push_back(p.second);
		  
        return dfs(0);
    }
    
private:
    int dfs(int s) // min number of transactions to settle starting from debt[s]
	{ 
    	while (s < debt.size() && !debt[s]) ++s; // get next non-zero debt
		
    	int res = INT_MAX;
    	for (long i = s+1, prev = 0; i < debt.size(); ++i)
    	  if (debt[i] != prev && debt[i]*debt[s] < 0) // skip already tested or same sign debt
		  {
		    debt[i] += debt[s]; 
			res = min(res, 1+dfs(s+1)); 
			prev = (debt[i]-=debt[s]);
		  }
    	    
    	return res < INT_MAX? res : 0;
    }
    
    vector<long> debt; // all non-zero debts
```

## Change The Root Of The Binary Tree
![alt text](/QuesBank/Microsoft/images/image3a.png)
![alt text](/QuesBank/Microsoft/images/image3b.png)

Solution :

```
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* parent;
};
*/

class Solution {
public:
    Node* flipBinaryTree(Node* root, Node * leaf) {

        Node* rt = leaf;
		Node* prev = NULL;
		while (true) {
 			Node* pnode = rt->parent;
			rt->parent = prev;

			prev = rt;
            
            if (pnode == NULL) break;

            // update left or right of current / rt  only if you have a parent. Set left to the parent node / pnode

            if (rt->left != NULL) {
                rt->right = rt->left;
            }
			rt->left = pnode; // left child is parent
            
            if (pnode->left == rt) pnode->left = NULL;
            else pnode->right = NULL;
			rt = pnode;
		}
		return leaf;
        
    }
};
```



