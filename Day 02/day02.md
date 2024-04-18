## [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/description/)
Solution : <br>
The basic approach is straight forward. Sort kardo given words ko. Ab apne target word ke ek ek substring ko match karwao array wale substring se. and 3 max pe break kardo.
```
class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {

        sort(products.begin(), products.end());

        vector<vector<string>> ans; vector<string> res;

        string stringMatch = "";

        for(auto charSearch : searchWord){
            stringMatch += charSearch;
            res.clear();
            for(auto product : products){
                if(stringMatch == product.substr(0, stringMatch.size())){
                    res.push_back(product);
                }
                if(res.size() == 3)break;
            }
            ans.push_back(res);
        }
       return ans; 
    }
};
```
When searching for matching substring we can use binary search because the array is sorted.
```
class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {

        sort(products.begin(), products.end());
        auto firstOcc = products.begin(); //initialize

        vector<vector<string>> ans; vector<string> res;

        string stringMatch = "";

        for(auto charSearch : searchWord){
            stringMatch += charSearch;
            res.clear(); // all same
            
            firstOcc = lower_bound(firstOcc, products.end(),stringMatch); //finding first occ

            for(int i = 0; i<3 && (firstOcc + i)!=products.end(); i++){ // checking outOfBounds and size==3
                string s = *(firstOcc + i); // pointer to value
                if(s.find(stringMatch))break; // checking if starting with
                res.push_back(s);
            }
            ans.push_back(res);
        }
       return ans; 
    }
};
```

## [Design A Text Editor](https://leetcode.com/problems/design-a-text-editor/description/)

Solution : <br>
The logic is simple, leftText, rightText naam ke variable rakho, imaginary cursor inke beech me rahega.

```
class TextEditor {
public:

    string ltext = "", rtext = "";
    TextEditor() {
        
    }
    
    void addText(string text) {
        ltext.append(text);
    }
    
    int deleteText(int k) {
        k = min(k,(int)ltext.size());
        ltext.resize(ltext.size() - k);
        return k;
    }
    
    string cursorLeft(int k) {
        while(k>0 && !ltext.empty()){

            rtext.push_back(ltext.back());

            ltext.pop_back(); k--;
        }

        return ltext.size() >= 10 ? ltext.substr(ltext.size() - 10, 10) : ltext;
        
    }
    
    string cursorRight(int k) {
        while(k>0 && !rtext.empty()){
            ltext.push_back(rtext.back());

            rtext.pop_back(); k--;
        }
    
        return ltext.size() >= 10 ? ltext.substr(ltext.size() - 10, 10) : ltext;
        
    }
};
```

## [Beautiful Arrangements](https://leetcode.com/problems/beautiful-arrangement/)

Solution : [Dry Run Here](https://www.youtube.com/watch?v=xf8qAkqDr8Y)

```
class Solution {
public:
    void solve(vector<int> &arr, int curr, int &ans, int n){

        if(curr > n){ //checking if currIndex over array
            ans++;
            return;
        }

        for(int i = 1; i <= n; i++){
            if(arr[i] == 0 && (i%curr == 0 || curr%i == 0)){
                arr[i] = curr;
                solve(arr,curr+1,ans,n);
                arr[i] = 0; //backtracking
            }
        }

    }


    int countArrangement(int n) {

        vector<int> arr(n+1); // n+1 because 1 position se deal karege
        int curr = 1, ans = 0; // curr index = 1, ans is 0
        solve(arr,curr,ans,n);
        return ans;
    }
};
```

> Next Question Karne se pehle tree ko thoda jaan lo, toh agle 3 4 sawal is based on trees/bst.

### [Lowest Common Ansector in Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

Solution : <br>
[Tech Dose Ka recursion explanation dekho](https://www.youtube.com/watch?v=KobQcxdaZKY)
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {

        if(!root || root == p || root == q) return root;

        auto l = lowestCommonAncestor(root->left, p,q); // left dauda do
        auto r = lowestCommonAncestor(root->right, p,q); // right dauda do

        return l && r ? root : !l && r ? r : l && !r ? l : NULL; 
        
        // agar l aur r dono exist karte toh root return; agar dono me se koi ek toh wo return, agar koi nahi toh NULL
        
    }
};
```

### [Lowest Common Ansector in Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

Solution : <br>
Intuition wahi hai, bas since ye bst; left wale humesha root se chhote. right wale humesha root se bade. Toh agar dono value root se chhoti left me daudao; agar dono value badi toh right me. ek choti ek badi toh obv root ans hai. Jab cant say dono left ya dono roght toh root ans. Edge case is obv root bull

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root)return NULL;
        if(p->val < root->val && root->val < q->val)return root;
        if(p->val < root->val && q->val < root->val)return lowestCommonAncestor(root->left,p,q);
        if(p->val > root->val && q->val > root->val)return lowestCommonAncestor(root->right,p,q);
        return root;
    }
};
```



## [Smallest Subtree With All Deepest Nodes](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/)

Solution : Similar to problem
1123. Lowest Common Ancestor of Deepest Leaves.

```
```





