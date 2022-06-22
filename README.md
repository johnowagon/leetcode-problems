# leetcode-problems
Current tracker: 4

<h2 style="color: green;"> Two Sum </h2>

The main idea is that if we have seen the value that is equal to the target minus the current value, we know the solution already. When this equation is true, we know the two answers.
$$ tar-curValue = previousValueInDict $$
To apply this, we need to keep track of some precious values as well. Since we are returning the indices of each value, we need the index and the value of what we have seen before. So that is where our dictionary comes into play.
```python
dictionary = { value1: index1, value2:index2 }
```
If target-curValue is in this array, the two answers are dictionary[target-curValue] and i from the loop.
```python
    def twoSum(nums:List[int], target: int) -> List[int]:
        dictionary = {}
        for i,v in enumerate(nums):#get index and value of each entry.
            if target-v in nums.keys():#if we have both values
                return [dictionary[target-v], i]
            else:
                dictionary[v] = i#save the values that we look at
            return -1#should never reach here since the list always has a valid solution
        #Time complexity is at worst O(n), if the last element in the array was one of the
        #values that needed to be returned.
```

## Contains Duplicate
This one seemed pretty easy at first. Sort of like Two Sum in that we have to keep track of values that we have seen before. I initially thought we didn't need a dictionary since we only care if we have seen the value once before. However this approach is suboptimal, because it relied on using the 'in' operator, which has an average runtime of O(n) when used on a list (slow!).
Instead, we can use a dictionary, with the key being the tracked value from nums, and the value being the amount of times it has been seen.
```python
    def containsDuplicate(nums: List[int]) -> bool:
        pastVals = {}
        for i in nums:
            if i not in pastVals:
                pastVals[i] = 0#if we haven't seen the value before
            else:
                pastVals[i] += 1#incrememnt counter if we have seen it before
            if pastVals[i] >= 1:
                return True
        return False
``` 
So our time complexity is O(n) since we only iterate through nums at most once.

## Contains Duplicate II
Super similar tot he last one, except we are just checking if two distinct indicies exist in the array such that abs(i - j) >= k. Right away I noticed that we really only care about values that we have seen before, as usual, so tracking them with a dictionary made sense. We take the index and value of each value in the array, and keep track of both in a dictionary:
```python
dictionary = {value: [index1, index2]}
```
Notice the dictionary of indicies, I thought we would need to keep track of all of them as we go on. From there the code is pretty self explanatory, if we have more than 1 value in the array check and see if our condition is true, if not append the value (wrong!). 
```python
    def containsDuplicateII(nums: List[int], k: int) -> bool:
        pastVals = {}
        for i,v in enumerate(nums):
            if v not in pastVals:
                pastVals[v] = [i]
            else:
                for d in pastVals[v]:
                    if abs(d-i) <= k:
                        return True
                pastVals[v].append(i)
        return False
```
This way works, but a better solution exists. Since we dont really care about the value if it does not satisfy the check, we don't really need to store it anywhere. As the array at pastVals[v] gets longer, so does the time of the inner for loop for this reason. Not keeping track of values gave a major speed boost:
```python
    def containsDuplicateII(nums: List[int], k: int) -> bool:
        pastVals = {}
        for i,v in enumerate(nums):
            if v in pastVals and abs(i-pastVals[v]) <= k:
                return True
            else:
                pastVals[v] = i#so we only really look at 2 indicies at a time here.
        return False
```
## Valid Anagram
This one was straightforward, my hammed solution iterates through both strings and keeps track of a dictionary that counts the appearance of each letter in each word. We then compare the dictionaries and if they are equal, then it is a valid anagram, if not then it isnt.
```python
    def isAnagram(s: str, t: str) -> bool:
        usedLetters1 = {}
        usedLetters2 = {}
        for i in s:
            if i not in usedLetters1:
                usedLetters1[i] = 1
            else:
                usedLetters1[i] += 1
        for j in t:
            if j not in usedLetters2:
                usedLetters2[j] = 1
            else:
                usedLetters2[j] += 1
        if usedLetters1 == usedLetters2:
            return True
        return False
```
To note, we can use one dictionary here instead of two to save on space: if we instead subtract 1 from each value in the dictionary on the second loop, we can then iterate through each value and if one is nonzero we know that it is not a valid anagram. Obviously this trades off time for memory. In my solution, the time complexity is O(n) since we iterate through the input only once.
## Valid Palindrome
This one just checks for a valid palindrome, without the non-aplhanumeric characters. The characters can be checked in place in the loop, heres what I got.
```python
    def isPalindrome(s: str) -> bool:
        x = s.lower()
        start = 0
        end = len(s)-1
        while start < end:
            if not x[start].isalnum():
                start += 1
                continue
            elif not x[end].isalnum():
                end -= 1
                continue
            elif x[start] != x[end]:
                return False
            start += 1
            end -= 1
        return True#if we have exited out of the loop, the string is a valid palindrome.
```
## Best Time to Buy and Sell Stock
Look through an array and find the max profit if you buy at the lowest price and sell at the highest price. Basically we keep track of both the 
minPrice and the current profit, only needing one pass through the array resulting in 0(n) time complexity and constant space.
```python
    def maxProfit(prices: List[int]) -> int:
        curProfit = 0
        minPrice = prices[0]
        for i in range(1, len(prices)):
            minPrice = min(minPrice, prices[i])
            curProfit = max(curProfit, prices[i]-minPrice)
        return curProfit
```