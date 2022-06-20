# leetcode-problems
## Two Sum
The main idea is that if we have seen the value that is equal to the target minus the current value, we know the solution already. When this equation is true, we know the two answers.
$$ tar-curValue = previousValueInDict $$
To apply this, we need to keep track of some precious values as well. Since we are returning the indices of each value, we need the index and the value of what we have seen before. So that is where our dictionary comes into play.
$$ dictionary = { value1: index1, value2:index2 } $$
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