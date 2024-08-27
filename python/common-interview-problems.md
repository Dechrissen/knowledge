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

## Products of Array Except Self
Given an integer array nums, return an array output where output[i] is the product of all the elements of nums except nums[i].

Each product is guaranteed to fit in a 32-bit integer. 
```
// i don't understand this one at all

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        res = [1] * (len(nums))

        for i in range(1, len(nums)):
            res[i] = res[i-1] * nums[i-1]
        postfix = 1
        for i in range(len(nums) - 1, -1, -1):
            res[i] *= postfix
            postfix *= nums[i]
        return res
```

## Valid Sudoku
https://neetcode.io/problems/valid-sudoku

You are given a a 9 x 9 Sudoku board board. A Sudoku board is valid if the following rules are followed:

    - Each row must contain the digits 1-9 without duplicates.
    - Each column must contain the digits 1-9 without duplicates.
    - Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without duplicates.

Return true if the Sudoku board is valid, otherwise return false

Note: A board does not need to be full or be solvable to be valid.
```
# create sets to hold the numbers in the rows and cols
# and a third set to hold the numbers in the 3x3 squares, which can be represented as indeces 
# 0, 1, 2 on each axis.
# iterate over each tile (nested for loops) and check if that item is elsewhere
# in the current row/col/square (if yes, then it's a duplicate, so return False since board invalid)
# otherwise, add the tile number to each set
# if no duplicates found, return True (valid board)

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        cols = defaultdict(set)
        rows = defaultdict(set)
        squares = defaultdict(set)  # key = (r /3, c /3)

        for r in range(9):
            for c in range(9):
                if board[r][c] == ".":
                    continue
                if (
                    board[r][c] in rows[r]
                    or board[r][c] in cols[c]
                    or board[r][c] in squares[(r // 3, c // 3)]
                ):
                    return False
                cols[c].add(board[r][c])
                rows[r].add(board[r][c])
                squares[(r // 3, c // 3)].add(board[r][c])

        return True

```

## Longest Consecutive Sequence
Given an array of integers nums, return the length of the longest consecutive sequence of elements.

A consecutive sequence is a sequence of elements in which each element is exactly 1 greater than the previous element.

You must write an algorithm that runs in O(n) time.
```
# you want to convert the input array into a set to make them unique
# then check if each element has a "left neighbor" (something comes before it consecutively)
# if it doesn't, then it's the start of a sequence

# so if it's the start of a sequence, then check how many numbers come after it consecutively
# keep track of the longest sequence throughout this problem

class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numSet = set(nums) # create a set from the initial array
        longest = 0 # keep track of the longest consecutive sequence is

        for n in nums:
            # check if its the start of a sequence
            if (n - 1) not in numSet:
                # if we got here, then n is the start of a sequence
                length = 0
                while (n + length) in numSet:
                    length += 1
                longest = max(length, longest)
        return longest
```

## Is palindrome
Given a string s, return true if it is a palindrome, otherwise return false.

A palindrome is a string that reads the same forward and backward. It is also case-insensitive and ignores all non-alphanumeric characters.
### Option 1 (cheater way)
```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s_list = [char.lower() for char in s if char.isalnum()]
        s_list_rev = [char for char in s_list[::-1]]
        if s_list == s_list_rev:
            return True
        return False
```
### Option 2 (more efficient way)
```
# create a left and right pointer to start at either end of the string
# create a custom alphaNum function to check if something is a-z,A-Z,0-9
# using python's ord() function to get ASCII values
# main while loop checks if left pointer is less than right pointer
# (because if left > right, that means they crossed each other,
# and if left == right, that's the exact middle of the string)
# while moving the pointers, we are also using our alphaNum to skip
# indexes if they are not alphanumeric
# if at any point s[left] != s[right], we know it's not a palindrome, so we return False

class Solution:
    def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) -1

        while left < right:
            while left < right and not self.alphaNum(s[left]):
                left += 1
            while right > left and not self.alphaNum(s[right]):
                right -= 1
            if s[left].lower() != s[right].lower():
                return False
            left, right = left + 1, right - 1
        return True

    def alphaNum(self, c):
        return (ord('A') <= ord(c) <= ord('Z') or
                ord('a') <= ord(c) <= ord('z') or
                ord('0') <= ord(c) <= ord('9'))
```

## Two integer sum II (sorted in ascending order)
Given an array of integers numbers that is sorted in non-decreasing order.

Return the indices (1-indexed) of two numbers, [index1, index2], such that they add up to a given target number target and index1 < index2. Note that index1 and index2 cannot be equal, therefore you may not use the same element twice.

There will always be exactly one valid solution.

Your solution must use O(1) additional space.
### Option 1 (easier way, takes more time)
```
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        for i in range(len(numbers)):
            for j in range(len(numbers[i+1:])):
                if (numbers[i] + numbers[j]) == target:
                    return [i+1,j+1]
```
### Option 2 (better way, uses O(n) linear time)
```
# uses two pointers starting at either end of the array
# takes advantage of the fact that the arra is pre-sorted from smallest to greatest
# use curSum to calculate the sum of the numbers at the l and r indexes
# if curSum > target, that means we want to decrease the sum (move r pointer to the left to get a smaller number)
# if curSum < target, that means we want to increase the sum (move l pointer to the right to get a larger number)
# if curSum == target, we found the correct pair and can return the pointers (the indexes) (remember to add 1 for 1-indexed answer)
# this way we are guaranteed to get the correct pair

class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        l, r = 0, len(numbers) - 1

        while l < r:
            curSum = numbers[l] + numbers[r]

            if curSum > target:
                r -= 1
            elif curSum < target:
                l += 1
            else:
                return [l + 1, r + 1]

```