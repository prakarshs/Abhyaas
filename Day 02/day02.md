## [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/description/)
Solution : <br>
The basic approach is straight forward. Sort kardo given words ko. Ab apne target word ke ek ek substring ko match karwao array wale substring se. and 3 max pe break kardo.
```
class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) 
    {
        sort(products.begin(), products.end()); 

        vector<string> tmp;
        vector<vector<string>> ans;
        string strMatch = "";

        for(auto charWord : searchWord){
            tmp.clear(); // ye zaroori because then pichle waale loop ke 3 strings reh jaate hai
            strMatch += charWord;
            for(auto word : products){
                if(strMatch == word.substr(0,strMatch.length())){
                    tmp.push_back(word);
                }
                if(tmp.size()==3) break;
            }
            ans.push_back(tmp);
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
        auto firstOcc = products.begin(); //initializing
        sort(products.begin(),products.end());
        vector<vector<string>>ans;
        vector<string>temp;
        string curr="";

        for(auto c:searchWord){
            curr+=c;
            temp.clear();                       
            firstOcc = lower_bound(firstOcc,products.end(),curr); // finding first occ of substring

            for(int i=0; firstOcc + i != products.end() && i<3; i++){ // checking of out of bound and less than 3  
                string s=*(firstOcc+i); //converting to string
                if(s.find(curr)) break; // if not starting with curr 
                temp.push_back(s);
            }
            ans.push_back(temp);
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



