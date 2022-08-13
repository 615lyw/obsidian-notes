---
date created: 2022-08-03, 19:06:59
date modified: 2022-08-03, 19:07:11
---

# Meta

- alias:
- parent :: [[算法刷题]]
- siblings ::
- child ::
- refs: 
    - [一个方法团灭 LeetCode 股票买卖问题 :: labuladong的算法小抄](https://labuladong.gitee.io/algo/1/13/)
    - https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/
    - https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/
    - https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/

---

# 细节卡点

通用框架：

（昨天的状态 +/- 今天的选择）

```java
dp[-1][...][0] = 0
解释：因为 i 是从 0 开始的，所以 i = -1 意味着还没有开始，这时候的利润当然是 0。

dp[-1][...][1] = -infinity
解释：还没开始的时候，是不可能持有股票的。
// 为什么设置为 -infinity？表示一种不可能的情况，因为使用 max 函数取最大值，故当设置为负无穷时，根本就不会取到这个值，其实设置为 <= 0 的任何值都可以

dp[...][0][0] = 0
解释：因为 k 是从 1 开始的，所以 k = 0 意味着根本不允许交易，这时候利润当然是 0。

dp[...][0][1] = -infinity
解释：不允许交易的情况下，是不可能持有股票的。

base case：
dp[-1][...][0] = dp[...][0][0] = 0
dp[-1][...][1] = dp[...][0][1] = -infinity

状态转移方程：
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i]) // buy 行为消耗交易次数，而非 sell！
// 为什么是 k-1？
// k 代表最大交易次数限制。例如到今天为止有 5 次可交易，今天一定会有 buy 行为，到昨天为止，我最多只能进行 5-1=4 次可交易
```


最佳买卖股票时机含冷冻期：

```java
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], dp[i-2][0] - prices[i])
// 为什么这里是 dp[i-2]？
```



```java
class Solution {
    public int maxProfit(int[] prices) {
        // 两种状态及对应的选择
        // 状态一：今天是哪天 状态二：手上是否有股票
        // 1 当前手上有股票：继续持有 今天买入的 （之前没有交易过，最大利润为 0）
        // 0 当前手上没有股票：之前也没有持有股票 在今天卖出了
        int n = prices.length;
        int[][] dp = new int[n][2];
        
        // base case
        dp[0][0] = 0;
        dp[0][1] = -prices[0];

        // 状态转移方程
        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            //dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], 0 - prices[i]); // 为什么是这个？因为本地只允许一次交易，故若买入股票意味着是第一次买入股票
        }

        return dp[n - 1][0]; // 为什么是这个？ dp 的定义为在 prices[0...i] 的这段日子里所能获得的最大利润，考虑到最后一天就多一天的机会
    }
}
```

```java
class Solution {
    public int maxProfit(int[] prices) {
        // 两种状态及对应的选择
        // 状态一：今天是哪天 状态二：手上是否有股票
        // 1 当前手上有股票：继续持有 今天买入的
        // 0 当前手上没有股票：无事 在今天卖出了
        int n = prices.length;
        int[][] dp = new int[n][2];
        // base case
        dp[0][0] = 0;
        dp[0][1] = -prices[0];

        // 状态转移方程
        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]); // 在上面的基础上改了一行代码，即可实现多次交易
        }

        return dp[n - 1][0];
    }
}
```