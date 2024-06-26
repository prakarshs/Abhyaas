## Design In Memory File System
![design-im-fs](/QuesBank/Salesforce/images/image1a.png)
![alt text](/QuesBank/Salesforce/images/image1b.png)
![alt text](/QuesBank/Salesforce/images/image1c.png)

Solution :

```
class Directory {
public:
    string name;
    map<string, Directory*> directories;
    map<string, string> files;
    Directory(string name) { this->name = name; }
    vector<string> getAllFiles() {
        vector<string> ans;
        for (auto x : directories) {
            ans.push_back(x.first);
        }
        for (auto x : files) {
            ans.push_back(x.first);
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
};

class FileSystem {
public:
    Directory* root;
    FileSystem() { root = new Directory("root"); }

    vector<string> getTokens(string input) {
        stringstream ss(input);
        string word;
        vector<string> tokens;
        while (getline(ss, word, '/')) {
            tokens.push_back(word);
        }
        return tokens;
    }

    Directory* getLastDirectory(string path) {
        vector<string> tokens = getTokens(path);
        Directory* curr = root;
        int n = tokens.size();
        for (int i = 1; i < (n - 1); i++) {
            curr = curr->directories[tokens[i]];
        }
        return curr;
    }

    vector<string> ls(string path) {
        if (path == "/")
            return root->getAllFiles();
        Directory* curr = getLastDirectory(path);
        vector<string> tokens = getTokens(path);
        int n = tokens.size();
        string s = tokens[n - 1];
        if (curr->directories.find(s) != curr->directories.end()) {
            return curr->directories[s]->getAllFiles();
        } else {
            return {tokens[n - 1]};
        }
    }

    void mkdir(string path) {
        vector<string> tokens = getTokens(path);
        int n = tokens.size();
        Directory* curr = root;
        for (int i = 1; i < n; i++) {
            string s = tokens[i];
            if (curr->directories.find(s) != curr->directories.end()) {
                curr = curr->directories[s];
            } else {
                curr->directories[s] = new Directory(s);
                curr = curr->directories[s];
            }
        }
    }

    void addContentToFile(string filePath, string content) {
        Directory* curr = getLastDirectory(filePath);
        vector<string> tokens = getTokens(filePath);
        int n = tokens.size();
        string s = tokens[n - 1];
        if (curr->files.find(s) != curr->files.end()) {
            curr->files[s] = curr->files[s] + content;
        } else {
            curr->files[s] = content;
        }
    }

    string readContentFromFile(string filePath) {
        Directory* curr = getLastDirectory(filePath);
        vector<string> tokens = getTokens(filePath);
        int n = tokens.size();
        string s = tokens[n - 1];
        return curr->files[s];
    }
};

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem* obj = new FileSystem();
 * vector<string> param_1 = obj->ls(path);
 * obj->mkdir(path);
 * obj->addContentToFile(filePath,content);
 * string param_4 = obj->readContentFromFile(filePath);
 */
```

## Meeting Rooms II
![alt text](/QuesBank/Salesforce/images/image2a.png)

Solution :

```
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        map<int, int> mp;

        pair<int, int> pair1 = {1, 2};
        
        for (int i=0; i<intervals.size(); i++) {
            mp[intervals[i][0]]++;
            mp[intervals[i][1]]--;
        }

        int count = 0, maxCount = 0;
        for (auto it:mp) {
            count += it.second;
            if (count > maxCount) maxCount = count;
        }
        return maxCount;
    }
};
```

## Graph Valid Tree
![alt text](/QuesBank/Salesforce/images/image3a.png)
![alt text](/QuesBank/Salesforce/images/image3b.png)

Solution:

```
// For a graph to be a tree:
//      1. Graph must be fully connected. => Use UF to find it.
//      2. Graph must not have any cycles. => If edges >= n, then there would be cycles.

class DSU {
    public:
    vector<int> parent, rank;
    int count;
    
    DSU(int n){
        count = n;
        parent.resize(n);
        rank.resize(n);
        iota(parent.begin(), parent.end(), 0);
    }
    
    int find(int x){
        if(x != parent[x])
            parent[x] = find(parent[x]);
        return parent[x];
    }
    
    void Union(int x, int y){
        int px = find(x);
        int py = find(y);
        if(px != py){
            if(rank[px] > rank[py])
                parent[py] = px;
            else if(rank[px] < rank[py])
                parent[px] = py;
            else {
                parent[py] = px;
                rank[px]++;
            }
            count--;
        }
    }
    int getCount(){
        return count;
    }
    
};

class Solution {
public:
    bool validTree(int n, vector<vector<int>>& e) {
        if(e.size() >= n)
            return false;
        DSU uf(n);
        for(auto E: e){
            uf.Union(E[0], E[1]);
        }
        if(uf.getCount() == 1)
            return true;
        else
            return false;
    }
};
```

## Nested List Weight Sum
![alt text](/QuesBank/Salesforce/images/image4a.png)
![alt text](/QuesBank/Salesforce/images/image4b.png)

Solution :

###### DFS
```
int depthSum(vector<NestedInteger>& nestedList, int depth = 1) {
	int res = 0;
	for(auto &n : nestedList) {
		if(n.isInteger()) res += depth * n.getInteger();
		else res += depthSum(n.getList(), depth + 1);
	}
	return res;
}
```
###### BFS
```
int depthSum(vector<NestedInteger>& nestedList) {
	int res = 0, depth = 1;
	queue<NestedInteger> q;
	for(auto &n : nestedList) q.push(n);
	while(q.size()) {
		int sz = q.size();
		while(sz--) {
			NestedInteger n = q.front();
			q.pop();
			if(n.isInteger()) res += depth * n.getInteger();
			else for(NestedInteger m : n.getList()) q.push(m);
		}
		depth++;
	}
	return res;
}
```

## Check If Number Is Majority In Sorted Array
![alt text](/QuesBank/Salesforce/images/image5a.png)

Solutions:

```
class Solution {
public:
    bool isMajorityElement(vector<int>& nums, int target) {
        const size_t MAJORITY_COUNT = nums.size() / 2;
        size_t count = 0;

        for (const auto num : nums) {
            if (num == target) {
                ++count;
            }
        }

        return count > MAJORITY_COUNT ? true : false;
        
    }
};
```