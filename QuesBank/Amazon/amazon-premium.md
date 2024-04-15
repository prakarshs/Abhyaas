## Minimum Adjacent Swaps To Make Valid Array
![alt text](/QuesBank/Amazon/images/image1a.png)

Solution :

```
class Solution {
public:
    int minimumSwaps(vector<int>& nums) {
        int count = 0;
        auto it = min_element(nums.begin(), nums.end());
        count += it - nums.begin();
        while (it != nums.begin()) {
            swap(*it, *(it-1)); it--;
        }
        auto jt = max_element(nums.rbegin(), nums.rend());
        count += jt - nums.rbegin();
        return count;
    }   
};
```

## Analyse User Website Visit Pattern
![alt text](/QuesBank/Amazon/images/image2a.png)
![alt text](/QuesBank/Amazon/images/image2b.png)

Solution :

```
class Solution {
public:
    vector<string> mostVisitedPattern(vector<string>& username, vector<int>& timestamp, vector<string>& website) {
        unordered_map<string, map<int, string>> mapp; // mapp[username][timestamp] = website;
        map<string, int> freq; // freq["w1 w2 w3"] = count
        int maxFreq = 0;
        for(int i=0; i<username.size(); i++){
            mapp[username[i]][timestamp[i]] = website[i];
        }
        
        for(auto [username, timeWebsiteMap] : mapp){
            unordered_set<string> ss;
            
            for(auto it1 = timeWebsiteMap.begin(); it1 != timeWebsiteMap.end(); it1++){
                for(auto it2 = next(it1); it2 != timeWebsiteMap.end(); it2++){
                    for(auto it3 = next(it2); it3 != timeWebsiteMap.end(); it3++){
                        ss.insert(it1->second + " " + it2->second + " " + it3->second); // all triplets of -> "w1 w2 w3"
                    }
                }
            }
            for(auto s : ss){
                freq[s]++;
                maxFreq = max(maxFreq, freq[s]);
            }
        }
        
        string res = "";
        for(auto [websiteTriplets, _freq] : freq){
            if(_freq == maxFreq){
                res = websiteTriplets;
                break;
            }
        }
        vector<string> resVec(3);
        istringstream ss(res);
        ss >> resVec[0] >> resVec[1] >> resVec[2];
        return resVec;
    }
};

```

## Minimum Number Of Keypresses
![alt text](/QuesBank/Amazon/images/image3a.png)
![alt text](/QuesBank/Amazon/images/image3b.png)
![alt text](/QuesBank/Amazon/images/image3c.png)

Solution :

![alt text](/QuesBank/Amazon/images/image3d.png)

```
class Solution {
public:
    int minimumKeypresses(string s) {
        vector<int> counts(26,0);
        for(char &x: s) counts[x - 'a']++;
        vector<vector<int>> p;
        for(int i = 0; i < 26; i++) {
            p.push_back({counts[i],i});
        }
        sort(p.begin(),p.end());
        reverse(p.begin(),p.end());

        unordered_map<char,int> pos;
        for(int i =0; i < 26; i++) {
            pos[p[i][1]] = i;
        }
        int count = 0;
        for(char &x : s){
            int loc = pos[x - 'a'];
            if(loc <= 8)count+=1;
            else if(loc >= 9 and loc <= 17)count +=2;
            else if(loc >= 18)count+=3;
            
        }
        return count;
    }
};
```

## Maximum Of Number Of Books You Can Take
![alt text](/QuesBank/Amazon/images/image4a.png)
![alt text](/QuesBank/Amazon/images/image4b.png)

Solution :

```
class Solution {
public:
    long long maximumBooks(vector<int>& books) {
        long long max{}, curr{};
        stack<int> ms;
        vector<long long> dp(books.size());

        auto sum = [](long long n) {
            return n <= 0 ? 0 : (n * (n + 1)) / 2;
        };

        for(int i = 0; i < books.size(); i++) {
            while(!ms.empty() and books[ms.top()] >= books[i] - (i - ms.top())) ms.pop();

            if(ms.empty()) dp[i] = sum(books[i]) - sum(books[i] - i - 1);
            else dp[i] = dp[ms.top()] + sum(books[i]) - sum(books[i] - (i - ms.top()));

            max = std::max(max, dp[i]);
            ms.push(i);
        }
        return max;        
    }
};
```

## Number Of Valid Subarrays
![alt text](/QuesBank/Amazon/images/image5a.png)

Solution :

![alt text](/QuesBank/Amazon/images/image5b.png)
```
class Solution {
public:
    int validSubarrays(vector<int>& nums) {
        int ans = 0, stack_ptr = 0;
        for (int num : nums) {
            while (stack_ptr > 0 && num < nums[stack_ptr - 1]) {
                stack_ptr--;
            }
            nums[stack_ptr++] = num;
            ans += stack_ptr;
        }
        return ans;
    }
};
```

## Count The Number Of K Big Indices
![alt text](/QuesBank/Amazon/images/image6a.png)

Solution :

```
# Like K-th Largest, which can be solved by priority queue. This one is similar, we can maintain two priority queue with size k to check every number.

class Solution {
public:
    int kBigIndices(vector<int>& nums, int k) {
        int n = nums.size();
        int retval = 0;
        priority_queue<int> left, right;
        vector<bool> left_good(n, false);
        for(int i=0;i<k;i++) {
            left.push(nums[i]);
            right.push(nums[n-1-i]);
        }
        for (int i=k;i<n-k;i++) {
            if (nums[i]>left.top()) {
                left_good[i] = true;
            } else {
                left.pop();
                left.push(nums[i]);
            }
        }
        for (int i=n-k-1;i>=k;i--) {
            if (nums[i]>right.top() && left_good[i]) {
                retval += 1;
            } else {
                right.pop();
                right.push(nums[i]);
            }
        }
        return retval;
    }
};
```

## Design Search Autocomplete
![alt text](/QuesBank/Amazon/images/image7a.png)
![alt text](/QuesBank/Amazon/images/image7b.png)
![alt text](/QuesBank/Amazon/images/image7c.png)

Solution :

```
class AutocompleteSystem {
public:

    struct TrieNode{
        unordered_map<char, TrieNode*> next;
        bool isEnd = false;
        int count = 0;
        string word = "";
    };

   struct comp{
        bool operator()(const pair<int, string>& a, const pair<int, string>& b)     {
            if(a.first == b.first) return a.second < b.second;
            return a.first > b.first;
        }
    };

    priority_queue<pair<int, string>, vector<pair<int, string>>, comp> pq;

    TrieNode *root;
    string prefix = "";

    void insert(const std::string& sentence, int times = 1) {
        auto* node = root;
        for (const auto& c : sentence) {
            if (!node->next.count(c)) {
                node->next[c] = new TrieNode();
            }
            node = node->next[c];
        }
        node->count += times;
        node->isEnd = true;
        node->word = sentence;
    }

    AutocompleteSystem(vector<string>& sentences, vector<int>& times) {
    
        root = new TrieNode();
        for(int i=0;i<sentences.size();i++){
            insert(sentences[i], times[i]);
        }
        TrieNode *cur = root;
    }


    void dfs(TrieNode* cur){

        if(cur->isEnd){
            pq.push({cur->count, cur->word});
            if(pq.size() > 3){
                pq.pop();
            }
        }
        for(auto node : cur->next){
            dfs(node.second);
        }
    }

    vector<string> search(string prefix){

        TrieNode *cur = root;

        for(auto ch : prefix){
            if(cur->next.find(ch) == cur->next.end()){
                return {};
            }
            cur = cur->next[ch];
        }

        dfs(cur);

        vector<string> ans;
        while(!pq.empty()){
            ans.push_back(pq.top().second);
            pq.pop();
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
    
    vector<string> input(char c) {

        if(c == '#'){
            insert(prefix, 1);
            prefix = "";
            return {};
        }

        prefix += c;
        return search(prefix);
    }
};

/**
 * Your AutocompleteSystem object will be instantiated and called as such:
 * AutocompleteSystem* obj = new AutocompleteSystem(sentences, times);
 * vector<string> param_1 = obj->input(c);
 */
```







