# Coin Change

## 322. Coin Change

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

&#x20;

**Example 1:**

<pre><code><strong>Input: coins = [1,2,5], amount = 11
</strong><strong>Output: 3
</strong><strong>Explanation: 11 = 5 + 5 + 1
</strong></code></pre>



## Solution

Clealy, its dp, and knapsnacks.  I solved this myself, easy.

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [math.inf] * (amount + 1)
        dp[0] = 0
        for i in range(amount + 1):
            for c in coins:
                if i >= c:
                    dp[i] = min(dp[i], dp[i - c] + 1)
        return -1 if dp[-1] == math.inf else dp[-1]

```

However i didnt have a very clear thought tree. combination or arrangement? we can pick coin multiple times or only one time? I dont even know what my loop means.



## TLDR

I copy paste a detail chinese explanation for all these, but to sum up&#x20;

1. if u for items inside, items has more loop, then it should be Permutation.  Permutation is more than items.
2. 1d array 01 pack, the item is outside and pack is inside and reverse loop.
   1. reverse loop is stop one item being added multiple times.&#x20;
   2. if reversed one is outside, we miss some imporant information.&#x20;
      * pack loop inside: pack loop need to be reversed. if the outside loop is reversed, but dp\[j] = max(dp\[j], dp\[j - weight\[i]] + value\[i]), we would want the smaller pack be calculated first.&#x20;



## 01和完全

我们知道01背包一维数组的内嵌的循环是从大到小遍历，为了保证每个物品仅被添加一次。

而完全背包的物品是可以添加多次的，所以要从小到大去遍历

01背包中二维dp数组的两个for遍历的先后循序是可以颠倒了，一维dp数组的两个for循环先后循序一定是先遍历物品，再遍历背包容量。

**在完全背包中，对于一维dp数组来说，其实两个for循环嵌套顺序是无所谓的！**

### **01**

#### 2 D ARRAY

本文用动规五部曲详细讲解了01背包的二维dp数组的实现方法，大家其实可以发现最简单的是推导公式了，推导公式估计看一遍就记下来了，但难就难在确定初始化和遍历顺序上。

1. 确定dp数组以及下标的含义

dp\[i]\[j] 表示从下标为\[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。

1. 确定递推公式

dp\[i]\[j] = max(dp\[i - 1]\[j], dp\[i - 1]\[j - weight\[i]] + value\[i]);

1. dp数组如何初始化

```
// 初始化 dp
vector<vector<int>> dp(weight.size() + 1, vector<int>(bagWeight + 1, 0));
for (int j = bagWeight; j >= weight[0]; j--) {
    dp[0][j] = dp[0][j - weight[0]] + value[0];
}
```

1. 确定遍历顺序

**01背包二维dp数组在遍历顺序上，外层遍历物品 ，内层遍历背包容量 和 外层遍历背包容量 ，内层遍历物品 都是可以的！**

但是先遍历物品更好理解。代码如下：

```
// weight数组的大小 就是物品个数
for(int i = 1; i < weight.size(); i++) { // 遍历物品
    for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
        if (j < weight[i]) dp[i][j] = dp[i - 1][j]; // 这个是为了展现dp数组里元素的变化
        else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

    }
}
```

1. 举例推导dp数组

背包最大重量为4。

物品为：

|     | 重量 | 价值 |
| --- | -- | -- |
| 物品0 | 1  | 15 |
| 物品1 | 3  | 20 |
| 物品2 | 4  | 30 |

来看一下对应的dp数组的数值，如图：

<figure><img src="https://camo.githubusercontent.com/f40a1c11f88b1b7db9b62bdaf65c8c9a05969b0282d41e6b15cc841a76c21264/68747470733a2f2f696d672d626c6f672e6373646e696d672e636e2f32303231303131383136333432353132392e6a7067" alt=""><figcaption></figcaption></figure>

最终结果就是dp\[2]\[4]。

#### 1D ARRAY

1. 一维dp数组遍历顺序

代码如下：

```
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

**这里大家发现和二维dp的写法中，遍历背包的顺序是不一样的！**

二维dp遍历的时候，背包容量是从小到大，而一维dp遍历的时候，背包是从大到小。

为什么呢？

**倒序遍历是为了保证物品i只被放入一次！**。但如果一旦正序遍历了，那么物品0就会被重复加入多次！

举一个例子：物品0的重量weight\[0] = 1，价值value\[0] = 15

如果正序遍历

dp\[1] = dp\[1 - weight\[0]] + value\[0] = 15

dp\[2] = dp\[2 - weight\[0]] + value\[0] = 30

此时dp\[2]就已经是30了，意味着物品0，被放入了两次，所以不能正序遍历。

为什么倒序遍历，就可以保证物品只放入一次呢？

倒序就是先算dp\[2]

dp\[2] = dp\[2 - weight\[0]] + value\[0] = 15 （dp数组已经都初始化为0）

dp\[1] = dp\[1 - weight\[0]] + value\[0] = 15

所以从后往前循环，每次取得状态不会和之前取得状态重合，这样每种物品就只取一次了。

**那么问题又来了，为什么二维dp数组历的时候不用倒序呢？**

因为对于二维dp，dp\[i]\[j]都是通过上一层即dp\[i - 1]\[j]计算而来，本层的dp\[i]\[j]并不会被覆盖！

（如何这里读不懂，大家就要动手试一试了，空想还是不靠谱的，实践出真知！）

**再来看看两个嵌套for循环的顺序，代码中是先遍历物品嵌套遍历背包容量，那可不可以先遍历背包容量嵌套遍历物品呢？**

不可以！

**因为一维dp的写法，背包容量一定是要倒序遍历（原因上面已经讲了），如果遍历背包容量放在上一层，那么每个dp\[j]就只会放入**一个**物品，即：背包里只放入了一个物品。**

**倒序遍历的原因是，本质上还是一个对二维数组的遍历，并且右下角的值依赖上一层左上角的值，因此需要保证左边的值仍然是上一层的，从右向左覆盖。**

（这里如果读不懂，就在回想一下dp\[j]的定义，或者就把两个for循环顺序颠倒一下试试！）

## 排列和组合

**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。

**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。

1. 确定遍历顺序

本题中我们是外层for循环遍历物品（钱币），内层for遍历背包（金钱总额），还是外层for遍历背包（金钱总额），内层for循环遍历物品（钱币）呢？

我在[动态规划：关于完全背包，你该了解这些！](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html)中讲解了完全背包的两个for循环的先后顺序都是可以的。

**但本题就不行了！**

因为纯完全背包求得装满背包的最大价值是多少，和凑成总和的元素有没有顺序没关系，即：有顺序也行，没有顺序也行！

而本题要求凑成总和的组合数，元素之间明确要求没有顺序。

所以纯完全背包是能凑成总和就行，不用管怎么凑的。

本题是求凑出来的方案个数，且每个方案个数是为组合数。

那么本题，两个for循环的先后顺序可就有说法了。

我们先来看 外层for循环遍历物品（钱币），内层for遍历背包（金钱总额）的情况。

代码如下：

```
for (int i = 0; i < coins.size(); i++) { // 遍历物品
    for (int j = coins[i]; j <= amount; j++) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}
```

假设：coins\[0] = 1，coins\[1] = 5。

那么就是先把1加入计算，然后再把5加入计算，得到的方法数量只有{1, 5}这种情况。而不会出现{5, 1}的情况。

**所以这种遍历顺序中dp\[j]里计算的是组合数！**

如果把两个for交换顺序，代码如下：

```
for (int j = 0; j <= amount; j++) { // 遍历背包容量
    for (int i = 0; i < coins.size(); i++) { // 遍历物品
        if (j - coins[i] >= 0) dp[j] += dp[j - coins[i]];
    }
}
```

背包容量的每一个值，都是经过 1 和 5 的计算，包含了{1, 5} 和 {5, 1}两种情况。

**此时dp\[j]里算出来的就是排列数！**

可能这里很多同学还不是很理解，**建议动手把这两种方案的dp数组数值变化打印出来，对比看一看！（实践出真知）**
