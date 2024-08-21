# Common Python Interview Problems

## Duplicate integer
Given an integer array nums, return true if any value appears more than once in the array, otherwise return false.
```
# use a hash set
# iterate thru the list, checking first if each item is already in the hash set
# if yes, return True (duplicate)
# if no, add it to the set and move to next

class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        hashset = set()
        
        for i in nums:
            if i in hashset:
                return True
            hashset.add(i)
        return False

```

## Is anagram
Given two strings s and t, return true if the two strings are anagrams of each other, otherwise return false.
### Option 1
```
# sort the strings and check if the sorted strings are equal.
# if yes, it's an anagram

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if not len(s) == len(t):
            return False
        
        s_list = [x for x in s]
        s_list.sort()
        t_list = [x for x in t]
        t_list.sort()

        return s_list == t_list
```
### Option 2
```
# this can also be done by constructing 2 dictionaries
# each containing the counts of each letter in the 2 strings
# and checking for equality

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)
        return countS == countT
```

## Two integer sum
Given an array of integers nums and an integer target, return the indices i and j such that nums[i] + nums[j] == target and i != j.
You may assume that every input has exactly one pair of indices i and j that satisfy the condition.
Return the answer with the smaller index first. 
```
# create a dict to store the previously checked numbers
# iterate over the numbers in the list
# calculate the difference between the current number and the target number
# check if the diff is in the dict
# return both indeces once we find the 2 numbers

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        prevMap = {}  # val -> index

        for i, n in enumerate(nums):
            diff = target - n
            if diff in prevMap:
                return [prevMap[diff], i]
            prevMap[n] = i
```

## Anagram groups
Given an array of strings strs, group all anagrams together into sublists. You may return the output in any order.
### Option 1
```
# create a defaultdict of list
# for each string `s`, create a list `count` of 26 0s (for the alphabet)
# increment the value at the index of each letter in the string
# then append to the list in defaultdict at key `tuple(count)` the value `s` (the string)
# return the values in the defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans = defaultdict(list)

        for s in strs:
            count = [0] * 26
            for c in s:
                count[ord(c) - ord("a")] += 1
            ans[tuple(count)].append(s)
        return ans.values()

```
### Option 2
```
# this is the way I did it, which was more intuitive to me
# sort each word in the list, and then add it to a dict  of sorted_word:[word1, word2, etc] pairs
# for each anagram in the dict's keys, add to a list all the values (the lists of anagrams for that sorted_word)
# return that list

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        solution = []
        sortedDict = {} # sorted_word : [word1, word2, etc]
        for i in range(len(strs)):
            word = strs[i]
            sorted_word = str(sorted(word))
            if sorted_word in sortedDict:
                sortedDict[sorted_word].append(word)
            else:
                sortedDict[sorted_word] = [word]
        
        for anagram in sortedDict:
            solution.append(sortedDict[anagram])


        return solution
```

## Top K elements in list
Given an integer array nums and an integer k, return the k most frequent elements within the array.
The test cases are generated such that the answer is always unique.
You may return the output in any order.
```{python}
# this problem uses bucket sort
# which is the idea that you can use the index values of a list
# to store frequencies of numbers.
# specifically, each index in a list `freq` will store an empty list,
# which we can append a number to if it occurs <index> number of times

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        freq = [[] for i in range(len(nums) + 1)]

        for n in nums:
            # the get method gets the value of some key n,
            # but falls back to 0 (the second arg) if it doesn't exist
            count[n] = 1 + count.get(n, 0)
        for n, c in count.items():
            # append the number n to the sublist in the `freq` list at index c
            freq[c].append(n)

        res = []
        for i in range(len(freq) - 1, 0, -1):
            for n in freq[i]:
                res.append(n)
                if len(res) == k:
                    return res

```

## String encode and decode
Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.

Please implement `encode` and `decode` functions.
```
# this one is hard
# encode the string by starting each word with its length + '#' + word, then concatenate them all together
# decode the string by using 2 while loops nested
# the first one uses the i counter
# the second nested one uses the j counter
# together they get the length of the string (the first number) then look for the '#'
# then it uses the length to get the actual word, and it's added to a list
# after the whole string is iterated through, the list is returned 

class Solution:

    def encode(self, strs: List[str]) -> str:
        long_string = ''
        # each word will get appended to a long string starting with its length and then a '#'
        for s in strs:
            long_string += (str(len(s)) + '#' + s)
        return long_string

    def decode(self, s: str) -> List[str]:
        result = [] # list of strings
        i = 0 # pointer to track position in string

        # iterate over the long concatenated (encoded) string
        while i < len(s):
            j = i
            while s[j] != '#':
                j += 1
            length = int(s[i:j])
            result.append(s[j + 1 : j + 1 + length])
            i = j + 1 + length
        return result
```