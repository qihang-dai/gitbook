# Majority Element

{% embed url="https://leetcode.com/problems/majority-element/" %}

### Description:

Find the majority element that appears more than n / 2 times in a length n array

### My Solution:

Simple and naive, count all into hashmap and iterate to solve it.

The practical problem is again, the hashmap maybe out of memory in some real world case to handle large dataset.

#### Simple Solution

Sort the list, if there is a majority value it must now be the middle value. To confirm it's the majority, run another pass through the list and count it's frequency.

The simple solution is `O(n lg n)` due to the sort though. We can do better!

### Leetcode Solution: &#x20;

Is the keys is a fixed amount? No, so we cant use ArrayMap

Can we refuse to count everything into hashmap but single variables? By rolling through the array, change the single variable to memorize different characters count in certain situation? Yes.

```java
public class Solution {
    public int majorityElement(int[] num) {

        int major=num[0], count = 1;
        for(int i=1; i<num.length;i++){
            if(count==0){
                count++;
                major=num[i];
            }else if(major==num[i]){
                count++;
            }else count--; 
            
        }
        return major;
    }
}
```

Its like candy crush game. You pick one character as your majority.&#x20;

If it meets its kind it grows the count.&#x20;

If not it decrease the count.

If count is decreased down to zero,  we can consider the current major element is eleminated, and we change it into the next&#x20;

## [Majority Element II](https://leetcode.com/problems/majority-element-ii/description/)

This time we need to find all find all elements that appear more than `⌊ n/3 ⌋` times.&#x20;

Still we can do the brutal force count thing. But how about the candy crash one? This is a medium problem, which brings some analysis comlexity.

For n/2, the previous would work, cause one splash one and the result of >n/2 elemnt would survive. But for 3/n, the major element could be 2 or 1 elements. We can transfer this problem into find the two most majority element, and then see if the second majority also have a > 3/n count.

{% embed url="https://leetcode.com/problems/majority-element-ii/solutions/543672/boyer-moore-majority-vote-algorithm-explained/?orderBy=most_votes" %}
I found this solution explanation very heplful
{% endembed %}

### Boyer–Moore majority vote algorithm

{% embed url="https://gregable.com/2013/10/majority-vote-algorithm-find-majority.html" %}

To mimic the first questions vote algo, I wrote something like this:

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:

        main1 = nums[0]
        count1 = 0
        main2 = nums[1]
        count2 = 0
        for i in range(2, len(nums)):
                if count1 == 0:
                    count1 = count1 + 1
                    main1 = nums[i]
                elif main1 == nums[i]:
                    count1 = count1 + 1
                elif count2 == 0:
                    count2 = count2 + 1
                    main2 = nums[i]
                elif main2 == nums[i]:
                    count2 = count2 + 1
                else:
                    count1 = count1 - 1
                    
                    count2 = count2 - 1
        
        print(main1, main2)
        pytho
```

\
However since the min array length is 1, we would have index overflow to initialize the two major. Thus we should change the way we loop. This is trival:

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:

        main1 = None #some random number
        count1 = 0
        main2 = None #some random number unless encounter
        count2 = 0
        for n in nums:
                if count1 == 0:
                    count1 = count1 + 1
                    main1 = n
                elif main1 == n:
                    count1 = count1 + 1
                elif count2 == 0:
                    count2 = count2 + 1
                    main2 = n
                elif main2 == n:
                    count2 = count2 + 1
                else:
                    count1 = count1 - 1
                    count2 = count2 - 1
        
        return [n for n in (major1, major2) if nums.count(n) > len(nums) // 3]
        
```

By assigning two random none exsitence to the two major and use if and elif to control single case is passed for every iteration, we can simply get two different major1 and major2 at first.&#x20;

Now comes the confusion: should we candy crush both major12 to the other element, or we only crush major1 with the other and crush major2 next time? (which is kinda complicated)

Here is the key of Boyer–Moore: why we can get the major element? cause we are making pairs using the array elment, whether its a double pair or triple pair. Once we have a pair, we deduct them away. And since we are making pairs out of the original array, the max amount of one character is 1/2 or 1/3 in all the deducted pairs. and whats left is the two majority (you have extra elements that cant be count into the pairs).

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

And what's left is to see if the count is larger than n/3. We can simply count those elements to see if it works.

Time: O(N) Space: O(1)

So why no use hashmap if you need to count it at last? Cause hashmap space O(N). This O(n) could be some extremley limitation.

Also the above codes is wrong:

with this case: \[2,1,1,3,1,4,5,6], after 3 the count of 2 would be deduct to 0. and the next is 1 -> major1 and major2 would be 1 at the same time. So we must put the increment elif before the assign value elif. Here is the correct code:

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:

        main1 = None #some random number
        count1 = 0
        main2 = None #some random number unless encounter
        count2 = 0
        for n in nums:
                if main2 == n:
                    count2 = count2 + 1
                elif main1 == n:
                    count1 = count1 + 1
                elif count2 == 0:
                    count2 = count2 + 1
                    main2 = n
                elif count1 == 0:
                    count1 = count1 + 1
                    main1 = n
                else:
                    count1 = count1 - 1
                    count2 = count2 - 1
                print(main1, count1, main2, count2)
        
        print(main1, main2) 
        return [n for n in (main1, main2) if nums.count(n) > len(nums) // 3]
        
```

You can see the main2 and main1 sequence is really messed up, but it doenst matter as long as we increment before reassign then we wont have main1 and main2 be the same
