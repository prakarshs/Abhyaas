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

## Meeting Rooms II (Salesforce Me Hai)

## Find The Celebrity
![alt text](/QuesBank/Microsoft/images/image5a.png)
![alt text](/QuesBank/Microsoft/images/image5b.png)

Solution : This solution is based on fact that if celebrity is there, everyone knows them, so ask from 0 to n if they know someone, also keep in mind that if they know someone, they can't be celebrity themselves. If there is a celebrity, candidate will stop at someone who doesn't know anyone from (candidate+1 to n-1), then we just have to check that candidate shouldn't know anyone from (0 to candidate-1) and everyone from (candidate+1 to n-1) should know candidate.
```
class Solution {
public:
    int findCelebrity(int n) {
        vector<int> possibleCelebs;
        int candidate = 0;
        for(int i=1; i<n; i++){
            if(knows(candidate, i)) candidate = i;
        }
        
        //Check if candidate found knows anyone or anyone who doesn't know candidate - If any of this is true, the candidate is not a celeb.
        for(int i=0; i<n; i++){
            if(candidate == i) continue;
            if(knows(candidate, i) || !knows(i, candidate)) return -1;
        }
        return candidate;
    }
};
```

## Convert BST To Sorted Double LL
![alt text](/QuesBank/Microsoft/images/image6a.png)
![alt text](/QuesBank/Microsoft/images/image6b.png)

Solution : Leverage inorder traversal to get the increasing order of nodes.
```
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/

class Solution {
    vector<Node*> v;
    void helper(Node* root){
        if(root==nullptr) return;
        helper(root->left);
        v.emplace_back(root);
        helper(root->right);
    }
public:
    Node* treeToDoublyList(Node* root) {
        if(root==nullptr) return root;
        helper(root);
        for(int i=0;i+1<v.size();i++){
            v[i]->right=v[i+1];
            v[i+1]->left=v[i];
        }
        v.back()->right=v.front();
        v.front()->left=v.back();
        return v.front();
    }
};
```
## Design TTT
![alt text](/QuesBank/Microsoft/images/image7a.png)

Solution :
```

class TicTacToe {
public:
    vector<vector<int>> board;
    int n;

    TicTacToe(int n) {
        board.assign(n, vector<int>(n, 0));
        this->n = n;
    }

    int move(int row, int col, int player) {
        board[row][col] = player;
        if (checkCol(col, player) ||
            checkRow(row, player) ||
            (row == col && checkDiagonal(player)) ||
            (row == n - col - 1 && checkAntiDiagonal(player))) {
            return player;
        }
        // No one wins
        return 0;
    }

    bool checkDiagonal(int player) {
        for (int row = 0; row < n; row++) {
            if (board[row][row] != player) return false;
        }
        return true;
    }

    bool checkAntiDiagonal(int player) {
        for (int row = 0; row < n; row++) {
            if (board[row][n - row - 1] != player) return false;
        }
        return true;
    }

    bool checkCol(int col, int player) {
        for (int row = 0; row < n; row++) {
            if (board[row][col] != player) return false;
        }
        return true;
    }

    bool checkRow(int row, int player) {
        for (int col = 0; col < n; col++) {
            if (board[row][col] != player) return false;
        }
        return true;
    }
};
```






