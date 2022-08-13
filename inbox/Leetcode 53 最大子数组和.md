---
date created: 2022-07-07, 08:45:55
date modified: 2022-07-07, 09:09:47
---

# Meta

- alias:
- parent :: [[算法刷题]]
- siblings ::
- child ::
- refs:
    - https://leetcode.cn/problems/maximum-subarray/
    - [动态规划设计：最大子数组 :: labuladong的算法小抄](https://labuladong.gitee.io/algo/3/25/77/)

---

因为要处理子数组，首先想到 [[滑动窗口]] 技巧，发现左右边界不知道怎么移动。

又因为是求最值，想到用 [[动态规划]]。

模板：

1. base case
2. dp 数组定义
3. for 循环遍历状态
4. 在每个状态做最值选择

容易卡在 dp 数组定义，两种定义：

- 第一种：类似凑零钱中 dp 数组定义，dp[i] 表示 nums[i] 中最大子数组和，最后返回值就是 `dp[nums.length]`
- 第二种：dp[i] 表示以 nums[i] 结尾的最大子数组和，即 nums[i] 这个元素一定在子序列中

第一种定义有一个问题：nums[i] 中最大子数组和与 nums[i] 本身可能不连续，例如：

`[4, -1, 2, 1, -5, 4]`，连续子数组 `[4, -1, 2, 1]` 的和最大，为 6 。最后一个元素 4 和连续子数组 `[4, -1, 2, 1]` 不连续，找不到状态转移方程（就是个递推公式）。

若是第二种定义，每个 nums[i] 元素有两种选择，要么加入 `nums[i - 1]` 的子数组中，要么自己单独为子数组。此时状态转移方程：`dp[i] = Math.max(nums[i], nums[i] + dp[i - 1])`。

最后求整个 nums 中最大和还需遍历一遍 dp 数组。
