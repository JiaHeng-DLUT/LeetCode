# [LCP 24. 数字游戏](https://leetcode-cn.com/problems/5TxKeK/)

## [1] MaxHeap and MinHeap

```cpp
class Solution {
public:
    vector<int> numsGame(vector<int>& nums) {
        const int MOD = 1e9 + 7;
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            nums[i] -= i;
        }
        vector<int> ans;
        priority_queue<int> lo;
        priority_queue<int, vector<int>, greater<int>> hi;
        long long lo_sum = 0, hi_sum = 0;
        for (auto num : nums) {
            lo.push(num);
            lo_sum += num;
            hi.push(lo.top());
            hi_sum += lo.top();
            lo_sum -= lo.top();
            lo.pop();
            if (lo.size() < hi.size()) {
                lo.push(hi.top());
                lo_sum += hi.top();
                hi_sum -= hi.top();
                hi.pop();
            }
            ans.push_back((1ll * lo.top() * lo.size() - lo_sum + hi_sum - 1ll * lo.top() * hi.size()) % MOD);
        }
        return ans;
    }
};
```

## References

1. [Java 等同于求数据流的中位数](https://leetcode-cn.com/problems/5TxKeK/solution/java-deng-tong-yu-qiu-shu-ju-liu-de-zhong-wei-shu-/)

