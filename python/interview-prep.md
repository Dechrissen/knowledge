# Python interview topic prep

https://www.youtube.com/watch?v=0K_eZGS5NsU

- dynamically typed language (types determined at runtime)

```
# multiple assignments
n, m = 0, "abc"
```
- python's null is None

## Loops
```
# for loops
for i in range(5):
    print(i)

# can also pass 2 values into range()
for i in range(2, 6):
    print(i)

# can also pass a third value to increment/decrement
for i in range(6, 2, -1):
    print(i)
```

## Math
```
# Division is decimal by default
print(5 / 2)
2.5

# Double slash rounds down
print(5 // 2)
2

# round towards 0 by default so negative numbers will round down
print(-3 // 2)
-2

# modulo
print(10 % 3)
1

# modulo with negative
print(-10 % 3)
2

# to be consistent with other languages' modulo
import math
print(math.fmod(-10, 3))
-1.0

# other math helpers
import math
print(math.floor(3 / 2)) # will explicitly round down
print(math.ceil(3 / 2)) # will explicitly round up
print(math.sqrt(2)) # square root
print(math.pow(2, 3)) # 2 to the power of 3

# Max / Min integers
float("inf") # for max
float("-inf") # for min

# Python numbers are infinite so they never overflow
# e.g. a huge number will be less than infinity
print(math.pow(2, 200) < float("inf"))
True
```

## Array stuff
```
# Arrays (list in python)
arr = [1, 2, 3]

# can be used as a stack
arr.append(4)
arr.append(5)

# pop
arr.pop()

# insert in the middle at index 1
arr.insert(1, 7)

# reassign the value at index 3 (constant time operation)
arr[3] = 0

# Initialize an array of size n with default value 1
n = 5
arr = [1] * n

# -1 will read last value in array, -2 second to last, etc.
print(arr[-1])

# Sublists (aka Slicing)

arr = [1, 2, 3, 4]
print(arr[1:3]) # non-inclusive of second value
[2, 3]

# Unpacking arrays (assigning list items to variables)
a, b, c = [1, 2, 3]
# be careful, the amount of variables on the left needs to match the length of the list

# Iterating over arrays
nums = [1, 2, 3]
# with index
for i in range(len(nums)):
    print(nums[i])
1
2
3

# without index
for n in nums:
    print(n)
1
2
3

# with index and value
for i, n in enumerate(nums):
    print(i, n)
0 1
1 2
2 3

# Loops thru multiple arrays simultaneously
# with unpacking using zip()
nums1 = [1, 3, 5]
nums2 = [2, 4, 6]
for n1, n2 in zip(nums1, nums2):
    print(n1, n2)
1 2
3 4
5 6

# Reverse an array
nums = [1, 2, 3]
nums.reverse()
print(nums)
[3, 2, 1]

# Sorting an array
nums = [5, 4, 2, 3, 1]
nums.sort() # will sort in ascending order by default
print(nums)
1, 2, 3, 4, 5

# to sort in descending order, pass in reverse=True e.g. nums.sort(reverse=True)

# you can also sort a list of strings, and the default sorting is alphabetical order

# Custom array sorting
# to sort using a custom method, you can pass in a lambda function (a function without a name)
# e.g.
arr = ["alice", "christian", "bob"]
arr.sort(key=lambda x: len(x))
print(arr)
["bob", "alice", "christian"]

# in other words, take every value from the array, call it x, and return from that the length of x
# this is the key that will be used to sort the strings
# default is ascending order

# List comprehension
arr = [i for i in range(5)] # shorthand for initializing a list
print(arr)
[0, 1, 2, 3, 4]

# 2-D lists
# using list comprehension
arr = [[0] * 4 for i in range(4)] # will create a 4x4 list of all 0s


# Note: strings are immutable
# you can't update their indeces like you can with arrays
s = "abc"
s[0] = "A"
# won't work!

# get the ASCII value of a char
print(ord("a"))
97
```

## Queues, hash sets, dictionaries, tuples, and other stuff
```
# Queues (double ended queue)
from collections import deque
queue = deque()
queue.append(1)
queue.append(2)
print(queue)
deque([1, 2])

# with queues you can do things like pop from the left
queue.popleft()
print(queue)
deque([2])

# or append to the left
queue.appendleft(1)
print(queue)
deque([1, 2])

# in addition to normal pop() and append()


# HashSet
mySet = set()
mySet.add(1)
mySet.add(2)
print(mySet)
{1, 2} # can't be any duplicates in a hash set

# get the length
print(len(mySet))
2

# check if a value is in the set
print(1 in mySet)
True

# remove value from a set
mySet.remove(2)

# list to set
set([1, 2, 3])

# Set comprehension (like list comprehension)
mySet = { i for i in range(5) }

# HashMap (aka dict) (most common data structure!)
myMap = {}
myMap["alice"] = 88
myMap["bob"] = 77
# can't have duplicate keys in the dict

# update a value using key
myMap["alice"] = 80

# get a value from a key
print(myMap["alice"])
80

# remove a key/value
myMap.pop("alice")

# check if key in dict
print("alice" in myMap)
False

# Dict comprehension
myMap = { i: 2*i for i in range(3) }
print(myMap)
{0: 0, 1: 2, 2: 4}

# Looping dictionaries (iterate over keys by default)
for key in myMap:
    print(key, myMap[key])

# or over values
for val in myMap.values():
    print(val)

# or both keys and values
for key, val in myMap.items():
    print(key, val)


# Tuples (arrays but immutable)
# can do everything you can with lists, except you can't reassign values
tup = (1, 2)
print(tup[0])
1

# can be used as keys for hash map/set
myMap = { (1,2): 3 }
print(myMap[(1,2)])
3

mySet = set()
mySet.add((1, 2))
print((1, 2) in mySet)
True

# list can't be keys for maps/dicts or sets (they are not hashable)


# Heaps (common data struct to find the min or max of a set of values)
# under the hood they are arrays
import heapq

minHeap = []
heapq.heappush(minHeap, 3)
heapq.heappush(minHeap, 2)
heapq.heappush(minHeap, 4)

# min of heap is always at index 0
print(minHeap[0])
2

# loop through a heap while length in nonzero, to get all the min values in ascending order
while len(minHeap):
    print(heapq.heappop(minHeap))
2
3
4

# python doesn't have max heaps by default (workaround is to multiply values by -1)

# build heap from inital values
arr = [2, 1, 8, 4, 5]
heapq.heapify(arr)
while arr:
    print(heapq.heappop(arr))
1
2
4
5
8
```

## Functions
```
# Nested functions

# nested functions have access to outer variables
def outer(a, b):
    c = "c"

    def inner():
        return a + b + c
    return inner()

print(outer("a", "b"))
abc

# can modify objects but not reassign unless using nonlocal keyword
def double(arr, val):
    '''
    doubles every value in an array
    '''
    def helper():
        # modifying array works
        for i, n in enumerate(arr):
            arr[i] *= 2
        
        # will only modify val in the helper() scope
        # val *= 2

        # this will modify val outside helper scope
        nonlocal val
        val *= 2
    helper()
    print(arr, val)

nums = [1, 2]
val = 3
double(nums, val)
[2, 4] 6
```

## Classes
```
class MyClass:
    # constuctor
    def __init__(self, nums):
        # create member variables
        self.nums = nums
        self.size = len(nums)

    # self key word required as parameter
    def getLength(self):
        return self.size

    def getDoubleLength(self):
        return 2 * self.getLength()
```