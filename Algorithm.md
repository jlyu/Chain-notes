# DP

DP问题本质是求最值，核心是穷举，且一定具备最优子结构。能够找出状态转移方程，就等于可以正确地穷举（穷举+剪枝+备忘）

套路：问题转化，找找Base case、状态和选择，明确dp数组，最优子结构，状态转移方程，优化dp结构

- [300. Longest Increasing Subsequence](https://leetcode-cn.com/problems/longest-increasing-subsequence/) `LIS` 算法只能用在一维DP数组情况。dp[i] 表示以 nums[i] 为结尾的最长递增子序列 `LIS` 长度 （要注意子序列、子串、子数组的区别）
- [354. Russian Doll Envelopes](https://leetcode-cn.com/problems/russian-doll-envelopes/) 此题是 300 的变体，先对二维数组按第一列排序，第二列就转化为求 `LIS` 算法。
- [53. Maximum Subarray](https://leetcode-cn.com/problems/maximum-subarray/) dp[i] 为 num[i] 为结尾的“最大子数组和”
- [1143. Longest Common Subsequence](https://leetcode-cn.com/problems/longest-common-subsequence/) `LCS`算法，典型的二维DP。dp[i] [j] 表示对于s1[0..i-1] 和 s2[0..j-1]，其LCS长度是 dp[i] [j]
- [72. Edit Distance](https://leetcode-cn.com/problems/edit-distance/) if s字符相同，则 i,j 同时往前移动。 else 则三选一（插入j-1/删除i-1/替换i-1, j-1）dp[i] [j] 就是 s1[0..i] 和 s2[0..j]的最小编辑距离
- [516. Longest Palindromic Subsequence](https://leetcode-cn.com/problems/longest-palindromic-subsequence/) 从中心往两端遍历，dp[0] [n-1]就是字符串s[0..n]的最长回文子序列长度。难度在需要使用反向遍历dp二维数组才能正确的状态转移 

