---
layout: post
title: Solutions
author: Chenlu Wang
date: 2019-02-13
---

<!-- more -->


## 1.Two Sum

>Description: Given an array of integers, return indices of the two numbers such that they add up to a specific target.

>You may assume that each input would have exactly one solution, and you may not use the same element twice.

>Given nums = [2, 7, 11, 15], target = 9,

>Because nums[0] + nums[1] = 2 + 7 = 9,

>return [0, 1].

## my solution

```{python}
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        n=len(nums)
        for i in range(n):
            for j in range(i+1,n):
                if nums[i]+nums[j]==target:
                    return i,j

```

## 2.Reverse Integer

>Given a 32-bit signed integer, reverse digits of an integer.

### Example 1:

>Input: 123

>Output: 321

### Example 2:

>Input: -123

>Output: -321

### Example 3:

>Input: 120

>Output: 21

>Note:

>Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

```{python}
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """

        b=0
        xxx=abs(x)
        if xxx>2**31-1:
            return 0
        else:
            xx=abs(x)
            n=len(str(xx))
            a=range(n)
            for i in range(n):
                a[i]=xx%10
                xx=xx//10
            for i in range(n):
                b=b+a[i]*pow(10,n-i-1)
            if b>2**31-1:
                return 0
            else:
                if xxx==x:
                    b=b
                else:
                    b=-b

            return b

```

## 3.Palindrome Number

>Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

### Example 1:

>Input: 121

>Output: true

### Example 2:

>Input: -121

>Output: false

>Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

### Example 3:

>Input: 10

>Output: false

>Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

>Follow up:

>Coud you solve it without converting the integer to a string?

## my Solution

```{python}
class Solution(object):
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """

        b=0


        xx=abs(x)
        n=len(str(xx))
        a=range(n)
        for i in range(n):
            a[i]=xx%10
            xx=xx//10
        for i in range(n):
            b=b+a[i]*pow(10,n-i-1)


        if b==x:
            return True
        else:
            return False

```

## 4.Roman to Integer

>Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

* Symbol  :I , V, X, L, C, D, M

* Value   :1 , 5, 10,50,100,500,1000  

>For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

>Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

* I can be placed before V (5) and X (10) to make 4 and 9.

* X can be placed before L (50) and C (100) to make 40 and 90.

* C can be placed before D (500) and M (1000) to make 400 and 900.

>Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

### Example 1:

>Input: "III"

>Output: 3

### Example 2:

>Input: "IV"

>Output: 4

### Example 3:

>Input: "IX"

>Output: 9

### Example 4:

>Input: "LVIII"

>Output: 58

>Explanation: L = 50, V= 5, III = 3.

### Example 5:

>Input: "MCMXCIV"

>Output: 1994

>Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

## my Solution:

```{python}
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        D={'I':1,'V':5,'X':10,'L':50,'C':100,'D':500,'M':1000}
        value=0
        s=str(s)
        n=len(s)
        for i in range(n):
            if i+1<n and D[s[i]]<D[s[i+1]]:
                value-=D[s[i]]

            else:
                value+=D[s[i]]
        return value
```

## 5.Valid Parentheses

>Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

>An input string is valid if:

- 1. Open brackets must be closed by the same type of brackets.

- 2. Open brackets must be closed in the correct order.

>Note that an empty string is also considered valid.

### Example 1:

>Input: "()"

>Output: true

### Example 2:

>Input: "()[]{}"

>Output: true

### Example 3:

>Input: "(]"

>Output: false

### Example 4:

>Input: "([)]"

>Output: false

### Example 5:

>Input: "{[]}"

>Output: true

## my Solution:

```{python}
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        n=len(s)
        s=list(s)
        if n%2!=0:
            return False
        map={'(':')','[':']','{':'}',')':'s',']':'s','}':'s'}

        j=0
        for i in range(pow(n,2)/2):
            if j+1<len(s) and map[str(s[j])]==s[j+1]:
                s.pop(j)
                s.pop(j)
                j=0

            else:
                s=s
                j+=1    
        n1=len(s)
        if n1==0:
            return True
        else:
            return False
```

## 6.Merge Two Sorted Lists

>Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

### Example:

>Input: 1->2->4, 1->3->4

>Output: 1->1->2->3->4->4

## my Solution:

```{python}
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        a=ListNode(None)
        p=a
        while l1!=None or l2!=None:
            if l1!=None and ((l2!=None and l1.val<=l2.val) or (l2==None)):
                p.next=l1
                l1=l1.next
            else:
                p.next=l2
                l2=l2.next
            p=p.next
        return a.next
```

## 7.Remove Duplicates from Sorted Array

>Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

>Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

### Example 1:

>Given nums = [1,1,2],

>Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

>It doesn't matter what you leave beyond the returned length.

### Example 2:

>Given nums = [0,0,1,1,1,2,2,3,3,4],

>Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

>It doesn't matter what values are set beyond the returned length.

### Clarification:

>Confused why the returned value is an integer but your answer is an array?

>Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

>Internally you can think of this:

```{c++}
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## my Solution
```{python}
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        i=0
        count=0
        while i<len(nums)-1:
            if nums[i]==nums[i+1]:
                nums.pop(i+1)
            else:
                i+=1
                count+=1
        return len(nums)
```

## 8.Remove Element

>Given an array nums and a value val, remove all instances of that value in-place and return the new length.

>Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

>The order of elements can be changed. It doesn't matter what you leave beyond the new length.

### Example 1:

>Given nums = [3,2,2,3], val = 3,

>Your function should return length = 2, with the first two elements of nums being 2.

>It doesn't matter what you leave beyond the returned length.

### Example 2:

>Given nums = [0,1,2,2,3,0,4,2], val = 2,

>Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

>Note that the order of those five elements can be arbitrary.

>It doesn't matter what values are set beyond the returned length.

### Clarification:

>Confused why the returned value is an integer but your answer is an array?

>Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

>Internally you can think of this:

```{c++}
// nums is passed in by reference. (i.e., without making a copy)
int len = removeElement(nums, val);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## my Solution

```{python}
class Solution(object):
    def removeElement(self, nums, val):
        """
        :type nums: List[int]
        :type val: int
        :rtype: int
        """
        i=0
        while i<len(nums):
            if nums[i]==val:
                nums.pop(i)
            else:
                i+=1
        return len(nums)
```

## 9.Implement strStr()

>Implement strStr().

>Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

### Example 1:

>Input: haystack = "hello", needle = "ll"

>Output: 2

### Example 2:

>Input: haystack = "aaaaa", needle = "bba"

>Output: -1

### Clarification:

>What should we return when needle is an empty string? This is a great question to ask during an interview.

>For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().

## my Solution:

```{python}
class Solution(object):
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        n=len(needle)
        m=len(haystack)
        s=0
        if n==0:
            s=0
        elif m==0:
            s=-1
        elif m<n:
            s=-1
        else:
            j=0
            for i in range(m-n+1):
                if haystack[i:i+n]==needle:
                    s=i
                    break
                else:
                    s=-1
        return s

```


## 10. Median of Two Sorted Arrays

>There are two sorted arrays nums1 and nums2 of size m and n respectively.

>Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

>You may assume nums1 and nums2 cannot be both empty.

### Example:

>nums1 = [1, 3]

>nums2 = [2]

>The median is 2.0

```{python}
class Solution(object):

    def partition(self,num):
        p=num[-1]
        n=len(num)
        if n==1:
            return num
        else:
            left=[]
            right=[]
            for i in num[0:n-1]:
                if i<=p:
                    left+=[i]
                else:
                    right+=[i]
            return left,p, right
    def quicksort(self,num):
        if len(num)==1:
            return [num[0]]
        elif len(num)==0:
            return [" "]
        else:

            left,p,right=self.partition(num)
            return [self.quicksort(left)]+[p]+[self.quicksort(right)]
    def expand(self,b):
        if len(b)==1:
            if b[0]==' ':
                return []
            else:
                return b
        else:

            return self.expand(b[0])+[b[1]]+self.expand(b[2])

    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        m=len(nums1)
        n=len(nums2)

        num=nums1+nums2
        b=self.quicksort(num)
        sort_num=self.expand(b)

        if (m+n)%2==0:
            return (sort_num[(m+n)/2-1]+sort_num[(m+n)/2])/2.0
        else:
            return sort_num[(m+n-1)/2]



```

## 11.Merge k Sorted Lists

>Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

### example

>Input:
[
  1->4->5,
  1->3->4,
  2->6
]

>Output: 1->1->2->3->4->4->5->6

```{python}
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def partition(self,num):
        p=num[-1]
        n=len(num)
        if n==1:
            return num
        else:
            left=[]
            right=[]
            for i in num[0:n-1]:
                if i<=p:
                    left+=[i]
                else:
                    right+=[i]
            return left,p, right
    def quicksort(self,num):
        if len(num)==1:
            return [num[0]]
        elif len(num)==0:
            return [" "]
        else:

            left,p,right=self.partition(num)
            return [self.quicksort(left)]+[p]+[self.quicksort(right)]
    def expand(self,b):
        if len(b)==1:
            if b[0]==' ':
                return []
            else:
                return b
        else:

            return self.expand(b[0])+[b[1]]+self.expand(b[2])
    def sortvalue(self,num):
        b=self.quicksort(num)
        sort_num=self.expand(b)
        return sort_num

    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        #get the value:

        if not lists:

            return None
        value=[]

        for list in lists:
            p=True
            while p:
                if list!=None:
                    value+=[list.val]

                    if list.next==None:
                        p=False
                    else:
                        list=list.next
                else:
                    p=False

        if value==[]:
            return None

        value=self.sortvalue(value)
        values=[]
        for val in value:
            values=[val]+values





        Next=None
        end_point=None
        for val in values:
            node=ListNode(val)
            node.next=Next
            Next=node


        return node
```


```{python}
class Solution(object):
    def partition(self,num):
        p=num[-1]
        n=len(num)
        if n==1:
            return num
        else:
            left=[]
            right=[]
            for i in num[0:n-1]:
                if i<=p:
                    left+=[i]
                else:
                    right+=[i]
            return left,p, right
    def quicksort(self,num):
        if len(num)==1:
            return [num[0]]
        elif len(num)==0:
            return [" "]
        else:

            left,p,right=self.partition(num)
            return [self.quicksort(left)]+[p]+[self.quicksort(right)]
    def expand(self,b):
        if len(b)==1:
            if b[0]==' ':
                return []
            else:
                return b
        else:

            return self.expand(b[0])+[b[1]]+self.expand(b[2])
    def sortvalue(self,num):
        b=self.quicksort(num)
        sort_num=self.expand(b)
        return sort_num

    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        #get the value:

        if lists==[]:

            return
        value=[]

        for list in lists:
            p=True
            while p:
                value+=[list.val]
                if list.next==None:
                    p=False
                else:
                    list=list.next

        value=self.sortvalue(value)
        values=[]
        for val in value:
            values=[val]+values





        Next=None
        end_point=None
        for val in values:
            node=ListNode(val)
            node.next=Next
            Next=node


        return node



```
