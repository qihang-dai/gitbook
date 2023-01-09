# PrefixSum

### [Plates-between-candles](https://leetcode.com/problems/plates-between-candles/)

![](<../.gitbook/assets/image (3).png>)

The first time i saw the question i know its a prefixSum question. But the naive prefix seems troublesome and silly. The solution are two type: binary search and prefixsum. But how they get the ideas?&#x20;

#### Break it down

Input: index range

Desired output: how many \* plates are between candles in the index range

step 1. we can keep track of prefix sum at index.  easy

step 2. how to figure out whether its between two candles or not?&#x20;

my first silly question is write a for loop, cur and prev to keep track of two plates and count inside. You can call it brutal force. Then it soon blow my brain.  Think camly it would work: given the index range, find the frist candle and the last candle, and count the plates inside. Then a loop for every index range, O(n2)

You got this clear brutal force idea, you want to implement it? check the input constraints. If its not small, don't hurry to implement this&#x20;

The idea is reasonable, the only fault is its not consistent with the input: we have different index range. We wont want a over time complex for loop each time we search an index range. we want to keep track of it in one run.

Rethink of the input, can we keep track of the left candle and right candle for each index? yes we can. Since we have known we can keep track of plates inside index range, then it works. This is prefixsum way to solve the question (or suffix for right candle postion). [FMI](https://leetcode.com/problems/plates-between-candles/discuss/1586720/Intuition-Explained-oror-Prefix-Sum-and-Binary-Search-oror-C%2B%2B-Clean-Code)

Another way to find the left/right candle without O(n) loop is binary search. But how? we get an index in the array, how can we binary search out the candle postion?  we can make an array with only candle index, then you can binary search the index value in the input. if the index is a candle you can use the index direcly, if the index is not in the candle index array, binary search will give u the left candle index. &#x20;

This binary search tricks really assess the understanding of binary search (what it return if found or not found). and the tricks to use candle index only array is another technique to narrow down the question.







