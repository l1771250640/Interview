# 动态规划问题总结

[1.硬币找零](##1.硬币找零)


[2.硬币组合](##2.硬币组合)


[3.背包问题](##3.背包问题)

[4.最长公共子序列](##4.最长公共子序列)

[5.最长公共子串](##5.最长公共子串)
## 1. 硬币找零
>假设有几种硬币，如1、3、5，并且数量无限。请找出能够组成某个数目的找零所使用最少的硬币数。<br>
* 解答：dp[i] = min(dp[i], dp[i-coins[j]]+1)<br>
``` c++
int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount+1, amount+1);
        dp[0] = 0;
        for(int i=1; i<=amount; i++){
            for(auto c: coins){
                if(i>=c)
                    dp[i] = min(dp[i], dp[i-c]+1);
            }
        }
        return dp[amount]>amount?-1:dp[amount];
    }
```
也可以对每个币种每个金额计算累积。
## 2.硬币组合
>零钱的兑换方式的数量<br>
* 解答：对每个币种->对每个找零金额累加 dp[i] = dp[i] + dp[i-c]

## 3.背包问题
* 二维DP求解
```
 vector<vector<int> > dp(N + 1, vector<int>(V + 1, 0));  // 不要求装满，初始化为 0 即可

    // 核心代码
    for (int i = 1; i <= N; i++) {
        for (int j = 0; j <= V; j++) {  // 可能存在重量为 0，但有价值的物品
            if (w[i] > j)               // 如果当前物品的重量大于剩余容量
                dp[i][j] = dp[i - 1][j];
            else
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]);
        }
    }
    return dp[N][V];
```

* 输出选了哪些物品:
```
for(int i=n;i>1;i--)
    {
        if(m[i][c]==m[i-1][c])
            x[i]=0;
        else
        {
            x[i]=1;
            c-=w[i];
        }
    }
```
* 一维DP求解
```
vector<int> dp(V + 1, 0);

    // 核心代码
    for (int i = 1; i <= N; i++) {
        for (int j = V; j >= w[i]; j--) {           // 递推方向发生了改变
            dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
        }
    }

    return dp[V];
```

## 4.最长公共子序列
```
vector<vector<int> > dp(n+1, vector<int>(m+1, 0));
        // 已经初始化为全 0，就不必再手动初始化 DP 了
        
        for (int i=1; i<=n; i++)
            for (int j=1; j<=m; j++)
                if (A[i-1] == B[j-1])  // 注意下标问题
                    dp[i][j] = dp[i-1][j-1] + 1;
                else
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j]);
        
        return dp[n][m];
```

## 5.最长公共子串

```
vector<vector<int> > dp(n + 1, vector<int>(m + 1, 0));
        // 已经初始化为全 0，就不必再手动初始化 DP 了

        int ret = 0;
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= m; j++)
                if (A[i - 1] == B[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                    ret = max(ret, dp[i][j]);         // 相比最长公共子序列，增加了这行
                }
                else
                    ;                                 // 去掉了这行

        return ret;
```

