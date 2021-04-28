# 算法要求

- [ ] 算法的复杂度分析
- [ ] 排序算法，以及区别和优化
- [ ] 数组中的双指针、滑动窗口思想
- [ ] 利用 Map 和 Set 处理查找表问题
- [ ] 链表的各种问题
- [ ] 利用递归和迭代法解决二叉树问题
- [ ] 栈、队列、DFS、BFS
- [ ] 回溯、贪心算法、动态规划
- [x] DP  (背包[01/完全/多重i-ii-iii/混合/二维费用/分组])
- [x] 手写 `LRU` 算法 (自定义双链表和哈希表实现/直接使用LinkedHashMap)
- [x] 手写 `LFU` 算法 (直接使用LinkedHashSet，自定义Node只要2个Map[KN, FN]，不定义需要3个Map[KV, KF, FK])
- [ ] 手写 `BST` 各种遍历框架及 CRUD 结点操作、序列化
- [x] Git rebase原理之二叉树最近的公共祖先 `LCA` (利用了二叉树递归后序遍历)
- [x] 单调栈
- [x] 单调队列 





# 复杂度

- `n≤30`, 指数级别, dfs+剪枝，状态压缩dp
- `n≤100` => O(n3)O(n3)，floyd，dp，高斯消元
- `n≤1000` => O(n2)O(n2)，O(n2logn)O(n2logn)，dp，二分，朴素版Dijkstra、朴素版Prim、Bellman-Ford
- `n≤10000` => O(n∗n√)O(n∗n)，块状链表、分块、莫队
- `n≤100000` => O(nlogn)O(nlogn) => 各种sort，线段树、树状数组、set/map、heap、拓扑排序、dijkstra+heap、prim+heap、spfa、求凸包、求半平面交、二分、CDQ分治、整体
- `n≤1000000` => O(n)O(n), 以及常数较小的 O(nlogn)O(nlogn) 算法 => 单调队列、 hash、双指针扫描、并查集，kmp、AC自动机，常数比较小的 

  ​      				  O(nlogn)O(nlogn) 的做法：sort、树状数组、heap、dijkstra、spfa
- `n≤10^6` => O(n)O(n)，双指针扫描、kmp、AC自动机、线性筛素数
- `n≤10^9` => O(n√)O(n)，判断质数
- `n≤10^18` => O(logn)O(logn)，最大公约数，快速幂
- `n≤10^1000` => O((logn)2)O((logn)2)，高精度加减乘除
- `n≤10^100000` => O(logk×loglogk)，k表示位数O(logk×loglogk)，k表示位数，高精度加减、FFT/NTT





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

## 背包问题 [01/完全/多重i-ii-iii/混合/二维费用/分组]

- 01背包

  - [x] [416. Partition Equal Subset Sum](https://leetcode-cn.com/problems/partition-equal-subset-sum/)  
- 完全背包
  - [ ] 279
  - [ ] [322. Coin Change](https://leetcode-cn.com/problems/coin-change/)
  - [x] [518. Coin Change 2](https://leetcode-cn.com/problems/coin-change-2/) 
- 多重背包



### 01背包

> 「01背包」是指给定物品价值与体积（对应了「给定价值与成本」），在规定容量下（对应了「限定决策规则」）如何使得所选物品的总价值最大。

有N件物品和一个容量是V的背包。每件物品有且只有一件。

第i件物品的体积是v[i]，价值是w[i]。求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。

```java
public int maxValue(int N, int V, int[] v, int[] w) {
    int[][] dp = new int[N][V+1]; // dp 数组：一个二维数组，其中一维代表当前「当前枚举到哪件物品」，另外一维「现在的剩余容量」，数组装的是「最大价值」。
	for (int i = 0; i < N; i++) {
		for (int j = 0; j <= V; j++) {
			int c1 = dp[i-1][j]; // 只存在2种选择：不放入物品，即上一次的重量状态
			
			int c2 = (j > v[i]) ? dp[i-1][j - v[i]] + w[i] : 0; // 另1种选择：放入物品，前提是重量未超
        	dp[i][j] = Math.max(c1, c2);
		}
	}
	return dp[N-1][V];
}
```

- dp[N]\[C+1] 解法 : 即上述解法
- dp[2]\[C+1] 解法：把dp一维的地方全部 &1，即%2操作. 滚动数组 
- dp[C+1] 解法：第2个for loop改成从后往前遍历： `for (int j = V; j >= v[i]; j--)`

```java
public int maxValue(int N, int V, int[] v, int[] w) {
    int[] dp = new int[V+1]; // dp 数组：一个一维数组，其中一维代表当前「现在的剩余容量」，数组装的是「最大价值」。
	for (int i = 0; i < N; i++) {
		for (int j = V; j >= v[i]; j--) { 
			//int c1 = dp[j]; // 只存在2种选择：不放入物品，即上一次的重量状态
			//int c2 = dp[j - v[i]] + w[i] : 0; // 另1种选择：放入物品，前提是重量未超
        	dp[i][j] = Math.max(dp[j], dp[j - v[i]] + w[i]);
		}
	}
	return dp[V];
}
```





# 回溯算法 

DFS? BFS? 

子集，排列，组合

解数独，括号生成，

```
result = []
def backTrack(path, chooselist):
	if (base case):
		result.add(chooselist)
		return
	
	for loop:
		chooselist.add(path) // 做出选择
		backTrack(path, chooselist) // 递归，回溯
		chooselist.remove(path) //撤销选择
```



# DFS BFS 







# 特殊数据结构

## 单调队列

单调队列在添加元素的时候靠删除元素保持队列的单调性，相当于抽取出某个函数中单调递增（或递减）的部分。核心思路和 `单调栈` 类似，push 方法依然在队尾添加元素，但要把比自己小的元素都出队删除。





# 

# 参考出处

[动态规划/背包问题](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU4NDE3MTEyMA==&action=getalbum&album_id=1751702161341628417&uin=&key=&devicetype=Windows+7+x64&version=6302019a&lang=zh_CN&ascene=7&fontgear=2) 宫水三叶的刷题日记

[由数据范围反推算法复杂度以及算法内容](https://www.acwing.com/blog/content/32/) 