# Python LeetCode problems
This playlist from DataDaft: https://www.youtube.com/playlist?list=PLiC1doDIe9rDFw1v-pPMBYvD6k1ZotNRO

## Two sum
https://leetcode.com/problems/two-sum

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

```python
# Double for loop O(n^2)
# very slow but low memory

for i in range(len(nums) - 1):
    for j in range(i+1, len(nums)):
        if nums[i] + num[j] == target:
            return [i, j]
```

```python
# Dictionary storage O(n)
# very fast and OK on memory

seen = {}
for i, num in enumerate(nums):
    if target - num in seen:
        return [seen[target - num], i]
    elif num not in seen:
        seen[num] = i

```

## Add 2 numbers
https://leetcode.com/problems/add-two-numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        # this sets the initial ones-place value, 
        # modulo 10 to only get the ones place if there's carryover
        added = ListNode(val = (l1.val + l2.val) % 10)
        # this gets the remainder if there's carryover
        carry_over = (l1.val + l2.val) // 10

        current_node = added

        while (l1.next and l2.next):
            l1 = l1.next
            l2 = l2.next

            current_node.next = ListNode(
                val = (carry_over + l1.val + l2.val) % 10
                )

            carry_over = (carry_over + l1.val + l2.val) // 10
            current_node = current_node.next
        
        while (l1.next):
            l1 = l1.next
            current_node.next = ListNode(
                val = (carry_over + l1.val) % 10
                )

            carry_over = (carry_over + l1.val) // 10
            current_node = current_node.next

        while (l2.next):
            l2 = l2.next
            current_node.next = ListNode(
                val = (carry_over + l2.val) % 10
                )

            carry_over = (carry_over + l2.val) // 10
            current_node = current_node.next
        
        if carry_over > 0:
            current_node.next = ListNode(val = 1)

        return added
```




## Longest substring without repeating characters
https://leetcode.com/problems/longest-substring-without-repeating-characters/

Given a string s, find the length of the longest
substring
without repeating characters.


Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        sub = {} # keep track of substrings {char : index}
        cur_sub_start = 0
        cur_length = 0
        longest = 0

        for i, char in enumerate(s):
            # check if current char already in our dictionary
            # if it is, and if its index is after the start
            # of our current substring, then it is a dupe. set the start
            # of a new substring to be 1 after the current char
            if char in sub and sub[char] >= cur_sub_start:
                cur_sub_start = sub[char] + 1
                cur_length = i - sub[char] # save the current substring length by subtracting the index of the first instance of the duplicate
                sub[char] = i # update the duplicate to save the new index, not the old one

            else:
                sub[char] = i
                cur_length += 1
                if cur_length > longest:
                    longest = cur_length
        
        return longest
```

## Longest palindromic substring
https://leetcode.com/problems/longest-palindromic-substring

Given a string s, return the longest
palindromic
substring
in s.


Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        
        # helper function
        def isPalindrome(s):
            return s == s[::-1]

        # Grow from center solution:
        biggest = s[0]
        step = len(biggest) // 2

        # handle single letter centers
        for center in range(1, len(s) - 1):
            bounds = [center - (1 + step), center + (1 + step)]
            while (bounds[0] > -1) and (bounds[1] < len(s)):
                if isPalindrome(s[bounds[0]:bounds[1]+1]):
                    biggest = s[bounds[0]:bounds[1]+1]
                    step = len(biggest) // 2
                    bounds[0] -= 1
                    bounds[1] += 1
                else:
                    break
        
        # handle double letter centers
        for center in range(step, len(s)-step-1):
            bounds = [center - (step), center + (1 + step)]
            while (bounds[0] > -1) and (bounds[1] < len(s)):
                if isPalindrome(s[bounds[0]:bounds[1]+1]):
                    biggest = s[bounds[0]:bounds[1]+1]
                    step = len(biggest) // 2
                    bounds[0] -= 1
                    bounds[1] += 1
                else:
                    break

        return biggest

```

## Palindrome Number
https://leetcode.com/problems/palindrome-number/description/

Given an integer x, return true if x is a palindrome, and false otherwise.

Example 1:

Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.

### Method 1: String conversion method
```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```

### Method 2: Without converting to string (floor division)
```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        # negatives are never palindromes
        if x < 0:
            return False
        
        rev_num = 0
        digit = 0

        # this loop runs on every iteration of increasing the FLOOR
        # DIVISION divisor, starting with 10
        # the first iteration will give us all the digits up to
        # the 10s place, then the next iteration will do the same
        # for the 100s place. if the divisor is ever greater than
        # the number x, the result will be 0, and the loop will end
        while (x // (10**digit) != 0):
            rev_num = (rev_num * 10) + (x // (10**digit) % 10)
            digit += 1

        return x == rev_num
```