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

## 
