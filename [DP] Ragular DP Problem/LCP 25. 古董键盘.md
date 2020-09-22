# [LCP 25. 古董键盘](https://leetcode-cn.com/problems/Uh984O/)

## [1] DP

1. Use DP to fill `C`.
2. `dp[i][j]` respresents the num of solutions to fill the `i-th` letter into `j` holes. 

```cpp
class Solution {
public:
    int keyboard(int k, int n) {
        const int MOD = 1e9 + 7;
        vector<vector<long long>> C(n + 1, vector<long long>(n + 1, 0));
        for (int i = 0; i <= n; i++) {
            C[i][0] = 1;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                C[i][j] = 1ll * C[i][j - 1] * (i - j + 1) / j % MOD;
            }
        }
        vector<vector<long long>> dp(26 + 1, vector<long long>(n + 1, 0));
        dp[0][n] = 1;
        for (int i = 0; i < 26; i++) {
            for (int j = 0; j <= n; j++) {
                for (int p = 0; p <= k; p++) {
                    if (j < p) {
                        continue;
                    }
                    dp[i + 1][j - p] += 1ll * dp[i][j] * C[j][p];
                    dp[i + 1][j - p] %= MOD;
                }
            }
        }
        return (int)dp[26][0];
    }
};
```

## [2] DFS

**Backtracking**

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;
    vector<int> status;
    unordered_map<long long, int> vis;
    
    int dfs(int n) {
        long long tmp = 0;
        for (int i = 0; i < status.size(); i++) {
            tmp = tmp * 100 + status[i];
        }
        if (vis.count(tmp)) {
            return vis[tmp];
        }
        if (n == 0) {
            return 1;
        }
        int ans = 0;
        for (int i = status.size() - 1; i >= 1; i--) {
            if (status[i]) {
                status[i]--;
                status[i - 1]++;
                ans = (1ll * ans + 1ll * (status[i] + 1) * dfs(n - 1)) % MOD;
                status[i - 1]--;
                status[i]++;
            }
        }
        vis[tmp] = ans;
        return ans;
    }
    
    int keyboard(int k, int n) {
        status = vector<int>(k + 1, 0);
        status.back() = 26;
        return dfs(n);
    }
};
```

