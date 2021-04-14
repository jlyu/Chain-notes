# 基准要求

- [x] DP 思想
- [x] 手写 `LRU` 算法 (自定义双链表和哈希表实现/直接使用LinkedHashMap)
- [x] 手写 `LFU` 算法 (直接使用LinkedHashSet，自定义Node只要2个Map[KN, FN]，不定义需要3个Map[KV, KF, FK])
- [ ] 手写 `BST` 各种遍历框架及 CRUD 结点操作、序列化
- [x] Git rebase原理之二叉树最近的公共祖先 `LCA` (利用了二叉树递归后序遍历)
- [ ] 单调栈
- [ ] 单调队列 











# DP

DP问题本质是求最值，核心是穷举，且一定具备最优子结构。能够找出状态转移方程，就等于可以正确地穷举（穷举+剪枝+备忘）

1. 搞清楚“状态”和“选择”。状态影响dp结构定义，而选择影响状态转移
2. 明确dp数组的定义。就是由第1步状态演变而来。到这步已经可以产出伪代码
3. 根据“选择”，理清状态转移的逻辑。最难的一步
4. 伪代码结合转移逻辑，翻译成源码，并处理边界情况
5. 优化DP结构（递归调用dp函数的改用数组，压缩2维变1维，1维变2个变量，优先考虑加上memo缓存）
6. 写单元测试

套路：问题转化，明确dp数组，找出状态和选择，Base case、最优子结构，（利用数学归纳找）状态转移方程，优化dp结构(压缩2维变1维，1维变2变量)

- [300. Longest Increasing Subsequence](https://leetcode-cn.com/problems/longest-increasing-subsequence/) `LIS`算法只能用在一维DP数组情况。dp[i] 表示以 nums[i] 为结尾的最长递增子序列 `LIS` 长度 （要注意子序列、子串、子数组的区别）
- [354. Russian Doll Envelopes](https://leetcode-cn.com/problems/russian-doll-envelopes/) 此题是 [300. Longest Increasing Subsequence](https://leetcode-cn.com/problems/longest-increasing-subsequence/) 的变体，先对二维数组按第一列排序，第二列就转化为求 `LIS` 算法。
- [53. Maximum Subarray](https://leetcode-cn.com/problems/maximum-subarray/) dp[i] 为 num[i] 为结尾的“最大子数组和”
- [1143. Longest Common Subsequence](https://leetcode-cn.com/problems/longest-common-subsequence/) `LCS`算法，典型的二维DP。dp[i] [j] 表示对于s1[0..i-1] 和 s2[0..j-1]，其LCS长度是 dp[i] [j]
- [72. Edit Distance](https://leetcode-cn.com/problems/edit-distance/) if s字符相同，则 i,j 同时往前移动。 else 则三选一（插入j-1/删除i-1/替换i-1, j-1）dp[i] [j] 就是 s1[0..i] 和 s2[0..j]的最小编辑距离
- [516. Longest Palindromic Subsequence](https://leetcode-cn.com/problems/longest-palindromic-subsequence/) 从中心往两端遍历，dp[0] [n-1]就是字符串s[0..n]的最长回文子序列长度。难度在需要使用反向遍历dp二维数组才能正确的状态转移 
- [1312. Minimum Insertion Steps to Make a String Palindrome](https://leetcode-cn.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)
- [10. Regular Expression Matching](https://leetcode-cn.com/problems/regular-expression-matching/)
- [887. Super Egg Drop](https://leetcode-cn.com/problems/super-egg-drop/)
- [312. Burst Balloons](https://leetcode-cn.com/problems/burst-balloons/)
- [416. Partition Equal Subset Sum](https://leetcode-cn.com/problems/partition-equal-subset-sum/) 01背包问题。（注意第2个for loop 要从后往前遍历
- [518. Coin Change 2](https://leetcode-cn.com/problems/coin-change-2/) 完全背包问题。 
- [198. House Robber](https://leetcode-cn.com/problems/house-robber/) 
- [213. House Robber II](https://leetcode-cn.com/problems/house-robber-ii/) 环形的DP，比较巧妙的定义robRange，因为首尾房子不能同时抢的限定，所以出现3种情况，1首尾同时不抢；2抢头不抢尾；3不抢头抢尾，整理一下发现只考虑23情况即可，在入口处控制rob的范围。
- [337. House Robber III](https://leetcode-cn.com/problems/house-robber-iii/) 二叉树型的DP，只是外型变了内核没有改变，还是选择rob或不rob 
- [494. Target Sum](https://leetcode-cn.com/problems/target-sum/) 用回溯也可以做。可以把这题转化为子集求和 [518. Coin Change 2](https://leetcode-cn.com/problems/coin-change-2/) 问题 



做DP状态定义的时候，要跳出自己对数组的思维。

做DP选择时候发现出现了多种情况，那就把每种情况都算一遍，因为求最值的本质还是穷举。



# 特殊数据结构

## 单调队列

单调队列在添加元素的时候靠删除元素保持队列的单调性，相当于抽取出某个函数中单调递增（或递减）的部分。核心思路和 `单调栈` 类似，push 方法依然在队尾添加元素，但要把比自己小的元素都出队删除。