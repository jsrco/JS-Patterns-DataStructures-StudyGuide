---
title: Study Guide
category: Study
tags:
- Studying
dir: ltr
description: >
  Problem Solving Patterns, Algorithms, and Data Structures.
---
# Code

Inline `code`

Indented code

    // Some comments
    line 1 of code
    line 2 of code
    line 3 of code

Block code "fences"

```
Sample text here...
```

Syntax highlighting

```js
let foo = function (bar) {
  return bar++;
};

console.log(foo(5));
```

# Problem Solving Patterns

## Frequency Counter Patterns

This pattern uses objects or sets to collect values / frequencies of values.

This can often avoid the need for nested loops or 0(n^2) operations with arrays / strings.

```js
const same = (str1, str2) => {
  if (str1.length !== str2.length) {
    return false
  }
  const frequencyCounter1 = {}
  const frequencyCounter2 = {}
  for (let val of str1) {
    frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1
  }
  for (let val of str2) {
    frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1
  }
  for (let key in frequencyCounter1) {
    if (!(key ** 2 in frequencyCounter2)) {
      return false
    }
    if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) {
      return false
    }
  }
  return true
}

const validAnagram = (first, second) => {
  if (first.length !== second.length) {
    return false
  }
  const lookup = {}
  for (let i = 0; i < first.length; i++) {
    let letter = first[i]
    // if letter exists, increment, otherwise set to 1
    lookup[letter] ? lookup[letter] += 1 : lookup[letter] = 1
  }
  for (let i = 0; i < second.length; i++) {
    let letter = second[i]
    // can't find letter or letter is zero then it's not an anagram
    if (!lookup[letter]) {
      return false
    } else {
      lookup[letter] -= 1
    }
  }
  return true
}

const sameFrequency = (arg1, arg2) => {
    let arr1 = Array.from(arg1.toString())
    let arr2 = Array.from(arg2.toString())
    if (arr1.length !== arr2.length) {
        return false
    }
    let frequencyCounter1 = {}
    let frequencyCounter2 = {}

    for (let val of arr1) {
        frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1
    }
    for (let val of arr2) {
        frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1
    }
    for (let key in frequencyCounter1) {
        if (!(key in frequencyCounter2)) {
            return false
        }
        if (frequencyCounter2[key] !== frequencyCounter1[key]) {
            return false
        }
    }
    return true
}

const areThereDuplicates = () => {
    // return new Set(arguments).size !== arguments.length
    let collection = {}
    for (let val in arguments) {
        collection[arguments[val]] = (collection[arguments[val]] || 0) + 1
    }
    for (let key in collection) {
        if (collection[key] > 1) return true
    }
    return false
}
```

## Multiple Pointers
Creating pointers or values that correspond to an index or position and move towards the beginning, end, or middle based on a certain condition.

Very efficient for solving problems with minimal space complexity as well. 

```js
const sumZero = (arr) => {
  let left = 0
  let right = arr.length - 1
  while (left < right) {
    let sum = arr[left] + arr[right]
    if (sum === 0) {
      return [arr[left], arr[right]]
    } else if (sum > 0) {
      right--
    } else {
      left++
    }
  }
}

const countUniqueValues = (arr) => {
  if (arr.length === 0) return 0
  let i = 0
  for (let j = 1; j < arr.length; j++) {
    if (arr[i] !== arr[j]) {
      i++
      arr[i] = arr[j]
    }
  }
  return i + 1
}

const areThereDuplicates = (...args) => {
    // return new Set(arguments).size !== arguments.length
    // Two pointers
    args.sort((a, b) => a > b)
    let start = 0
    let next = 1
    while (next < args.length) {
        if (args[start] === args[next]) {
            return true
        }
        start++
        next++
    }
    return false
}

const averagePair = (arr, num) => {
    let start = 0
    let end = arr.length - 1
    while (start < end) {
        let avg = (arr[start] + arr[end]) / 2
        if (avg === num) return true
        else if (avg < num) start++
        else end--
    }
    return false
}

const isSubsequence = (str1, str2) => {
    let i = 0
    let j = 0
    if (!str1) return true
    while (j < str2.length) {
        if (str2[j] === str1[i]) i++
        if (i === str1.length) return true
        j++
    }
    return false


}
```

## Sliding Window
This pattern involves creating a window which can either be an array or number from one position to another.

Depending on a certain condition, the window either increases or closes (and a new window is created).

Very useful for keeping rack of a subset of data in an array / string etc.


```js
const maxSubarraySum = (arr, num) => {
    let maxSum = 0
    let tempSum = 0
    if (arr.length < num) return null
    for (let i = 0; i < num; i++) {
        maxSum += arr[i]
    }
    tempSum = maxSum
    for (let i = num; i < arr.length; i++) {
        tempSum = tempSum - arr[i - num] + arr[i]
        maxSum = Math.max(maxSum, tempSum)
    }
    return maxSum
}

const maxSubarraySum = (arr, num) => {
    if (arr.length < num) return null
    let total = 0
    for (let i = 0; i < num; i++) {
        total += arr[i]
    }
    let currentTotal = total
    for (let i = num; i < arr.length; i++) {
        currentTotal += arr[i] - arr[i - num]
        total = Math.max(total, currentTotal)
    }
    return total
}

const minSubArrayLen = (nums, sum) => {
    let total = 0
    let start = 0
    let end = 0
    let minLen = Infinity
    while (start < nums.length) {
        if (total < sum && end < nums.length) {
            total += nums[end]
            end++
        }
        else if (total >= sum) {
            minLen = Math.min(minLen, end - start)
            total -= nums[start]
            start++
        }
        else {
            break
        }
    }
    return minLen === Infinity ? 0 : minLen
}

const findLongestSubstring = (str) => {
    let longest = 0
    let seen = {}
    let start = 0
    for (let i = 0; i < str.length; i++) {
        let char = str[i]
        if (seen[char]) {
            start = Math.max(start, seen[char])
        }
        // index - beginning of substring + 1 (to include current in count)
        longest = Math.max(longest, i - start + 1)
        // store the index of the next char so as to not double count
        seen[char] = i + 1
    }
    return longest
}
```

## Divide and Conquer
This pattern involves dividing a data set into smaller chunks and then repeating a process with a subset of data. 

This pattern can tremendously decrease time complexity.

```js
const search = (array, val) => {
    let min = 0
    let max = array.length - 1
    while (min <= max) {
        let middle = Math.floor((min + max) / 2)
        let currentElement = array[middle]
        if (array[middle] < val) min = middle + 1
        else if (array[middle] > val) max = middle - 1
        else return middle
    }
    return - 1
}
```

## Recursion

A process (a function in our case) that calls itself.

```js
// Helper function
const collectOddValues = (arr) => {
    let result = []
    const helper = (helperInput) => {
        if (helperInput.length === 0) {
            return
        }
        if (helperInput[0] % 2 !== 0) {
            result.push(helperInput[0])
        }
        helper(helperInput.slice(1))
    }
    helper(arr)
    return result
}

// Pure recursion
const collectOddValues = (arr) => {
    let newArr = []
    if (arr.length === 0) {
        return newArr
    }
    if (arr[0] % 2 !== 0) {
        newArr.push(arr[0])
    }
    newArr = newArr.concat(collectOddValues(arr.slice(1)))
    return newArr
}

const countDown = (num) => {
    if (num <= 0) {
        console.log("All done!")
        return
    }
    console.log(num)
    num--
    countDown(num)
}

const sumRange = (num) => {
   if (num === 1) return 1
   return num + sumRange(num - 1)
}

const factorial = (x) => {
   if (x < 0) return 0
   if (x <= 1) return 1
   return x * factorial(x - 1)
}

const power = (base, exponent) => {
    if (exponent === 0) return 1
    return base * power(base, exponent - 1)
}

const productOfArray = (arr) => {
    if (arr.length === 0) return 1
    return arr[0] * productOfArray(arr.slice(1))
}

const recursiveRange = (x) => {
    if (x === 0) return 0
    return x + recursiveRange(x - 1)
}

const fib = (n) => {
    if (n <= 2) return 1
    return fib(n - 1) + fib(n - 2)
}

const reverse = (str) => {
    if (str.length <= 1) return str
    return reverse(str.slice(1)) + str[0]
}

const isPalindrome = (str) => {
    if (str.length === 1) return true
    if (str.length === 2) return str[0] === str[1]
    if (str[0] === str.slice(-1)) return isPalindrome(str.slice(1, -1))
    return false
}

const someRecursive = (array, callback) => {
    if (array.length === 0) return false
    if (callback(array[0])) return true
    return someRecursive(array.slice(1), callback)
}

const flatten = (oldArr) => {
    let newArr = []
    for (let i = 0; i < oldArr.length; i++) {
        if (Array.isArray(oldArr[i])) {
            newArr = newArr.concat(flatten(oldArr[i]))
        } else {
            newArr.push(oldArr[i])
        }
    }
    return newArr
}

const capitalizeFirst = (arr) => {
    if (arr.length === 1) return [arr[0][0].toUpperCase() + arr[0].substr(1)]
    const res = capitalizeFirst(arr.slice(0, -1))
    const string = arr.slice(arr.length - 1)[0][0].toUpperCase() + arr.slice(arr.length - 1)[0].substr(1)
    res.push(string)
    return res
}

const nestedEvenSum = (obj, sum = 0) => {
    for (let key in obj) {
        if (typeof obj[key] === "object") sum += nestedEvenSum(obj[key])
        else if (typeof obj[key] === "number" && obj[key] % 2 === 0) sum += obj[key]
    }
    return sum
}

const capitalizeWords = (arr) => {
    if (arr.length === 1) return [arr[0].toUpperCase()]
    let res = capitalizeWords(arr.slice(0, -1))
    res.push(arr.slice(arr.length - 1)[0].toUpperCase())
    return res
}

const stringifyNumbers = (obj) => {
    let newObj = {}
    for (let key in obj) {
        if (typeof obj[key] === 'number') newObj[key] = obj[key].toString()
        else if (typeof obj[key] === 'object' && !Array.isArray(obj[key])) newObj[key] = stringifyNumbers(obj[key])
        else newObj[key] = obj[key]
    }
    return newObj
}

const collectStrings = (obj) => {
    let stringsArr = []
    for (let key in obj) {
        if (typeof obj[key] === 'string') stringsArr.push(obj[key])
        else if (typeof obj[key] === 'object') stringsArr = stringsArr.concat(collectStrings(obj[key]))
    }
    return stringsArr
}
```

# Searching Algorithms 

## Linear Search

For unsorted arrays. Best case 0(1), worst case 0(n). Average is 0(n).

```js
const linearSearch = (arr, num) => {
    let inc = 0
    while (inc < arr.length) {
        if (arr[inc] === num) return inc
        inc++
    }
    return -1
}
```

## Binary Search

Only works on sorted arrays. Best case 0(1), worst and average case 0(log n).

```js
const binarySearch = (arr, elem) => {
    let start = 0
    let end = arr.length - 1
    let middle = Math.floor((start + end) / 2)
    while (arr[middle] !== elem && start <= end) {
        if (elem < arr[middle]) {
            end = middle - 1
        } else {
            start = middle + 1
        }
        middle = Math.floor((start + end) / 2)
    }
    return arr[middle] === elem ? middle : -1
}
```

## Naive String Search


```js
const naiveSearch = (long, short) => {
    let count = 0
    for (let i = 0; i < long.length; i++) {
        for (let j = 0; j < short.length; j++) {
            if (short[j] !== long[i + j]) break
            if (j === short.length - 1) count++
        }
    }
    return count
}
```

# Sorting Algorithms

## Bubble Sort

A sorting algorithm where the largest values bubble up to the top. 0(n^2)

```js
const bubbleSort = (arr) => {
    let noSwaps
    for (let i = arr.length; i > 0; i--) {
        noSwaps = true
        for (let j = 0; j < i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                let temp = arr[j]
                arr[j] = arr[j + 1]
                arr[j + 1] = temp
                noSwaps = false
            }
        }
        if (noSwaps) break
    }
    return arr
}
```

## Selection Sort

Similar to bubble sort, but instead of placing large values into sorted position, it places small values into sorted position. 0(n^2)

```js
const selectionSort = (arr) => {
    for (let i = 0; i < arr.length; i++) {
        let lowest = i
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[lowest]) {
                lowest = j
            }
        }
        if (i !== lowest) {
            // swap
            let temp = arr[i]
            arr[i] = arr[lowest]
            arr[lowest] = temp
        }
    }
    return arr
}
```

## Insertion Sort

Builds up the sort by gradually creating a larger left half which is always sorted. 0(n^2), Great for data coming in live or streaming.

```js
const inerstionSort = (arr) => {
    let currentVal
    for (let i = 1; i < arr.length; i++) {
        currentVal = arr[i]
        let target = 0
        for (let j = i - 1; j >= 0 && arr[j] > currentVal; j--) {
            arr[j + 1] = arr[j]
            target = j
        }
        arr[target + 1] = currentVal
    }
    return arr
}
```

# Intermediate Sorting Algorithms

## Merge Sort

It's a combination of two things, merging and sorting.
Exploits the fact that arrays of 0 or 1 elements are always sorted.
Works by decomposing an array into smaller arrays of 0 or 1 elements, then building up a newly sorted array.
0(n log n)

```js
const mergeHelper = (arr1, arr2) => {
    let combine = []
    let left = 0
    let right = 0
    while (left < arr1.length && right < arr2.length) {
        if (arr1[left] < arr2[right]) {
            combine.push(arr1[left])
            left++
        } else {
            combine.push(arr2[right])
            right++
        }
    }
    while (left < arr1.length) {
        combine.push(arr1[left])
        left++
    }
    while (right < arr2.length) {
        combine.push(arr2[right])
        right++
    }
    return combine
}

const mergeSort = (arr) => {
    if (arr.length <= 1) return arr
    let mid = Math.floor(arr.length / 2)
    let left = mergeSort(arr.slice(0, mid))
    let right = mergeSort(arr.slice(mid))
    return mergeHelper(left, right)
}
```

## Quick Sort

Like merge sort, exploits the fact that arrays of 0 or 1 element are always sorted.
Works by selecting one element (called the "pivot") and finding the index where the pivot should end up in the sorted array.
Best and average 0(n log n), worst 0(n^2).

```js
const pivot = (arr, start = 0, end = arr.length - 1) => {
  const swap = (arr, idx1, idx2) => {
    [arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]]
  }
  let pivot = arr[start]
  let swapIdx = start
  for (let i = start + 1; i <= end; i++) {
    if (pivot > arr[i]) {
      swapIdx++
      swap(arr, swapIdx, i)
    }
  }
  swap(arr, start, swapIdx)
  return swapIdx
}

const quickSort = (arr, left = 0, right = arr.length - 1) => {
  if (left < right) {
    let pivotIndex = pivot(arr, left, right)
    quickSort(arr, left, pivotIndex - 1)
    quickSort(arr, pivotIndex + 1, right)
  }
  return arr
}
```

## Radix Sort

Is a special sorting algorithm that works on lists of numbers. 
It never makes comparisons between elements. It exploits the fact that information about the size of a number is encoded in the number of digits.
0(nk)

```js
const digitCount = (num) => {
  if (num === 0) return 1
  return Math.floor(Math.log10(Math.abs(num))) + 1
}

const getDigit = (num, i) => {
  return Math.floor(Math.abs(num) / Math.pow(10, i)) % 10
}

const mostDigits = (nums) => {
  let maxDigits = 0
  for (let i = 0; i < nums.length; i++) {
    maxDigits = Math.max(maxDigits, digitCount(nums[i]))
  }
  return maxDigits
}

const radixSort = (nums) => {
  let maxDigitCount = mostDigits(nums)
  for (let k = 0; k < maxDigitCount; k++) {
    let digitBuckets = Array.from({ length: 10 }, () => [])
    for (let i = 0; i < nums.length; i++) {
      let digit = getDigit(nums[i], k)
      digitBuckets[digit].push(nums[i])
    }
    nums = [].concat(...digitBuckets)
  }
  return nums
}
```

# Data Structures

## Singly Linked List

A data structure that contains a head, tail, and length property. 
Linked List consist of nodes. Each node has  value an a pointer to another node or null.
Singly Linked List are an excellent alternative to arrays when insertion and deletion at the beginning are frequently required.
Insertion:  0(1)
Removal:    0(1) or (n)
Searching:  0(n)
Access:     0(n)

```js
class Node {
    constructor(val) {
        this.val = val
        this.next = null
    }
}

class SinglyLinkedList {
    constructor() {
        this.head = null
        this.tail = null
        this.length = 0
    }
    get(index) {
        if (index < 0 || index >= this.length) return null
        let counter = 0
        let current = this.head
        while (counter !== index) {
            current = current.next
            counter++
        }
        return current
    }
    insert(index, val) {
        if (index < 0 || index > this.length) return false
        if (index === this.length) return !!this.push(val)
        if (index === 0) return !!this.unshift(val)

        let newNode = new Node(val)
        let prev = this.get(index - 1)
        let temp = prev.next
        prev.next = newNode
        newNode.next = temp
        this.length++
        return true
    }
    pop() {
        if (!this.head) return undefined
        let current = this.head
        let newTail = current
        while (current.next) {
            newTail = current
            current = current.next
        }
        this.tail = newTail
        this.tail.next = null
        this.length--
        if (this.length === 0) {
            this.head = null
            this.tail = null
        }
        return current
    }
    print() {
        let arr = []
        let current = this.head
        while (current) {
            arr.push(current.val)
            current = current.next
        }
        console.log(arr)
    }
    push(val) {
        let newNode = new Node(val)
        if (!this.head) {
            this.head = newNode
            this.tail = this.head
        } else {
            this.tail.next = newNode
            this.tail = newNode
        }
        this.length++
        return this
    }
    remove(index) {
        if (index < 0 || index >= this.length) return undefined
        if (index === 0) return this.shift()
        if (index === this.length - 1) return this.pop()
        let previousNode = this.get(index - 1)
        let removed = previousNode.next
        previousNode.next = removed.next
        this.length--
        return removed
    }
    reverse() {
        let node = this.head
        this.head = this.tail
        this.tail = node
        let next
        let prev = null
        for (let i = 0; i < this.length; i++) {
            next = node.next
            node.next = prev
            prev = node
            node = next
        }
        return this
    }
    set(index, val) {
        let foundNode = this.get(index)
        if (foundNode) {
            foundNode.val = val
            return true
        }
        return false
    }
    shift() {
        if (!this.head) return undefined
        let currentHead = this.head
        this.head = currentHead.next
        this.length--
        if (this.length === 0) {
            this.tail = null
        }
        return currentHead
    }
    unshift(val) {
        let newNode = new Node(val)
        if (!this.head) {
            this.head = newNode
            this.tail = this.head
        }
        newNode.next = this.head
        this.head = newNode
        this.length++
        return this
    }
}
```

## Doubly Linked List

Almost identical to Singly Linked List, except every node has another pointer, to the previous node.
More memory === More flexibility
Better than Singly Linked List for finding nodes and can be done in half the time. But do take up more memory with extra pointer.
Insertion:  0(1)
Removal:    0(1)
Searching:  0(n)
Access:     0(n)
Technically searching is 0(n/2), but that is still o(n).

```js
class Node {
    constructor(val) {
        this.val = val
        this.next = null
        this.prev = null
    }
}

class DoublyLinkedList {
    constructor() {
        this.head = null
        this.tail = null
        this.length = 0
    }
    get(index) {
        if (index < 0 || index >= this.length) return null
        let count, current
        if (index <= this.length / 2) {
            count = 0
            current = this.head
            while (count !== index) {
                current = current.next
                count++
            }
        } else {
            count = this.length - 1
            current = this.tail
            while (count !== index) {
                current = current.prev
                count--
            }
        }
        return current
    }
    insert(index, val) {
        if (index < 0 || index > this.length) return false
        if (index === 0) return !!this.unshift(val)
        if (index === this.length) return !!this.push(val)
        let newNode = new Node(val)
        let beforeNode = this.get(index - 1)
        let afterNode = beforeNode.next
        beforeNode.next = newNode, newNode.prev = beforeNode
        newNode.next = afterNode, afterNode.prev = newNode
        this.length++
        return true
    }
    pop() {
        if (!this.head) return undefined
        let poppedNode = this.tail
        if (this.length === 1) {
            this.head = null
            this.tail = null
        } else {
            this.tail = poppedNode.prev
            this.tail.next = null
            poppedNode.prev = null
        }
        this.length--
        return poppedNode
    }
    push(val) {
        let newNode = new Node(val)
        if (this.length === 0) {
            this.head = newNode
            this.tail = newNode
        } else {
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
        }
        this.length++
        return this
    }
    remove(index) {
        if (index < 0 || index >= this.length) return undefined
        if (index === 0) return this.shift()
        if (index === this.length - 1) return this.pop()
        let removedNode = this.get(index)
        let beforeNode = removedNode.prev
        let afterNode = removedNode.next
        beforeNode.next = afterNode
        afterNode.prev = beforeNode
        removedNode.next = null
        removedNode.prev = null
        this.length--
        return removedNode
    }
    reverse() {
        let temp = null
        let current = this.head
        while (current) {
            temp = current.prev
            current.prev = current.next
            current.next = temp
            current = current.prev
        }
        if (temp != null) {
            this.head = temp.prev
        }
        return this
    }
    set(index, val) {
        let foundNode = this.get(index)
        if (foundNode != null) {
            foundNode.val = val
            return true
        }
        return false
    }
    shift() {
        if (this.length === 0) return undefined
        let oldHead = this.head
        if (this.length === 1) {
            this.head = null
            this.tail = null
        } else {
            this.head = oldHead.next
            this.head.prev = null
            oldHead.next = null
        }
        this.length--
        return oldHead
    }
    unshift(val) {
        let newNode = new Node(val)
        if (this.length === 0) {
            this.head = newNode
            this.tail = newNode
        } else {
            this.head.prev = newNode
            newNode.next = this.head
            this.head = newNode
        }
        this.length++
        return this
    }
}
```

## Stack

LIFO data structure. Last in first out.
Last element added will be the first element removed.
Used for function invocations, undo / redo, and for routing.
Insertion:  0(1)
Removal:    0(1)
Searching:  0(n)
Access:     0(n)

```js
class Node {
    constructor(value) {
        this.value = value
        this.next = null
    }
}

class Stack {
    constructor() {
        this.first = null
        this.last = null
        this.size = 0
    }
    pop() {
        if (!this.first) return null
        let temp = this.first
        if (this.first === this.last) {
            this.last = null
        }
        this.first = this.first.next
        this.size--
        return temp.value
    }
    push(val) {
        let newNode = new Node(val)
        if (!this.first) {
            this.first = newNode
            this.last = newNode
        } else {
            let temp = this.first
            this.first = newNode
            this.first.next = temp
        }
        return ++this.size
    }
}
```

## Queue

FIFO data structure. First in first out.
First element added will be the first element removed.
Used for processing task, uploading resources, and printing / task processing.
Insertion:  0(1)
Removal:    0(1)
Searching:  0(n)
Access:     0(n)

```js
class Node {
    constructor(value) {
        this.value = value
        this.next = null
    }
}

class Queue {
    constructor() {
        this.first = null
        this.last = null
        this.size = 0
    }
    dequeue() {
        if (!this.first) return null

        let temp = this.first
        if (this.first === this.last) {
            this.last = null
        }
        this.first = this.first.next
        this.size--
        return temp.value
    }
    enqueue(val) {
        let newNode = new Node(val)
        if (!this.first) {
            this.first = newNode
            this.last = newNode
        } else {
            this.last.next = newNode
            this.last = newNode
        }
        return ++this.size
    }
}
```

## Binary Search Tree

A data structure that consist of nodes in a parent / child relationship.
Usage examples: network routing, HTML DOM, abstract syntax tree, AI, folders in operating systems, and computer file systems.
Binary Search Tree every parent node can only have at most 2 children.
Every node to the left of a parent node is always less than the parent.
Every node to the right of a parent node is always greater than the parent.
Insertion:  0(log n)
Searching:  0(log n)
Not guaranteed.

```js
class Node {
    constructor(value) {
        this.value = value
        this.left = null
        this.right = null
    }
}

class BinarySearchTree {
    constructor() {
        this.root = null
    }
    contains(value) {
        if (this.root === null) return false
        let current = this.root,
            found = false
        while (current && !found) {
            if (value < current.value) {
                current = current.left
            } else if (value > current.value) {
                current = current.right
            } else {
                return true
            }
        }
        return false
    }
    find(value) {
        if (this.root === null) return false
        let current = this.root,
            found = false
        while (current && !found) {
            if (value < current.value) {
                current = current.left
            } else if (value > current.value) {
                current = current.right
            } else {
                found = true
            }
        }
        if (!found) return undefined
        return current
    }
    insert(value) {
        let newNode = new Node(value)
        if (this.root === null) {
            this.root = newNode
            return this
        }
        let current = this.root
        while (true) {
            if (value === current.value) return undefined
            if (value < current.value) {
                if (current.left === null) {
                    current.left = newNode
                    return this
                }
                current = current.left
            } else {
                if (current.right === null) {
                    current.right = newNode
                    return this
                }
                current = current.right
            }
        }
    }
    BFS() {
        let node = this.root,
            data = [],
            queue = []
        queue.push(node)
        while (queue.length) {
            node = queue.shift()
            data.push(node.value)
            if (node.left) queue.push(node.left)
            if (node.right) queue.push(node.right)
        }
        return data
    }
    DFSInOrder() {
        let data = []
        function traverse(node) {
            if (node.left) traverse(node.left)
            data.push(node.value)
            if (node.right) traverse(node.right)
        }
        traverse(this.root)
        return data
    }
    DFSPreOrder() {
        let data = []
        function traverse(node) {
            data.push(node.value)
            if (node.left) traverse(node.left)
            if (node.right) traverse(node.right)
        }
        traverse(this.root)
        return data
    }
    DFSPostOrder() {
        let data = []
        function traverse(node) {
            if (node.left) traverse(node.left)
            if (node.right) traverse(node.right)
            data.push(node.value)
        }
        traverse(this.root)
        return data
    }
}
```

## Binary Heap

Very similar to a binary search tree.
In a Max Binary Heap, parent nodes are always larger than child nodes. 
In a Min Binary Heap, parent nodes are always smaller than child nodes.
Binary Heaps are used to implement priority queues, which are very commonly used data structures.
Used quite a bit for graph traversal algorithms.
Insertion:  0(log n)
Removal:    0(log n)
Search:     0(n)

```js
// Max Binary Heap 
class MaxBinaryHeap {
    constructor(){
        this.values = [];
    }
    bubbleUp(){
        let idx = this.values.length - 1;
        const element = this.values[idx];
        while(idx > 0){
            let parentIdx = Math.floor((idx - 1)/2);
            let parent = this.values[parentIdx];
            if(element <= parent) break;
            this.values[parentIdx] = element;
            this.values[idx] = parent;
            idx = parentIdx;
        }
    }
    extractMax(){
        const max = this.values[0];
        const end = this.values.pop();
        if(this.values.length > 0){
            this.values[0] = end;
            this.sinkDown();
        }
        return max;
    }
    insert(element){
        this.values.push(element);
        this.bubbleUp();
    }
    sinkDown(){
        let idx = 0;
        const length = this.values.length;
        const element = this.values[0];
        while(true){
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let leftChild,rightChild;
            let swap = null;

            if(leftChildIdx < length){
                leftChild = this.values[leftChildIdx];
                if(leftChild > element) {
                    swap = leftChildIdx;
                }
            }
            if(rightChildIdx < length){
                rightChild = this.values[rightChildIdx];
                if(
                    (swap === null && rightChild > element) || 
                    (swap !== null && rightChild > leftChild)
                 ) {
                    swap = rightChildIdx;
                }
            }
            if (swap === null) break;
            this.values[idx] = this.values[swap];
            this.values[swap] = element;
            idx = swap;
        }
    }
}

// Priority Queue
class Node {
    constructor(val, priority) {
        this.val = val
        this.priority = priority
    }
}

class PriorityQueue {
    constructor() {
        this.values = []
    }
    bubbleUp() {
        let idx = this.values.length - 1
        const element = this.values[idx]
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2)
            let parent = this.values[parentIdx]
            if (element.priority >= parent.priority) break
            this.values[parentIdx] = element
            this.values[idx] = parent
            idx = parentIdx
        }
    }
    dequeue() {
        const min = this.values[0]
        const end = this.values.pop()
        if (this.values.length > 0) {
            this.values[0] = end
            this.sinkDown()
        }
        return min
    }
    enqueue(val, priority) {
        let newNode = new Node(val, priority)
        this.values.push(newNode)
        this.bubbleUp()
    }
    sinkDown() {
        let idx = 0
        const length = this.values.length
        const element = this.values[0]
        while (true) {
            let leftChildIdx = 2 * idx + 1
            let rightChildIdx = 2 * idx + 2
            let leftChild, rightChild
            let swap = null

            if (leftChildIdx < length) {
                leftChild = this.values[leftChildIdx]
                if (leftChild.priority < element.priority) {
                    swap = leftChildIdx
                }
            }
            if (rightChildIdx < length) {
                rightChild = this.values[rightChildIdx]
                if (
                    (swap === null && rightChild.priority < element.priority) ||
                    (swap !== null && rightChild.priority < leftChild.priority)
                ) {
                    swap = rightChildIdx
                }
            }
            if (swap === null) break
            this.values[idx] = this.values[swap]
            this.values[swap] = element
            idx = swap
        }
    }
}
```

## Hash Table

Hash tables are used to store key-value pairs.
They are like arrays, but the keys are not ordered.
Unlike arrays, hash tables are fast for all of the following operations: finding values, adding new values, an removing values.
A good hash should be fast, distribute keys uniformly, and be deterministic.
Separate chaining and linear probing are two strategies used to deal with two keys that hash to the same index.
Insertion:  0(1)
Deletion:   0(1)
Access:     0(1)
Average case.

```js
class HashTable {
  constructor(size = 53) {
    this.keyMap = new Array(size)
  }
  _hash(key) {
    let total = 0
    let WEIRD_PRIME = 31
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      let char = key[i]
      let value = char.charCodeAt(0) - 96
      total = (total * WEIRD_PRIME + value) % this.keyMap.length
    }
    return total
  }
  get(key) {
    let index = this._hash(key)
    if (this.keyMap[index]) {
      for (let i = 0; i < this.keyMap[index].length; i++) {
        if (this.keyMap[index][i][0] === key) {
          return this.keyMap[index][i][1]
        }
      }
    }
    return undefined
  }
  keys() {
    let keysArr = []
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
        for (let j = 0; j < this.keyMap[i].length; j++) {
          if (!keysArr.includes(this.keyMap[i][j][0])) {
            keysArr.push(this.keyMap[i][j][0])
          }
        }
      }
    }
    return keysArr
  }
  set(key, value) {
    let index = this._hash(key)
    if (!this.keyMap[index]) {
      this.keyMap[index] = []
    }
    this.keyMap[index].push([key, value])
  }
  values() {
    let valuesArr = []
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
        for (let j = 0; j < this.keyMap[i].length; j++) {
          if (!valuesArr.includes(this.keyMap[i][j][1])) {
            valuesArr.push(this.keyMap[i][j][1])
          }
        }
      }
    }
    return valuesArr
  }
}
```

## Graph

A graph data structure consists of a finite (and possibly mutable) set of vertices or nodes or points, together with a set of unordered pairs of these vertices for an undirected graph or a set of ordered pairs for a directed graph.
Uses for graphs: social networks, location / mapping, routing algorithms, visual hierarchy, file system optimizations
Vertex / Edge
Add V:      0(1)
Add E:      0(1)
Remove V:   0(|V| + |E|)
Remove E:   0(|E|)
Query:      0(|V| + |E|)
Storage:    0(|V| + |E|)

```js
class Graph {
    constructor() {
        this.adjacencyList = {}
    }
    addEdge(v1, v2) {
        this.adjacencyList[v1].push(v2)
        this.adjacencyList[v2].push(v1)
    }
    addVertex(vertex) {
        if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = []
    }
    removeEdge(vertex1, vertex2) {
        this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(
            v => v !== vertex2
        )
        this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(
            v => v !== vertex1
        )
    }
    removeVertex(vertex) {
        while (this.adjacencyList[vertex].length) {
            const adjacentVertex = this.adjacencyList[vertex].pop()
            this.removeEdge(vertex, adjacentVertex)
        }
        delete this.adjacencyList[vertex]
    }
    breadthFirst(start) {
        const queue = [start]
        const result = []
        const visited = {}
        let currentVertex
        visited[start] = true
        while (queue.length) {
            currentVertex = queue.shift()
            result.push(currentVertex)
            this.adjacencyList[currentVertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    visited[neighbor] = true
                    queue.push(neighbor)
                }
            })
        }
        return result
    }
    depthFirstIterative(start) {
        const stack = [start]
        const result = []
        const visited = {}
        let currentVertex
        visited[start] = true
        while (stack.length) {
            currentVertex = stack.pop()
            result.push(currentVertex)

            this.adjacencyList[currentVertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    visited[neighbor] = true
                    stack.push(neighbor)
                }
            })
        }
        return result
    }
    depthFirstRecursive(start) {
        const result = []
        const visited = {}
        const adjacencyList = this.adjacencyList;

        (function dfs(vertex) {
            if (!vertex) return null
            visited[vertex] = true
            result.push(vertex)
            adjacencyList[vertex].forEach(neighbor => {
                if (!visited[neighbor]) {
                    return dfs(neighbor)
                }
            })
        })(start)
        return result
    }
}
```

## Dijkstra's Algorithm

```js
class Node {
    constructor(val, priority) {
        this.val = val
        this.priority = priority
    }
}

class PriorityQueue {
    constructor() {
        this.values = []
    }
    bubbleUp() {
        let idx = this.values.length - 1
        const element = this.values[idx]
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2)
            let parent = this.values[parentIdx]
            if (element.priority >= parent.priority) break
            this.values[parentIdx] = element
            this.values[idx] = parent
            idx = parentIdx
        }
    }
    dequeue() {
        const min = this.values[0]
        const end = this.values.pop()
        if (this.values.length > 0) {
            this.values[0] = end
            this.sinkDown()
        }
        return min
    }
    enqueue(val, priority) {
        let newNode = new Node(val, priority)
        this.values.push(newNode)
        this.bubbleUp()
    }
    sinkDown() {
        let idx = 0
        const length = this.values.length
        const element = this.values[0]
        while (true) {
            let leftChildIdx = 2 * idx + 1
            let rightChildIdx = 2 * idx + 2
            let leftChild, rightChild
            let swap = null

            if (leftChildIdx < length) {
                leftChild = this.values[leftChildIdx]
                if (leftChild.priority < element.priority) {
                    swap = leftChildIdx
                }
            }
            if (rightChildIdx < length) {
                rightChild = this.values[rightChildIdx]
                if (
                    (swap === null && rightChild.priority < element.priority) ||
                    (swap !== null && rightChild.priority < leftChild.priority)
                ) {
                    swap = rightChildIdx
                }
            }
            if (swap === null) break
            this.values[idx] = this.values[swap]
            this.values[swap] = element
            idx = swap
        }
    }
}

class WeightedGraph {
    constructor() {
        this.adjacencyList = {}
    }
    addEdge(vertex1, vertex2, weight) {
        this.adjacencyList[vertex1].push({ node: vertex2, weight })
        this.adjacencyList[vertex2].push({ node: vertex1, weight })
    }
    addVertex(vertex) {
        if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = []
    }
    Dijkstra(start, finish) {
        const nodes = new PriorityQueue()
        const distances = {}
        const previous = {}
        let path = [] //to return at end
        let smallest
        //build up initial state
        for (let vertex in this.adjacencyList) {
            if (vertex === start) {
                distances[vertex] = 0
                nodes.enqueue(vertex, 0)
            } else {
                distances[vertex] = Infinity
                nodes.enqueue(vertex, Infinity)
            }
            previous[vertex] = null
        }
        // as long as there is something to visit
        while (nodes.values.length) {
            smallest = nodes.dequeue().val
            if (smallest === finish) {
                //WE ARE DONE
                //BUILD UP PATH TO RETURN AT END
                while (previous[smallest]) {
                    path.push(smallest)
                    smallest = previous[smallest]
                }
                break
            }
            if (smallest || distances[smallest] !== Infinity) {
                for (let neighbor in this.adjacencyList[smallest]) {
                    //find neighboring node
                    let nextNode = this.adjacencyList[smallest][neighbor]
                    //calculate new distance to neighboring node
                    let candidate = distances[smallest] + nextNode.weight
                    let nextNeighbor = nextNode.node
                    if (candidate < distances[nextNeighbor]) {
                        //updating new smallest distance to neighbor
                        distances[nextNeighbor] = candidate
                        //updating previous - How we got to neighbor
                        previous[nextNeighbor] = smallest
                        //enqueue in priority queue with new priority
                        nodes.enqueue(nextNeighbor, candidate)
                    }
                }
            }
        }
        return path.concat(smallest).reverse()
    }
}
```
