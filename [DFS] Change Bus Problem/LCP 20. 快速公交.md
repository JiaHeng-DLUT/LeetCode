# [LCP 20. 快速公交](https://leetcode-cn.com/problems/meChtZ/)

## [1] DFS [Endless Loop]

```cpp
class Solution {
public:
    unordered_map<int, long long> dp;
    int inc, dec;
    vector<int> jump, cost;
    int m;

    long long dfs(int x) {
        cout << x << endl;
        if (dp.count(x)) {
            return dp[x];
        }
        dp[x] = min(dec + dfs(x + 1), inc + dfs(x - 1));    //没有终止条件，x 会一直增加，导致死循环
        for (int i = 0; i < m; i++) {
            if (x % jump[i] == 0) {
                dp[x] = min(dp[x], cost[i] + dfs(x / jump[i]));
            }
        }
        return dp[x];
    }

    int busRapidTransit(int target, int inc, int dec, vector<int>& jump, vector<int>& cost) {
        this->inc = inc;
        this->dec = dec;
        this->jump = jump;
        this->cost = cost;
        this->m = jump.size();
        return dfs(target);
    }
};
```

## [2] DFS

- `q = x / jump[i]`
- `r = x % jump[i]`

从 x 到 0 有以下几条路，只需要找出时间最短的那条就可以了。

1. 从 x 一步一步走回 0（`x * inc`）。
2. 如果 x 在站点上（`r == 0`），直接跳到 q（`cost[i] + dfs(q)`）。
3. 如果 x 不在站点上（`r != 0`）：
   1. 如果 x 不在第一个站点左侧（`q > 0`），先走到相邻的左侧站点（`inc * r`），然后跳到 q（`cost[i] + dfs(q)`）。
   2. 先走到相邻的右侧站点（`dec * (jump[i] - r)`），然后跳到 q + 1（`cost[i] + dfs(q + 1)`）。

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;
    unordered_map<int, long long> dp;
    int inc, dec;
    vector<int> jump, cost;
    int m;

    long long dfs(int x) {
        if (dp.count(x)) {
            return dp[x];
        }
        long long tmp = (long long)inc * x;
        for (int i = 0; i < m; i++) {
            int q = x / jump[i];
            int r = x % jump[i];
            if (r == 0) {
                tmp = min(tmp, cost[i] + dfs(q));
            }
            else {
                if (x > jump[i]) {
                    tmp = min(tmp, (long long)inc * r + dfs(q) + cost[i]);
                }
                tmp = min(tmp, (long long)dec * (jump[i] - r) + dfs(q + 1) + cost[i]);
            }
        }
        dp[x] = tmp;
        return dp[x];
    }

    int busRapidTransit(int target, int inc, int dec, vector<int>& jump, vector<int>& cost) {
        this->inc = inc;
        this->dec = dec;
        this->jump = jump;
        this->cost = cost;
        this->m = jump.size();
        dp[0] = 0;
        dp[1] = inc;
        return dfs(target) % (MOD);
    }
};
```