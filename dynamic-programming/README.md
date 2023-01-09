# Dynamic Programming

### Climbing Stairs

It's the most basic form of dynamic programming. You can climb one step or two step each time, return the distinct ways you can reach the top.&#x20;

Thus `next_step = (one_step_before + two_step_before)`

Since two\_stepp\_before + 2 step did not make one more ways for climb, but its just a future version of the step before, thats why we shouldnt so  `next_step = one_step_before + 1 + two_step_before + 1`

My solution use the old school dp array:

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 1

        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        
        return dp[-1]
```

But ACC we can use a two variable to rolling through the array, which saves some space complexity.



```python
def climbStairs(self, n):
    a = b = 1 # a = stair 0 and b = stair 1
    for _ in range(n):
    """
    Based on Fibonacci, formula a+b, 
    where a+b becomes the b of the next computation(b += a) ,
    and b becomes the a of the next computation(a = b+a-a).
    This is too mathematic and we shouldnt waste too much time dig into it,
    """
        a, b = b, a + b 
    return a
```

A more clear way in java:

```java
public int climbStairs(int n) {
    // base cases
    if(n <= 0) return 0;
    if(n == 1) return 1;
    if(n == 2) return 2;
    
    int one_step_before = 2;
    int two_steps_before = 1;
    int all_ways = 0;
    
    for(int i=2; i<n; i++){
        // like move two pointer through a time series
    	all_ways = one_step_before + two_steps_before;
    	two_steps_before = one_step_before;
        one_step_before = all_ways;
    }
    return all_ways;
}va
```
