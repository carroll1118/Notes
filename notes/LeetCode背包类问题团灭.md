@[toc]
#   0-1 背包问题
### 416. 分割等和子集
* [原题地址](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int len = nums.length;
        int sum = 0;
        for(int num:nums){
            sum += num;
        }
        //如何数组的数的和为奇数，则不能分割为两个相等的数组
        if((sum&1) == 1){
            return false;
        }
        sum= sum/2;
        boolean[][] dp = new boolean[len][sum+1];
        //dp[i][j] 代表数组前i个元素中，是否恰好存在和为j的子数组

        if (nums[0] <= sum) {
            dp[0][nums[0]] = true;
        }

        for(int i = 1;i<len;i++){
            for(int j = 1;j<=sum;j++){
                if(j-nums[i]<0){
                    dp[i][j] = dp[i-1][j];
                }else{
                    dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i]];
                }
            }
        }
        return dp[len-1][sum];
    }
}
```

#  完全背包问题
###  322. 零钱兑换
* [原题地址](https://leetcode-cn.com/problems/coin-change/)

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        //dp[i]  表示凑成金额i所需要的的最少硬币数
        int[] dp = new int[amount+1];
        Arrays.fill(dp,amount+1);
        dp[0] = 0;
        for(int coin:coins){
            for(int i = coin;i<=amount;i++){
                dp[i] = Math.min(dp[i],dp[i-coin]+1);
            }
        }
        if(dp[amount]==amount+1){
            return -1;
        }
        return dp[amount];
    }
}
```

> 下面这道题的思路和这题一模一样，换汤不换药！！！！

###  279. 完全平方数
* [原题地址](https://leetcode-cn.com/problems/perfect-squares/)

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n+1];
        //dp[i] 表示正整数为i时，组成的和为i的平方数最小，数量为dp[i]
        for(int i = 1;i<=n;i++){
            dp[i] = i;
            for(int j = 1;i-j*j>=0;j++){
                dp[i] = Math.min(dp[i],dp[i-j*j]+1);
            }
        }
        return dp[n];
    }
}
```
###  518. 零钱兑换 II
* [原题地址](https://leetcode-cn.com/problems/coin-change-2/)
```java
class Solution {
    public int change(int amount, int[] coins) {
        int len = coins.length;

        int[][] dp = new int[len+1][amount+1];
        //dp[i][j]  若只使用前 i 个物品，当背包容量为 j 时，有 dp[i][j] 种方法可以装满背包。
        for(int i = 0;i<=len;i++){
            dp[i][0] = 1; //当容量为0时,任何一个物品都有一种方法装满背包
        }

        for(int i = 1;i<=len;i++){
            for(int j = 1;j<=amount;j++){
                if(j-coins[i-1]>=0){
                    dp[i][j] = dp[i-1][j]+dp[i][j-coins[i-1]];
                }else{
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        return dp[len][amount];
    }
}
```
#  多重背包问题
#  混合三种背包问题
#  二维费用的背包问题
#  分组的背包问题
# 有依赖的背包问题

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
