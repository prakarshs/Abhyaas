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




