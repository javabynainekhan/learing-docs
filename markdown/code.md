# slidding window pattern in leetcode with js

The **sliding window pattern** is a commonly used technique in problems dealing with **arrays or strings** where you need to examine a **contiguous block** (or "window") of elements. It's especially useful for problems involving:

* Maximum/minimum in a subarray
* Sum of subarrays
* Longest/shortest substring with conditions

---

### 🚀 Key Idea

Instead of recalculating results for every possible subarray or substring (which is inefficient), we **slide a "window"** over the data, updating the result incrementally.

---

### 🔧 Template for Sliding Window (JavaScript)

```js
function slidingWindow(arr, windowSize) {
    let result = [];
    let windowSum = 0;
    let start = 0;

    for (let end = 0; end < arr.length; end++) {
        windowSum += arr[end];  // Add the next element

        // Slide the window when we hit the window size
        if (end >= windowSize - 1) {
            result.push(windowSum);
            windowSum -= arr[start];  // Remove the element going out
            start++;
        }
    }

    return result;
}
```

---

### 🧠 Common LeetCode Problems

Here are a few classic LeetCode problems using the sliding window pattern, with example JS solutions:

---

### 1. **Maximum Sum Subarray of Size K**

**Problem:** Given an array of integers, find the maximum sum of a subarray of size `k`.

```js
function maxSumSubarray(arr, k) {
    let maxSum = 0;
    let windowSum = 0;
    let start = 0;

    for (let end = 0; end < arr.length; end++) {
        windowSum += arr[end];

        if (end >= k - 1) {
            maxSum = Math.max(maxSum, windowSum);
            windowSum -= arr[start];
            start++;
        }
    }

    return maxSum;
}
```

---

### 2. **Longest Substring Without Repeating Characters**

LeetCode #3

```js
function lengthOfLongestSubstring(s) {
    let seen = new Set();
    let left = 0;
    let maxLength = 0;

    for (let right = 0; right < s.length; right++) {
        while (seen.has(s[right])) {
            seen.delete(s[left]);
            left++;
        }
        seen.add(s[right]);
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

---

### 3. **Minimum Size Subarray Sum**

LeetCode #209

```js
function minSubArrayLen(target, nums) {
    let left = 0;
    let sum = 0;
    let minLength = Infinity;

    for (let right = 0; right < nums.length; right++) {
        sum += nums[right];

        while (sum >= target) {
            minLength = Math.min(minLength, right - left + 1);
            sum -= nums[left];
            left++;
        }
    }

    return minLength === Infinity ? 0 : minLength;
}
```

---

### 📌 Tips

* Use **sets or maps** for unique or frequency-based constraints.
* Use **while loops inside for loops** when shrinking the window based on a condition.
* Always track the **start** and **end** pointers of the window.

---

## find the longest substring with k unique charactrts in a given string

To solve the problem of finding the **longest substring with exactly `k` unique characters** in a given string, we can use the **sliding window** approach with the help of a **hash map** or **hash set** to track the frequency of characters in the current window.

### Problem Breakdown:

1. We need to maintain a sliding window that contains **at most `k` unique characters**.
2. If the window exceeds `k` unique characters, we shrink the window from the left.
3. We update the result each time we encounter a window with exactly `k` unique characters.

### Steps:

1. Use two pointers: `left` and `right` to represent the sliding window.
2. Expand the window by moving `right` pointer.
3. Once the window contains more than `k` unique characters, move the `left` pointer to reduce the size of the window until it has `k` or fewer unique characters.
4. Keep track of the maximum length of any valid window with `k` unique characters.

### Solution in JavaScript:

```js
function longestSubstringWithKUnique(s, k) {
    if (k === 0) return 0;
    let left = 0;
    let maxLength = 0;
    let map = new Map();

    for (let right = 0; right < s.length; right++) {
        // Add the character at the 'right' pointer to the map
        map.set(s[right], (map.get(s[right]) || 0) + 1);

        // If we have more than 'k' unique characters, shrink the window
        while (map.size > k) {
            map.set(s[left], map.get(s[left]) - 1);
            if (map.get(s[left]) === 0) {
                map.delete(s[left]);
            }
            left++;
        }

        // Update the result if the window has exactly 'k' unique characters
        if (map.size === k) {
            maxLength = Math.max(maxLength, right - left + 1);
        }
    }

    return maxLength;
}
```

### Explanation:

1. **Initialization**:

   * We use a `map` to store the frequency of characters in the current window.
   * `left` is the starting point of the window, and `right` is the end.
   * `maxLength` stores the length of the longest valid window with exactly `k` unique characters.

2. **Sliding Window**:

   * For each character in the string (moving `right` pointer), we update the `map` with the frequency of the character.
   * If the number of unique characters (`map.size`) exceeds `k`, we increment the `left` pointer to shrink the window until it contains `k` or fewer unique characters.

3. **Track Maximum Length**:

   * If the window contains exactly `k` unique characters, we update the `maxLength`.

### Example:

```js
let s = "araaci";
let k = 2;
console.log(longestSubstringWithKUnique(s, k)); // Output: 4 ("araa")
```

### Time Complexity:

* **Time Complexity:** `O(N)` where `N` is the length of the string. Each character is processed at most twice (once when expanding the window and once when contracting it).
* **Space Complexity:** `O(K)` where `K` is the number of unique characters, which is the size of the map.

This solution efficiently solves the problem using the sliding window technique. Let me know if you need more examples or explanations!

# subset patterns

In algorithm design, **subset patterns** refer to problems or techniques where we need to generate or examine all possible subsets of a given set, array, or string. These types of problems often arise in combinatorics, dynamic programming, and backtracking.

### Key Approaches for Subset Problems:

1. **Iterative Method**: Build subsets by iterating through each element and deciding whether to include it or not.
2. **Backtracking**: Generate all subsets recursively, building one subset at a time and backtracking when necessary.
3. **Bit Manipulation**: Use binary numbers to represent subsets.

---

### 1. **Subsets (Power Set)**

The classic **power set** problem is to find all subsets of a given set.

#### Problem:

Given a set (or array), generate all possible subsets (including the empty set).

#### Example:

Input: `[1, 2, 3]`
Output: `[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]`

#### Solution (Iterative Method):

```js
function subsets(nums) {
    let result = [[]];  // Start with the empty set
    
    for (let num of nums) {
        let newSubsets = [];
        // For each existing subset, add the current number to form a new subset
        for (let subset of result) {
            newSubsets.push([...subset, num]);
        }
        result = result.concat(newSubsets);  // Add new subsets to result
    }
    
    return result;
}
```

#### Explanation:

* Start with the empty set `[]`.
* For each element in the input array, create new subsets by adding that element to all previously generated subsets.
* This approach iteratively builds the power set.

---

### 2. **Subsets II (Handling Duplicates)**

When the input array contains **duplicates**, we need to make sure that we don't generate duplicate subsets.

#### Problem:

Given an array of integers (which may contain duplicates), generate all possible subsets (without duplicates).

#### Example:

Input: `[1, 2, 2]`
Output: `[[], [1], [1, 2], [1, 2, 2], [2], [2, 2]]`

#### Solution (Backtracking):

```js
function subsetsWithDup(nums) {
    nums.sort((a, b) => a - b);  // Sort the array to handle duplicates
    
    let result = [];
    
    function backtrack(start, path) {
        result.push([...path]);  // Add the current subset (path)
        
        for (let i = start; i < nums.length; i++) {
            // Skip duplicates
            if (i > start && nums[i] === nums[i - 1]) continue;
            path.push(nums[i]);  // Include the number
            backtrack(i + 1, path);  // Recurse
            path.pop();  // Backtrack (remove the last number)
        }
    }
    
    backtrack(0, []);
    return result;
}
```

#### Explanation:

* We first **sort** the input array so that duplicates are adjacent to each other.
* In the `backtrack` function, we **skip duplicates** by checking if the current number is the same as the previous one, and only recurse on the unique choices.

---

### 3. **Subsets with a Given Sum (Knapsack Variation)**

We can also modify subset problems to find subsets that satisfy certain conditions, such as a sum constraint.

#### Problem:

Given a set of integers and a target sum, find all subsets that sum up to the target.

#### Example:

Input: `nums = [1, 2, 3]`, `target = 3`
Output: `[[1, 2], [3]]`

#### Solution (Backtracking):

```js
function subsetsWithTargetSum(nums, target) {
    let result = [];
    
    function backtrack(start, path, sum) {
        if (sum === target) {
            result.push([...path]);
            return;
        }
        
        if (sum > target) {
            return;
        }
        
        for (let i = start; i < nums.length; i++) {
            path.push(nums[i]);
            backtrack(i + 1, path, sum + nums[i]);  // Include current number
            path.pop();  // Backtrack
        }
    }
    
    backtrack(0, [], 0);
    return result;
}
```

#### Explanation:

* We use backtracking to try all subsets, but only add those whose sum equals the target.
* We stop exploring subsets whose sum exceeds the target.

---

### 4. **Bit Manipulation for Subsets**

Bit manipulation can also be used to generate subsets by treating the inclusion/exclusion of each element as a bit (1 for inclusion, 0 for exclusion).

#### Problem:

Given a set, generate all possible subsets using bit manipulation.

#### Solution:

```js
function subsets(nums) {
    let result = [];
    let n = nums.length;
    
    // Iterate over all possible binary numbers from 0 to (2^n - 1)
    for (let mask = 0; mask < (1 << n); mask++) {
        let subset = [];
        for (let i = 0; i < n; i++) {
            if (mask & (1 << i)) {  // Check if the ith bit is set
                subset.push(nums[i]);
            }
        }
        result.push(subset);
    }
    
    return result;
}
```

#### Explanation:

* We loop through all possible subsets, which can be represented by binary numbers from `0` to `(2^n - 1)`.
* For each bitmask, we check which elements are included by inspecting each bit.

---

### 5. **Subset Sum Problem**

This problem is a variation where we need to find if there is a subset that sums to a given target.

#### Problem:

Given a set of integers and a target sum, check if there is a subset whose sum is equal to the target.

#### Solution:

```js
function canPartition(nums, target) {
    let n = nums.length;
    let dp = new Array(target + 1).fill(false);
    dp[0] = true;  // Base case: 0 sum is always achievable (empty subset)
    
    for (let num of nums) {
        for (let i = target; i >= num; i--) {
            dp[i] = dp[i] || dp[i - num];
        }
    }
    
    return dp[target];
}
```

#### Explanation:

* We use dynamic programming to track whether a sum can be achieved using subsets.
* We iterate over the numbers, and for each number, we update possible sums starting from the target down to the number itself.

---

### Conclusion

Subset problems come in many flavors, and we use various techniques to solve them:

* **Iterative generation**: Great for simple power sets.
* **Backtracking**: Useful for generating subsets with specific constraints or avoiding duplicates.
* **Bit manipulation**: Efficient for generating subsets in cases where the number of elements is small (since `2^n` grows fast).
* **Dynamic programming**: Often used when there's a sum condition (e.g., subset sum problems).

If you'd like to dive into any specific pattern further or need more examples, feel free to ask!


# Permutations

### Permutations

In combinatorics, a **permutation** of a set is a particular arrangement of its elements in a specific order. For example, the permutations of the set `[1, 2, 3]` are:

```
[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]
```

A **permutation problem** typically involves generating all possible arrangements of a given set or sequence of elements.

### Key Approaches to Permutation Problems

1. **Backtracking**: The most common approach to generating all permutations, where we build permutations step by step and backtrack if needed.
2. **Iterative**: Using loops to generate permutations iteratively.
3. **Heap’s Algorithm**: A more optimized approach for generating permutations, especially for sequences of n elements.

---

### 1. **Generate All Permutations (Backtracking)**

Let's start with generating all **permutations of a list of numbers**. This problem is a classic example of recursion and backtracking.

#### Problem:

Given a list of numbers, generate all possible permutations of the list.

#### Example:

Input: `[1, 2, 3]`
Output: `[[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]`

#### Solution (Backtracking):

```js
function permute(nums) {
    let result = [];
    
    function backtrack(path) {
        // If the path has the same length as the original array, we've found a permutation
        if (path.length === nums.length) {
            result.push([...path]);
            return;
        }
        
        for (let i = 0; i < nums.length; i++) {
            // Skip if the element has already been used
            if (path.includes(nums[i])) continue;
            path.push(nums[i]);  // Choose the current element
            backtrack(path);      // Recurse
            path.pop();           // Backtrack (remove the last element)
        }
    }
    
    backtrack([]);
    return result;
}
```

#### Explanation:

1. **Base case**: Once the length of the `path` is equal to the length of the original array (`nums`), a full permutation has been generated, and we add it to the `result`.
2. **Recursive case**: For each element, if it hasn’t been used yet (checked by `path.includes(nums[i])`), we add it to the current `path` and recursively generate the next part of the permutation.
3. **Backtracking**: After generating permutations that include the current element, we remove it from the `path` (`path.pop()`), and continue to explore other possibilities.

---

### 2. **Generate All Permutations with Duplicates (Handling Duplicates)**

When there are **duplicate numbers** in the input array, we need to ensure that we don't generate duplicate permutations.

#### Problem:

Given an array that may contain duplicates, generate all unique permutations.

#### Example:

Input: `[1, 1, 2]`
Output: `[[1, 1, 2], [1, 2, 1], [2, 1, 1]]`

#### Solution (Backtracking with Duplicate Handling):

```js
function permuteUnique(nums) {
    let result = [];
    nums.sort((a, b) => a - b); // Sort the array to handle duplicates
    
    function backtrack(path, used) {
        if (path.length === nums.length) {
            result.push([...path]);
            return;
        }
        
        for (let i = 0; i < nums.length; i++) {
            if (used[i]) continue; // Skip used elements
            if (i > 0 && nums[i] === nums[i - 1] && !used[i - 1]) continue; // Skip duplicates
            
            used[i] = true;
            path.push(nums[i]);
            backtrack(path, used);
            path.pop();
            used[i] = false;
        }
    }
    
    backtrack([], Array(nums.length).fill(false));
    return result;
}
```

#### Explanation:

* **Sorting**: We sort the array to make it easier to skip duplicates.
* **Used Array**: We keep track of which elements have been used in the current permutation by using a `used` array.
* **Skipping Duplicates**: We skip duplicate elements by ensuring that we don’t use the same number twice in the same recursive step unless it’s the first occurrence (this is handled by the condition `if (i > 0 && nums[i] === nums[i - 1] && !used[i - 1])`).

---

### 3. **Generate Permutations Using Heap’s Algorithm**

Heap’s algorithm is an optimized way to generate permutations. It’s particularly efficient in terms of minimizing the number of swaps.

#### Problem:

Generate all permutations using Heap's algorithm.

#### Example:

Input: `[1, 2, 3]`
Output: `[[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]`

#### Solution (Heap’s Algorithm):

```js
function permuteHeap(nums) {
    let result = [];
    
    function heapPermute(n) {
        if (n === 1) {
            result.push([...nums]); // When there's only one element, push the current permutation
        } else {
            for (let i = 0; i < n; i++) {
                heapPermute(n - 1); // Recursively generate the rest of the permutations
                
                if (n % 2 === 0) {
                    [nums[i], nums[n - 1]] = [nums[n - 1], nums[i]]; // Swap elements
                } else {
                    [nums[0], nums[n - 1]] = [nums[n - 1], nums[0]]; // Swap elements
                }
            }
        }
    }
    
    heapPermute(nums.length);
    return result;
}
```

#### Explanation:

* **Heap’s Algorithm** uses a recursive approach where at each level, we swap the current element with another element, generate all permutations of the remaining elements, and then swap back.
* This algorithm is highly efficient because it minimizes the number of swaps.

---

### 4. **Permutations of a String**

We can also generate all permutations of a string using the same principles, but treating each character as an element in an array.

#### Problem:

Generate all permutations of a given string.

#### Example:

Input: `"abc"`
Output: `["abc", "acb", "bac", "bca", "cab", "cba"]`

#### Solution (Backtracking):

```js
function permuteString(str) {
    let result = [];
    
    function backtrack(path) {
        if (path.length === str.length) {
            result.push(path);
            return;
        }
        
        for (let i = 0; i < str.length; i++) {
            if (path.includes(str[i])) continue;  // Skip if the character is already used
            backtrack(path + str[i]); // Recurse with the updated path
        }
    }
    
    backtrack("");
    return result;
}
```

#### Explanation:

* This solution is very similar to the previous numeric permutation solutions, but instead of numbers, we're dealing with characters in the string.
* Each recursive step adds one character to the `path` string until the length of `path` matches the original string.

---

### Time and Space Complexity

* **Time Complexity**: The time complexity for generating all permutations is **O(n!)**, where `n` is the length of the input. This is because there are `n!` possible permutations of an `n`-length list or string.
* **Space Complexity**: The space complexity is **O(n!)** due to the storage of all possible permutations.

---

### Conclusion

* **Backtracking** is the most common technique for generating permutations, whether you're dealing with distinct elements or duplicates.
* **Heap’s Algorithm** is more efficient for generating permutations but is a bit more complex to implement.
* In most cases, especially when dealing with strings or arrays with duplicates, the **backtracking** approach is easier to understand and implement.

If you have any specific questions about permutations or want to explore further examples, feel free to ask!


## modified binary search patterns

### Modified Binary Search Patterns

Binary search is a fundamental algorithm often used for searching or finding elements in a sorted array. However, there are several **modified versions** of binary search, where the goal is not necessarily to find a specific element, but to find certain conditions or to optimize some criteria.

These modified patterns make use of the efficiency of binary search, often reducing time complexity from **O(n)** to **O(log n)** in the process.

Below are some common **modified binary search patterns** with examples:

---

### 1. **Finding the First Occurrence of a Target**

In some problems, you might need to find the **first occurrence** of a target element in a sorted array (including duplicates).

#### Problem:

Given a sorted array, find the index of the first occurrence of a target number.

#### Example:

Input: `nums = [1, 2, 2, 3, 3, 4, 5]`, `target = 3`
Output: `3` (the first occurrence of `3` is at index 3)

#### Solution (Modified Binary Search):

```js
function findFirstOccurrence(nums, target) {
    let left = 0, right = nums.length - 1, result = -1;
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            result = mid;  // Found target, but continue to find the first occurrence
            right = mid - 1;  // Move left to find earlier occurrences
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

#### Explanation:

* We continue searching even after finding the target to ensure we capture the **first occurrence** by moving `right` to `mid - 1` once we find the target.

---

### 2. **Finding the Last Occurrence of a Target**

This modification is the reverse of the previous one. Instead of finding the first occurrence, we find the **last occurrence** of a target element.

#### Problem:

Given a sorted array, find the index of the last occurrence of a target number.

#### Example:

Input: `nums = [1, 2, 2, 3, 3, 4, 5]`, `target = 3`
Output: `4` (the last occurrence of `3` is at index 4)

#### Solution (Modified Binary Search):

```js
function findLastOccurrence(nums, target) {
    let left = 0, right = nums.length - 1, result = -1;
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            result = mid;  // Found target, but continue to find the last occurrence
            left = mid + 1;  // Move right to find later occurrences
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

#### Explanation:

* After finding the target, we keep searching to the **right** (move `left = mid + 1`) to ensure we capture the **last occurrence** of the target.

---

### 3. **Finding the Smallest Element Greater Than or Equal to Target (Ceiling)**

This problem involves finding the smallest element that is greater than or equal to the target in a sorted array.

#### Problem:

Given a sorted array and a target, find the **smallest number greater than or equal** to the target.

#### Example:

Input: `nums = [1, 2, 8, 10, 10, 12]`, `target = 5`
Output: `8` (The smallest element greater than or equal to `5` is `8`)

#### Solution (Modified Binary Search):

```js
function findCeiling(nums, target) {
    let left = 0, right = nums.length - 1, result = -1;
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            return nums[mid];  // Target is exactly the ceiling
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            result = nums[mid];  // This is a potential ceiling
            right = mid - 1;
        }
    }
    
    return result;
}
```

#### Explanation:

* If the element is less than the target, we move `left` to `mid + 1`.
* If the element is greater than or equal to the target, we record it as the **ceiling** and move `right` to `mid - 1` to find a potentially smaller ceiling.

---

### 4. **Finding the Largest Element Less Than or Equal to Target (Floor)**

This problem is the inverse of the ceiling problem. We want to find the largest element that is less than or equal to the target.

#### Problem:

Given a sorted array and a target, find the **largest element less than or equal** to the target.

#### Example:

Input: `nums = [1, 2, 8, 10, 10, 12]`, `target = 9`
Output: `8` (The largest element less than or equal to `9` is `8`)

#### Solution (Modified Binary Search):

```js
function findFloor(nums, target) {
    let left = 0, right = nums.length - 1, result = -1;
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            return nums[mid];  // Target is exactly the floor
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            result = nums[mid];  // This is a potential floor
            left = mid + 1;
        }
    }
    
    return result;
}
```

#### Explanation:

* If the element is greater than the target, we move `right` to `mid - 1`.
* If the element is less than or equal to the target, we record it as the **floor** and move `left` to `mid + 1` to find a potentially larger floor.

---

### 5. **Finding the Insert Position (If Not Present)**

This pattern is often used to determine where an element would be inserted in a sorted array if it were not present.

#### Problem:

Given a sorted array and a target, return the index where the target should be inserted so that the array remains sorted.

#### Example:

Input: `nums = [1, 3, 5, 6]`, `target = 5`
Output: `2` (The target `5` is already at index `2`)

Input: `nums = [1, 3, 5, 6]`, `target = 2`
Output: `1` (The target `2` should be inserted at index `1`)

#### Solution (Modified Binary Search):

```js
function searchInsert(nums, target) {
    let left = 0, right = nums.length - 1;
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) {
            return mid;  // Target found, return the index
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return left;  // Left is the index where target should be inserted
}
```

#### Explanation:

* If the target is not found, we return the `left` pointer, which will indicate the position where the target should be inserted to maintain the sorted order.

---

### 6. **Finding the Rotation Point in a Rotated Sorted Array**

In this problem, the array has been rotated at some pivot, and you need to find the point at which the rotation occurs.

#### Problem:

Given a rotated sorted array, find the index of the **smallest element** (rotation point).

#### Example:

Input: `nums = [6, 7, 9, 15, 19, 2, 3]`
Output: `5` (The smallest element is `2` at index 5)

#### Solution (Modified Binary Search):

```js
function findMin(nums) {
    let left = 0, right = nums.length - 1;
    
    while (left < right) {
        let mid = Math.floor((left + right) / 2);
        
        if (nums[mid] > nums[right]) {
            left = mid + 1;  // Minimum must be to the right
        } else {
            right = mid;  // Minimum could be at mid or to the left
        }
    }
    
    return nums[left];  // The left pointer will be at the minimum
}
```

#### Explanation:

* The idea is that if the middle element is greater than the rightmost element, the minimum is on the **right side** of `mid`, otherwise, it’s on the **left side**.

---

### Conclusion:

Modified binary search techniques are powerful tools for solving a wide variety of problems efficiently. These patterns are especially useful when the problem involves


# search in rotated sorted array

### Search in Rotated Sorted Array

The **Rotated Sorted Array** is a sorted array that has been "rotated" at some unknown pivot. For example, `[6, 7, 9, 15, 19, 2, 3]` is a rotated version of `[2, 3, 6, 7, 9, 15, 19]`.

The goal is to find the **target value** in this rotated sorted array using **binary search**, achieving **O(log n)** time complexity.

### Approach

We can apply binary search by utilizing the properties of a rotated sorted array:

* A rotated sorted array consists of two sorted subarrays, one on the left and the other on the right of the pivot point.
* One of these subarrays will always be **sorted**, and the other will be **unsorted**.

By comparing the middle element with the left and right boundaries, we can decide which half of the array is sorted and which half contains the target.

---

### Steps:

1. **Find the middle element**: Calculate `mid` as usual.
2. **Check if the target is at `mid`**: If yes, return the index.
3. **Determine the sorted half**:

   * If `nums[left] <= nums[mid]`, the left half is sorted.
   * Otherwise, the right half is sorted.
4. **Narrow the search**:

   * If the left half is sorted, check if the target is within the range of `nums[left]` to `nums[mid]`. If so, adjust the `right` pointer. Otherwise, adjust the `left` pointer.
   * If the right half is sorted, check if the target is within the range of `nums[mid]` to `nums[right]`. If so, adjust the `left` pointer. Otherwise, adjust the `right` pointer.

---

### Solution (Binary Search in Rotated Sorted Array):

```js
function search(nums, target) {
    let left = 0, right = nums.length - 1;

    while (left <= right) {
        let mid = Math.floor((left + right) / 2);

        // Check if mid is the target
        if (nums[mid] === target) {
            return mid;
        }

        // Check if the left half is sorted
        if (nums[left] <= nums[mid]) {
            // Target lies within the left half
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1; // Target is in the left part
            } else {
                left = mid + 1;  // Target is in the right part
            }
        }
        // If the right half is sorted
        else {
            // Target lies within the right half
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1; // Target is in the right part
            } else {
                right = mid - 1; // Target is in the left part
            }
        }
    }

    return -1; // Target is not found
}
```

---

### Explanation:

1. **Initialization**:

   * `left` starts at the beginning of the array, and `right` starts at the end.
2. **While loop**:

   * The loop continues as long as `left <= right`.
3. **Binary Search Check**:

   * The middle element (`mid`) is calculated.
   * If `nums[mid] === target`, return `mid` immediately.
4. **Left Sorted Half**:

   * If `nums[left] <= nums[mid]`, the left half is sorted. We check if the target is between `nums[left]` and `nums[mid]`. If yes, move the `right` pointer to `mid - 1`; otherwise, move the `left` pointer to `mid + 1`.
5. **Right Sorted Half**:

   * If `nums[mid] < nums[right]`, the right half is sorted. We check if the target is between `nums[mid]` and `nums[right]`. If yes, move the `left` pointer to `mid + 1`; otherwise, move the `right` pointer to `mid - 1`.
6. **End of Search**:

   * If the loop finishes and no element is found, return `-1` indicating that the target is not in the array.

---

### Time and Space Complexity:

* **Time Complexity**: `O(log n)` — because we are halving the search space at each step, similar to regular binary search.
* **Space Complexity**: `O(1)` — we are only using a few extra variables for `left`, `right`, and `mid`, so the space complexity is constant.

---

### Example Walkthrough

#### Input:

```js
let nums = [6, 7, 9, 15, 19, 2, 3];
let target = 3;
```

1. **First iteration**:

   * `left = 0`, `right = 6`, `mid = 3` (`nums[mid] = 15`).
   * Since `nums[left] = 6 <= nums[mid] = 15`, the left half is sorted.
   * Since the target (`3`) is less than `nums[mid] = 15` and greater than `nums[left] = 6`, we move `right = mid - 1 = 2`.

2. **Second iteration**:

   * `left = 0`, `right = 2`, `mid = 1` (`nums[mid] = 7`).
   * Since `nums[left] = 6 <= nums[mid] = 7`, the left half is sorted.
   * Since the target (`3`) is less than `nums[mid] = 7`, we move `right = mid - 1 = 0`.

3. **Third iteration**:

   * `left = 0`, `right = 0`, `mid = 0` (`nums[mid] = 6`).
   * Since `nums[left] = nums[mid] = 6`, we move `left = mid + 1 = 1`, and the loop ends.

### Conclusion:

* In the above example, we correctly find that the target (`3`) is not in the array and return `-1`.

If you have any further questions or need additional clarification, feel free to ask!

# bisect - Array bisection algorithm  

### Bisect Algorithm (Array Bisection)

The **bisect algorithm** is commonly used to perform binary search in sorted arrays or to **insert elements into a sorted array** while maintaining the sorted order. It helps find positions in an array using binary search principles.

In Python, the `bisect` module provides functions like `bisect_left`, `bisect_right`, and `insort` to perform array bisection operations.

However, in a more generic sense, the **bisect algorithm** refers to using **binary search** to efficiently find specific positions in a sorted array.

We can implement this functionality in JavaScript and explain it in a bit more detail.

### Common Bisect Operations:

1. **`bisect_left`**: This function finds the **leftmost** position where the target value can be inserted while maintaining the sorted order.
2. **`bisect_right`**: This function finds the **rightmost** position where the target value can be inserted while maintaining the sorted order.
3. **`insort`**: This inserts an element into the array at the correct position while keeping the array sorted.

### Implementation of Bisect Algorithm

#### 1. **Bisect Left**

Finds the position where an element should be inserted to maintain the order, **in the leftmost position**.

```js
function bisectLeft(arr, target) {
    let left = 0, right = arr.length;

    while (left < right) {
        let mid = Math.floor((left + right) / 2);
        if (arr[mid] < target) {
            left = mid + 1; // Move to the right side
        } else {
            right = mid; // Move to the left side
        }
    }

    return left; // The position where the target can be inserted
}
```

#### 2. **Bisect Right**

Finds the position where an element should be inserted to maintain the order, **in the rightmost position**.

```js
function bisectRight(arr, target) {
    let left = 0, right = arr.length;

    while (left < right) {
        let mid = Math.floor((left + right) / 2);
        if (arr[mid] <= target) {
            left = mid + 1; // Move to the right side
        } else {
            right = mid; // Move to the left side
        }
    }

    return left; // The position where the target can be inserted
}
```

#### 3. **Inserting Using Bisect**

If you want to insert a new element into the array while maintaining its sorted order, you can combine the **bisect** function with an insertion operation. Here's an implementation of `insort` (insertion sort using bisect):

```js
function insort(arr, target) {
    let position = bisectLeft(arr, target); // Find the position to insert
    arr.splice(position, 0, target); // Insert the element at the found position
}
```

### Examples:

#### Example 1: **Using `bisectLeft`**

```js
let arr = [1, 3, 3, 5, 6];
let target = 3;

let position = bisectLeft(arr, target);
console.log(position); // Output: 1 (index of the leftmost 3)
```

#### Example 2: **Using `bisectRight`**

```js
let arr = [1, 3, 3, 5, 6];
let target = 3;

let position = bisectRight(arr, target);
console.log(position); // Output: 3 (index where 3 should be inserted at the rightmost position)
```

#### Example 3: **Inserting with `insort`**

```js
let arr = [1, 3, 3, 5, 6];
let target = 4;

insort(arr, target);
console.log(arr); // Output: [1, 3, 3, 4, 5, 6] (target inserted in sorted order)
```

---

### Explanation of the Algorithm:

1. **`bisectLeft(arr, target)`**:

   * This function finds the index where `target` can be inserted, such that all elements before this position are **less than or equal** to the target. The function stops when the `left` pointer exceeds the `right` pointer, and at this point, `left` will be the index where the element can be inserted.

2. **`bisectRight(arr, target)`**:

   * Similar to `bisectLeft`, but this finds the rightmost index where `target` can be inserted such that all elements before this index are **less than** or **equal to** the target. This function is useful when you want to handle duplicates and find the position where the next occurrence of the target should be inserted.

3. **Insertion (`insort`)**:

   * After finding the correct position using either `bisectLeft` or `bisectRight`, you can insert the target into the array at that position while keeping it sorted.

---

### Time Complexity:

* **Binary Search (bisectLeft, bisectRight)**:

  * **Time Complexity**: `O(log n)` because we're performing binary search in a sorted array, which splits the array in half each time.
  * **Space Complexity**: `O(1)` because we're using constant space for the search.

* **Insertion (insort)**:

  * **Time Complexity**: `O(n)` because the `splice` operation may take linear time in the worst case.
  * **Space Complexity**: `O(1)` as no extra space is used besides the input array.

---

### Applications of Bisect Algorithm:

1. **Searching for an Element**: Efficiently finding the index of a target element in a sorted array.
2. **Insertion of Elements**: Inserting elements into a sorted array while maintaining the order.
3. **Range Queries**: Finding the range where certain values fall (e.g., finding elements greater than a specific target or less than another).
4. **Solving Problems with Sorted Arrays**: Many problems like "finding the first/last occurrence of an element," "finding the minimum/maximum greater than a target," etc., can be solved efficiently using bisect-like operations.

---

### Conclusion:

The **bisect algorithm** is a powerful tool for working with sorted arrays. It leverages binary search principles to find appropriate positions for insertion or to find a target value quickly. Understanding these principles is crucial when working with sorted collections in algorithms or data structures, particularly when aiming for efficient solutions.

Let me know if you need any further clarification or examples!


## top k elements patterns

### Top K Elements Patterns

The **Top K Elements** problem is a common type of problem where the goal is to find the **k largest (or smallest) elements** from a collection of data. These problems are frequently encountered in a variety of contexts, such as when dealing with large datasets, streaming data, or when optimizing search queries.

There are several approaches to solving these types of problems efficiently. These approaches utilize a combination of data structures, such algorithms like **Heaps**, **Sorting**, **Quickselect**, and **Bucket Sort**.

---

### 1. **Using a Max Heap / Min Heap**

Heaps are often used for problems involving the **top K elements** because they allow us to maintain the top K elements in a very efficient manner, with **O(log K)** time complexity for insertions and deletions.

* **Max Heap**: If you are looking for the **K largest elements** from a collection, you can use a **min-heap** of size K.
* **Min Heap**: If you are looking for the **K smallest elements** from a collection, you can use a **max-heap** of size K.

#### Approach (Using a Min-Heap to find the K largest elements):

1. Maintain a min-heap of size K.
2. Traverse through the list and insert the current element into the heap.
3. If the size of the heap exceeds K, remove the smallest element (this keeps only the K largest elements in the heap).

**Time Complexity**:

* **Building the heap**: `O(n log K)`
* **Overall Complexity**: `O(n log K)` for finding top K elements

```js
function findTopK(nums, k) {
    const minHeap = new MinHeap();
    
    for (let num of nums) {
        minHeap.push(num);
        if (minHeap.size() > k) {
            minHeap.pop();  // Remove smallest element to keep only K largest
        }
    }
    
    return minHeap.toArray();  // Return top K elements in the heap
}

class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    size() {
        return this.heap.length;
    }
    
    push(val) {
        this.heap.push(val);
        this.bubbleUp();
    }
    
    pop() {
        if (this.size() <= 1) return this.heap.pop();
        const root = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.bubbleDown();
        return root;
    }
    
    bubbleUp() {
        let idx = this.heap.length - 1;
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2);
            if (this.heap[parentIdx] <= this.heap[idx]) break;
            [this.heap[parentIdx], this.heap[idx]] = [this.heap[idx], this.heap[parentIdx]];
            idx = parentIdx;
        }
    }
    
    bubbleDown() {
        let idx = 0;
        const length = this.heap.length;
        const element = this.heap[0];
        
        while (true) {
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let swap = null;
            
            if (leftChildIdx < length && this.heap[leftChildIdx] < element) {
                swap = leftChildIdx;
            }
            if (rightChildIdx < length && this.heap[rightChildIdx] < (swap === null ? element : this.heap[leftChildIdx])) {
                swap = rightChildIdx;
            }
            if (swap === null) break;
            this.heap[idx] = this.heap[swap];
            this.heap[swap] = element;
            idx = swap;
        }
    }
    
    toArray() {
        return this.heap;
    }
}
```

### 2. **Quickselect Algorithm (Partition-based Selection)**

**Quickselect** is an efficient algorithm to find the **Kth largest/smallest** element in an unordered list. It’s a modified version of **Quicksort**, but instead of sorting the entire array, it partitions the array around a pivot and recursively searches for the **Kth** element.

* **Average time complexity**: `O(n)`
* **Worst-case time complexity**: `O(n^2)` (though this is rare, typically it can be reduced with good pivot selection)

This can be extended to find the **Top K largest** elements by performing **Quickselect** to partition the array around the **Kth largest element**, and then returning the **top K elements** from the array.

```js
function quickSelect(nums, left, right, k) {
    if (left === right) return nums[left];
    
    let pivotIndex = partition(nums, left, right);
    
    if (k === pivotIndex) return nums[k];
    else if (k < pivotIndex) return quickSelect(nums, left, pivotIndex - 1, k);
    else return quickSelect(nums, pivotIndex + 1, right, k);
}

function partition(nums, left, right) {
    let pivot = nums[right];
    let i = left;
    
    for (let j = left; j < right; j++) {
        if (nums[j] < pivot) {
            [nums[i], nums[j]] = [nums[j], nums[i]];
            i++;
        }
    }
    
    [nums[i], nums[right]] = [nums[right], nums[i]];
    return i;
}

function findTopK(nums, k) {
    let n = nums.length;
    let pivotIndex = n - k; // k-th largest element is at position (n - k)
    
    quickSelect(nums, 0, n - 1, pivotIndex);
    return nums.slice(pivotIndex); // Slice the array to get top K elements
}
```

### 3. **Sorting Approach**

If the input size is small or if the top K elements are required from a **static array**, **sorting** can be the most straightforward approach. Simply **sort** the array and return the first `K` elements.

* **Time complexity**: `O(n log n)` due to sorting the entire array.
* This method is often less efficient than others when working with very large datasets, but it’s simple and works well when K is close to the size of the array.

```js
function findTopK(nums, k) {
    nums.sort((a, b) => b - a);  // Sort in descending order
    return nums.slice(0, k);      // Return the first K elements
}
```

### 4. **Bucket Sort / Counting Sort (for Range of Elements)**

When the elements have a known, limited range, a **Bucket Sort** or **Counting Sort** approach can be very efficient.

#### Bucket Sort (Finding the Top K Largest Elements):

1. **Bucket Sort** works well when the input has a **known range** (for example, integers between `1` and `1000`).
2. We can distribute the numbers into **buckets**, then iterate through the buckets in reverse order to extract the **top K elements**.

**Time complexity**:

* **Best case**: `O(n + k)` where `n` is the number of elements and `k` is the range of numbers.
* **Worst case**: `O(n log n)` if sorting is required for large buckets.

```js
function findTopK(nums, k) {
    let max = Math.max(...nums);
    let min = Math.min(...nums);
    let range = max - min + 1;
    
    // Create and initialize count array
    let count = Array(range).fill(0);
    
    // Count frequency of each element
    for (let num of nums) {
        count[num - min]++;
    }
    
    // Find top K elements
    let result = [];
    for (let i = range - 1; i >= 0; i--) {
        while (count[i] > 0 && result.length < k) {
            result.push(i + min);
            count[i]--;
        }
    }
    
    return result;
}
```

### 5. **Heap + Bucket Sorting (Hybrid Approach)**

For **streaming** or **online algorithms**, where the dataset is too large to fit in memory, you might combine **heap** with **bucket sorting** or another technique for managing the stream efficiently.

---

### Summary of Approaches:

1. **Heap (Min or Max Heap)**:

   * Efficient when needing to maintain a small number of top elements.
   * Time Complexity: `O(n log K)` for finding top K elements.

2. **Quickselect**:

   * Great for finding the **Kth largest/smallest** element efficiently without needing to sort the entire array.
   * Average Time Complexity: `O(n)`.

3. **Sorting**:

   * Simple and effective for small arrays or when K is large.
   * Time Complexity: `O(n log n)`.

4. **Bucket Sort/Counting Sort**:

   * Best for problems with **bounded integers**.
   * Time Complexity: `O(n + k)` or `O(n log n)`.

Each method has its use cases depending on the problem, the size of the dataset, and constraints like memory and time.

Let me know if you'd like further clarification or examples for any of these methods!


## given an integer array nums and an intiger k, returns the kth largest element in the array.
note that it is the kth largest element in the sorted order, not the kth distinct element.
can you solve it without sorting?

Yes, it’s possible to solve the problem of finding the **kth largest element** in an array **without sorting**. One of the most efficient ways to do this is by using a **min-heap** (or priority queue).

### Approach: Min-Heap

A **min-heap** is a binary heap where the smallest element is at the root. For the problem of finding the **kth largest element**, we can maintain a **min-heap of size k**.

### Steps:

1. **Min-Heap**: Use a min-heap of size `k`. The heap will store the largest `k` elements seen so far. If the heap exceeds size `k`, we pop the smallest element.
2. **Iterate over the array**:

   * For each element in the array:

     * If the heap has fewer than `k` elements, push the current element onto the heap.
     * If the heap already has `k` elements, compare the current element with the smallest element in the heap (which is at the root of the min-heap):

       * If the current element is greater than the smallest element in the heap, replace the smallest element with the current element.
3. After processing all elements, the root of the heap will contain the `kth` largest element.

### Time Complexity:

* **Inserting into the heap** takes `O(log k)`.
* We perform `n` insertions (one for each element in the array).
* Thus, the overall time complexity is **O(n log k)**, where `n` is the length of the array.

### Space Complexity:

* The space complexity is **O(k)** because we only store `k` elements in the heap.

### Code Implementation:

```js
function findKthLargest(nums, k) {
    const minHeap = new MinHeap();

    for (let num of nums) {
        minHeap.push(num);
        if (minHeap.size() > k) {
            minHeap.pop();  // Remove the smallest element to maintain size k
        }
    }

    return minHeap.peek(); // The root of the heap will be the kth largest element
}

class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    size() {
        return this.heap.length;
    }
    
    push(val) {
        this.heap.push(val);
        this.bubbleUp();
    }
    
    pop() {
        if (this.size() <= 1) return this.heap.pop();
        const root = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.bubbleDown();
        return root;
    }
    
    peek() {
        return this.heap[0];  // Root of the heap
    }

    bubbleUp() {
        let idx = this.heap.length - 1;
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2);
            if (this.heap[parentIdx] <= this.heap[idx]) break;
            [this.heap[parentIdx], this.heap[idx]] = [this.heap[idx], this.heap[parentIdx]];
            idx = parentIdx;
        }
    }

    bubbleDown() {
        let idx = 0;
        const length = this.heap.length;
        const element = this.heap[0];
        
        while (true) {
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let swap = null;
            
            if (leftChildIdx < length && this.heap[leftChildIdx] < element) {
                swap = leftChildIdx;
            }
            if (rightChildIdx < length && this.heap[rightChildIdx] < (swap === null ? element : this.heap[leftChildIdx])) {
                swap = rightChildIdx;
            }
            if (swap === null) break;
            this.heap[idx] = this.heap[swap];
            this.heap[swap] = element;
            idx = swap;
        }
    }
}
```

### Explanation of the Code:

1. **MinHeap Class**:

   * We use a **min-heap** to keep track of the largest `k` elements.
   * The `push` method inserts elements into the heap and then "bubbles" them up to ensure the heap property is maintained.
   * The `pop` method removes the smallest element (root of the heap), and the `bubbleDown` method is used to restore the heap property.
   * The `peek` method returns the root of the heap, which will be the smallest element in the heap (the `kth` largest element in the array after processing all elements).

2. **findKthLargest Function**:

   * We iterate through the array and insert each element into the heap.
   * If the heap exceeds size `k`, we remove the smallest element.
   * After all elements have been processed, the root of the heap will be the `kth` largest element.

### Example:

#### Input:

```js
let nums = [3, 2, 1, 5, 6, 4];
let k = 2;

console.log(findKthLargest(nums, k));  // Output: 5
```

### Conclusion:

* This approach does not require sorting the entire array, making it much more efficient for large arrays when `k` is much smaller than `n`.
* The time complexity is **O(n log k)**, and the space complexity is **O(k)**, which is a significant improvement over sorting the entire array, which would have a time complexity of **O(n log n)**.

Let me know if you need further clarification or additional explanations!


# Binary Tree DFS

### Depth-First Search (DFS) on a Binary Tree

**Depth-First Search (DFS)** is a classic graph traversal algorithm. In the context of a **binary tree**, DFS explores as far as possible along each branch before backtracking. DFS is usually implemented using either **recursion** or an **explicit stack**.

In a binary tree, DFS can be applied in three common ways, which are defined by the order in which nodes are visited:

1. **Preorder Traversal** (Root -> Left -> Right)
2. **Inorder Traversal** (Left -> Root -> Right)
3. **Postorder Traversal** (Left -> Right -> Root)

### DFS Traversal Types Explained:

1. **Preorder Traversal**:
   In this traversal, the root node is visited first, followed by the left subtree and then the right subtree.

2. **Inorder Traversal**:
   In this traversal, the left subtree is visited first, then the root node, and then the right subtree. This is commonly used for binary search trees (BST) because it returns the nodes in sorted order.

3. **Postorder Traversal**:
   In this traversal, the left subtree is visited first, then the right subtree, and finally the root node.

---

### Binary Tree DFS in JavaScript

Let’s define a simple binary tree and implement DFS in all three traversal orders using **recursion**.

#### Tree Node Structure:

```js
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

### 1. **Preorder Traversal (Root -> Left -> Right)**

In **Preorder**, we visit the root first, then the left subtree, and finally the right subtree.

```js
function preorderTraversal(root) {
    if (root === null) return [];
    
    // Visit root, then left, then right
    return [root.val, ...preorderTraversal(root.left), ...preorderTraversal(root.right)];
}
```

#### Example:

```js
let root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);

console.log(preorderTraversal(root));  // Output: [1, 2, 4, 5, 3]
```

### 2. **Inorder Traversal (Left -> Root -> Right)**

In **Inorder**, we first visit the left subtree, then the root, and finally the right subtree. This is especially useful for binary search trees because it returns the nodes in sorted order.

```js
function inorderTraversal(root) {
    if (root === null) return [];
    
    // Visit left, then root, then right
    return [...inorderTraversal(root.left), root.val, ...inorderTraversal(root.right)];
}
```

#### Example:

```js
console.log(inorderTraversal(root));  // Output: [4, 2, 5, 1, 3]
```

### 3. **Postorder Traversal (Left -> Right -> Root)**

In **Postorder**, we first visit the left subtree, then the right subtree, and finally the root node.

```js
function postorderTraversal(root) {
    if (root === null) return [];
    
    // Visit left, then right, then root
    return [...postorderTraversal(root.left), ...postorderTraversal(root.right), root.val];
}
```

#### Example:

```js
console.log(postorderTraversal(root));  // Output: [4, 5, 2, 3, 1]
```

### 4. **DFS Using Stack (Iterative Approach)**

Although recursion is the natural way to implement DFS, sometimes an **explicit stack** can be used to perform DFS iteratively.

#### Preorder Traversal Using Stack:

```js
function preorderTraversalIterative(root) {
    if (root === null) return [];
    
    const stack = [root];
    const result = [];
    
    while (stack.length > 0) {
        const node = stack.pop();
        result.push(node.val);
        
        // Right child is pushed first so left child is processed first
        if (node.right) stack.push(node.right);
        if (node.left) stack.push(node.left);
    }
    
    return result;
}
```

#### Inorder Traversal Using Stack:

```js
function inorderTraversalIterative(root) {
    const result = [];
    const stack = [];
    let current = root;
    
    while (current !== null || stack.length > 0) {
        while (current !== null) {
            stack.push(current);
            current = current.left;
        }
        
        current = stack.pop();
        result.push(current.val);
        current = current.right;
    }
    
    return result;
}
```

#### Postorder Traversal Using Stack:

Postorder traversal is a bit more complicated with an iterative approach. One way to do it is to simulate the postorder traversal using two stacks.

```js
function postorderTraversalIterative(root) {
    if (root === null) return [];
    
    const result = [];
    const stack = [root];
    const visited = [];
    
    while (stack.length > 0) {
        const node = stack.pop();
        visited.push(node);
        
        // Push left and right children to the stack
        if (node.left) stack.push(node.left);
        if (node.right) stack.push(node.right);
    }
    
    // Reverse the visited nodes to get postorder
    while (visited.length > 0) {
        result.push(visited.pop().val);
    }
    
    return result;
}
```

---

### Example Tree for Testing:

```js
let root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);
```

#### Example Outputs:

```js
// Recursive DFS Traversals:
console.log(preorderTraversal(root));      // Output: [1, 2, 4, 5, 3]
console.log(inorderTraversal(root));       // Output: [4, 2, 5, 1, 3]
console.log(postorderTraversal(root));     // Output: [4, 5, 2, 3, 1]

// Iterative DFS Traversals:
console.log(preorderTraversalIterative(root));   // Output: [1, 2, 4, 5, 3]
console.log(inorderTraversalIterative(root));    // Output: [4, 2, 5, 1, 3]
console.log(postorderTraversalIterative(root));  // Output: [4, 5, 2, 3, 1]
```

---

### Summary:

* **Preorder Traversal (Root -> Left -> Right)**: Visit root first, then left subtree, then right subtree.
* **Inorder Traversal (Left -> Root -> Right)**: Visit left subtree first, then root, then right subtree. This traversal is often used for binary search trees to return the nodes in sorted order.
* **Postorder Traversal (Left -> Right -> Root)**: Visit left subtree first, then right subtree, then root.

Both **recursive** and **iterative** methods (using a stack) can be used to perform DFS in a binary tree. The recursive approach is simpler and more intuitive, while the iterative approach can sometimes be more space-efficient.

Let me know if you need further explanations or clarifications!

# maximum depth of binary tree

### Maximum Depth of a Binary Tree

The **maximum depth** (or height) of a binary tree is defined as the length of the longest path from the **root node** to a **leaf node**. A **leaf node** is a node with no children.

The depth of a tree is measured by counting the number of edges along the path from the root to the farthest leaf node. If the tree is empty (i.e., the root is `null`), the depth is `0`.

#### Example:

For a tree like this:

```
       1
     /   \
    2     3
   / \
  4   5
```

The **maximum depth** of the tree is `3` because the longest path is from the root (1) to leaf node (4 or 5), which involves 3 edges.

### Approach to Find Maximum Depth

To find the maximum depth, we can use a **Depth-First Search (DFS)** approach, which can be implemented recursively or iteratively.

1. **Recursively**:

   * For each node, calculate the depth of the left and right subtrees.
   * The maximum depth at the current node will be the larger of the two subtree depths, plus `1` (to account for the current node).

2. **Base Case**:

   * If the current node is `null`, return a depth of `0`.

3. **Recursive Case**:

   * The depth of a node is `1 + max(depth of left subtree, depth of right subtree)`.

### Code Implementation (Recursive Approach):

#### JavaScript Code:

```js
// Definition for a binary tree node
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

// Function to find the maximum depth of a binary tree
function maxDepth(root) {
    if (root === null) {
        return 0; // If the tree is empty, the depth is 0
    }
    
    // Recursively find the depth of left and right subtrees, then return the maximum of the two
    const leftDepth = maxDepth(root.left);
    const rightDepth = maxDepth(root.right);
    
    // The maximum depth is the larger of the two subtrees' depths + 1 for the current node
    return Math.max(leftDepth, rightDepth) + 1;
}
```

### Explanation of the Code:

* **TreeNode Class**: Defines the structure of a tree node with a value (`val`), a left child (`left`), and a right child (`right`).
* **maxDepth Function**:

  * If the `root` is `null`, the function returns `0` because an empty tree has a depth of `0`.
  * Recursively calculates the depth of the left and right subtrees by calling `maxDepth` on `root.left` and `root.right`.
  * The depth of the current node is `1 + max(leftDepth, rightDepth)`, where `1` accounts for the current node.

### Example Usage:

```js
// Create the binary tree:
let root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);

console.log(maxDepth(root)); // Output: 3
```

### Step-by-step Calculation:

For the tree:

```
       1
     /   \
    2     3
   / \
  4   5
```

* The depth of node `4` and node `5` is `1` (both are leaf nodes).
* The depth of node `2` is `2` because `max(1, 1) + 1 = 2`.
* The depth of node `3` is `1` (it's a leaf node).
* The depth of the root node `1` is `3` because `max(2, 1) + 1 = 3`.

Thus, the **maximum depth** is `3`.

---

### Iterative Approach Using BFS (Breadth-First Search)

An **iterative approach** can also be used to find the maximum depth, which is typically done using **level-order traversal** (BFS). In this approach, we traverse each level of the tree, counting the number of levels.

### Code Implementation (Iterative Approach):

```js
function maxDepthBFS(root) {
    if (root === null) {
        return 0; // If the tree is empty, the depth is 0
    }
    
    const queue = [root];
    let depth = 0;
    
    // Perform level-order traversal (BFS)
    while (queue.length > 0) {
        let levelSize = queue.length;
        depth++;
        
        // Process all nodes at the current level
        for (let i = 0; i < levelSize; i++) {
            let node = queue.shift();
            
            // Add left and right children to the queue
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
    }
    
    return depth;
}
```

### Explanation of the Code:

* **Queue**: We use a queue to perform a **level-order traversal**. The queue holds nodes at each level, and we process them level by level.
* **Level Counting**: For each level, we increment the `depth` variable and process all the nodes at that level by pushing their children into the queue.

### Example Usage:

```js
console.log(maxDepthBFS(root)); // Output: 3
```

### Time and Space Complexity:

* **Time Complexity**:

  * **Recursive DFS**: `O(n)`, where `n` is the number of nodes in the tree, since each node is visited exactly once.
  * **BFS**: `O(n)`, since we process each node once during the level-order traversal.
* **Space Complexity**:

  * **Recursive DFS**: `O(h)`, where `h` is the height of the tree (due to the recursion stack).
  * **BFS**: `O(n)` in the worst case (e.g., for a completely unbalanced tree) due to the queue storing all nodes at the bottom level.

---

### Conclusion:

* **DFS (Recursive)** is simple to implement and works well for most binary trees.
* **BFS (Iterative)** uses a queue to traverse the tree level by level and can be useful if you're dealing with an unbalanced tree or a tree with many levels.

Both approaches give the same result: the **maximum depth** of the binary tree. You can choose the approach based on your preference or specific constraints.

Let me know if you need further clarifications or examples!

## mlkas

The array `[3, 9, 20, null, null, 15, 7]` represents a **binary tree** in **level-order traversal** (also known as **BFS** or **breadth-first traversal**). In this case, `null` represents the absence of a node (i.e., a missing child).

Let's break down the tree structure:

```
         3
       /   \
      9     20
           /  \
          15   7
```

### Constructing the Binary Tree:

* The root node is `3`.
* The left child of `3` is `9` and the right child of `3` is `20`.
* The left and right children of `9` are `null` (so no further nodes for `9`).
* The left child of `20` is `15` and the right child of `20` is `7`.

### Maximum Depth Calculation:

We can calculate the **maximum depth** of this tree using either a **recursive DFS** approach or an **iterative BFS** approach, as discussed earlier.

Here’s how both methods work for this tree:

### 1. Recursive DFS Approach:

```js
function maxDepth(root) {
    if (root === null) {
        return 0;
    }

    const leftDepth = maxDepth(root.left);
    const rightDepth = maxDepth(root.right);

    return Math.max(leftDepth, rightDepth) + 1;
}

// Construct the tree
let root = new TreeNode(3);
root.left = new TreeNode(9);
root.right = new TreeNode(20);
root.right.left = new TreeNode(15);
root.right.right = new TreeNode(7);

console.log(maxDepth(root)); // Output: 3
```

### 2. Iterative BFS Approach:

```js
function maxDepthBFS(root) {
    if (root === null) return 0;
    
    const queue = [root];
    let depth = 0;
    
    while (queue.length > 0) {
        let levelSize = queue.length;
        depth++;
        
        // Process all nodes at the current level
        for (let i = 0; i < levelSize; i++) {
            let node = queue.shift();
            
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
    }
    
    return depth;
}

// Construct the tree
let root = new TreeNode(3);
root.left = new TreeNode(9);
root.right = new TreeNode(20);
root.right.left = new TreeNode(15);
root.right.right = new TreeNode(7);

console.log(maxDepthBFS(root)); // Output: 3
```

### Explanation:

* Both approaches correctly calculate the **maximum depth** of the tree, which is `3` in this case. The tree has 3 levels:

  1. Level 1: Root node `3`
  2. Level 2: Nodes `9` and `20`
  3. Level 3: Nodes `15` and `7`

### Tree Construction:

If you wanted to construct the binary tree from the array `[3, 9, 20, null, null, 15, 7]`, you would need to follow the level-order pattern. Here's the tree construction process:

1. Start with `3` as the root.
2. The next two values (`9` and `20`) are the children of `3`.
3. The next two `null` values indicate that `9` has no children.
4. The next two values (`15` and `7`) are the children of `20`.

Thus, the tree is:

```
         3
       /   \
      9     20
           /  \
          15   7
```

Let me know if you'd like more information or have further questions!


# topological sort


### Topological Sort

**Topological Sort** is an ordering of the vertices in a Directed Acyclic Graph (DAG) such that for every directed edge $uv$ (from vertex $u$ to vertex $v$), $u$ comes before $v$ in the ordering.

Topological sorting is **only possible** in **Directed Acyclic Graphs (DAGs)**. If the graph contains a cycle, a topological sort is **not possible** because there would be a circular dependency.

### Applications of Topological Sort:

* **Task Scheduling**: Where some tasks need to be completed before others.
* **Build Systems**: For compiling source files where certain files depend on others.
* **Course Prerequisites**: To find a valid order of courses to take when some courses have prerequisites.

### Approach to Topological Sort:

There are two common methods for performing topological sorting:

1. **Kahn's Algorithm (BFS Approach)**
2. **DFS-based Approach**

Let's break down both approaches.

---

### 1. **Kahn's Algorithm (BFS Approach)**

Kahn’s algorithm uses **indegree** (the number of incoming edges to a node) to determine which vertices can be placed in the topological order. The basic idea is:

* **Step 1**: Find all nodes with **0 indegree** (i.e., nodes with no incoming edges). These nodes can be added to the topological sort.
* **Step 2**: Remove these nodes from the graph and update the indegrees of their neighbors.
* **Step 3**: Repeat this process until all nodes have been processed or a cycle is detected.

#### Algorithm:

1. Initialize an empty queue and push all nodes with 0 indegree.
2. For each node in the queue:

   * Add it to the topological sort order.
   * For each neighbor of the node, reduce its indegree by 1.
   * If a neighbor's indegree becomes 0, push it into the queue.
3. If all nodes are processed (i.e., the topological sort contains all nodes), return the result. If not, a cycle is detected, and topological sorting is not possible.

#### Code Implementation (Kahn’s Algorithm):

```js
function topologicalSortKahn(graph) {
    const indegree = new Array(graph.length).fill(0);
    const result = [];
    const queue = [];

    // Calculate indegree of each node
    for (let node = 0; node < graph.length; node++) {
        for (let neighbor of graph[node]) {
            indegree[neighbor]++;
        }
    }

    // Enqueue nodes with 0 indegree
    for (let node = 0; node < graph.length; node++) {
        if (indegree[node] === 0) {
            queue.push(node);
        }
    }

    // Perform BFS (Kahn’s algorithm)
    while (queue.length > 0) {
        const node = queue.shift();
        result.push(node);

        // Reduce indegree of neighbors
        for (let neighbor of graph[node]) {
            indegree[neighbor]--;
            if (indegree[neighbor] === 0) {
                queue.push(neighbor);
            }
        }
    }

    // If the result contains all nodes, then return the sorted order
    if (result.length === graph.length) {
        return result;
    } else {
        // Cycle detected, return an empty array
        return [];
    }
}

// Example: Directed graph as an adjacency list
// 0 -> 1 -> 3
// 2 -> 3
const graph = [
    [1],    // Node 0 -> Node 1
    [3],    // Node 1 -> Node 3
    [3],    // Node 2 -> Node 3
    []      // Node 3 has no outgoing edges
];

console.log(topologicalSortKahn(graph));  // Output: [0, 2, 1, 3]
```

### Explanation:

* **graph**: This is a directed graph represented as an adjacency list. Each index corresponds to a node, and the array at that index contains the nodes it points to.
* **indegree**: An array that tracks the number of incoming edges for each node.
* **queue**: A queue that stores nodes with no incoming edges (indegree 0).
* **result**: The topologically sorted order.
* If the graph is a DAG, the `result` will contain all the nodes. If there’s a cycle, we’ll return an empty array.

---

### 2. **DFS-based Topological Sort**

In the DFS-based approach, we explore the graph depth-first and track the nodes being visited. After the DFS finishes processing all neighbors of a node, we can add the node to the topological order.

#### Algorithm:

1. Perform DFS starting from each unvisited node.
2. For each node, mark it as visited and explore its neighbors.
3. Once all neighbors of a node are processed, add the node to the **topological order**.
4. Finally, reverse the topological order to get the result (since the DFS processes nodes after their neighbors).

#### Code Implementation (DFS Approach):

```js
function topologicalSortDFS(graph) {
    const visited = new Array(graph.length).fill(false);
    const result = [];

    // Helper function for DFS traversal
    function dfs(node) {
        if (visited[node] === 1) return;  // Already processed node
        if (visited[node] === 2) {  // Cycle detected
            throw new Error("Graph has a cycle, topological sort not possible");
        }

        // Mark node as being visited
        visited[node] = 2;
        
        // Visit all neighbors
        for (let neighbor of graph[node]) {
            dfs(neighbor);
        }

        // Mark the node as processed and add to the result
        visited[node] = 1;
        result.push(node);
    }

    // Perform DFS for all nodes
    for (let node = 0; node < graph.length; node++) {
        if (!visited[node]) {
            dfs(node);
        }
    }

    // Reverse the result array to get the topological order
    return result.reverse();
}

// Example: Directed graph as an adjacency list
// 0 -> 1 -> 3
// 2 -> 3
const graph = [
    [1],    // Node 0 -> Node 1
    [3],    // Node 1 -> Node 3
    [3],    // Node 2 -> Node 3
    []      // Node 3 has no outgoing edges
];

console.log(topologicalSortDFS(graph));  // Output: [0, 2, 1, 3]
```

### Explanation:

* **visited**: Tracks the status of each node (0 = unvisited, 1 = visited, 2 = currently visiting).
* **dfs()**: A helper function that performs a DFS and adds nodes to the topological order after processing all their neighbors.
* **result**: Stores the nodes in reverse order of finishing time.

In this approach:

* If a cycle is detected (i.e., a node is revisited while it's being processed), an exception is thrown.
* The result array is reversed at the end to get the correct topological order.

---

### Comparison of Kahn's Algorithm vs DFS:

* **Kahn's Algorithm**:

  * Iterative approach using **BFS**.
  * Tracks indegrees of nodes.
  * Detects cycles by checking if we have processed all nodes.
  * Requires a queue and works in **O(V + E)** time complexity.

* **DFS-based Algorithm**:

  * Recursive approach using **DFS**.
  * Uses a visited array to detect cycles.
  * Works in **O(V + E)** time complexity.
  * May be harder to implement in cases with cycles but can be more intuitive in some cases.

---

### Time and Space Complexity:

Both algorithms have the same time and space complexity:

* **Time Complexity**: **O(V + E)**, where `V` is the number of vertices (nodes) and `E` is the number of edges.
* **Space Complexity**: **O(V + E)**, where space is used to store the graph, the visited array, and the result.

---

### Example Usage:

Let's try the topological sort on a simple directed graph.

```js
// Directed graph as an adjacency list
// 0 -> 1 -> 3
// 2 -> 3
const graph = [
    [1],    // Node 0 -> Node 1
    [3],    // Node 1 -> Node 3
    [3],    // Node 2 -> Node 3
    []      // Node 3 has no outgoing edges
];

// Kahn's algorithm
console.log(topologicalSortKahn(graph));  // Output: [0, 2, 1, 3]

// DFS-based approach
console.log(topologicalSortDFS(graph));   // Output: [0, 2, 1, 3]
```

Both algorithms will output the same result, which is a valid topological sort of the graph.

---

Let me know if you have more questions or need further clarification!

## course schedule

### Course Schedule Problem

The **Course Schedule** problem is a common interview question that involves topological sorting. The problem is usually framed like this:

### Problem Statement:

You are given `n` courses labeled from `0` to `n-1` and a list of **prerequisites** where each prerequisite is a pair `[course, prerequisite]` indicating that you must take `prerequisite` before you can take `course`. The task is to determine if it is possible to finish all the courses.

#### Example:

Input:

* `n = 2`
* `prerequisites = [[1, 0]]`

This means:

* You have 2 courses (0 and 1).
* Course 1 has a prerequisite of Course 0, i.e., you must take Course 0 before you can take Course 1.

Output:

* Return `true` if you can finish all the courses, otherwise return `false`.

#### Example 1:

Input:

```js
n = 2;
prerequisites = [[1, 0]];
```

Output: `true`
Explanation: There are two courses: `0` and `1`. The only prerequisite is that Course `1` depends on Course `0`. Since there are no cycles in the graph, it is possible to finish all the courses.

#### Example 2:

Input:

```js
n = 2;
prerequisites = [[1, 0], [0, 1]];
```

Output: `false`
Explanation: There is a cycle in the graph: Course `0` depends on Course `1` and Course `1` depends on Course `0`. Hence, it's not possible to finish the courses.

---

### Approach to Solve the Problem:

This problem can be solved using **Topological Sort**, which is a typical approach for solving **cycle detection in a Directed Acyclic Graph (DAG)**. If we can perform a valid topological sort on the graph (which represents courses and prerequisites), it means that there are no cycles and we can finish all the courses. If we cannot perform a valid topological sort, it means there's a cycle, and it's impossible to finish all the courses.

We can use **Kahn’s Algorithm (BFS)** or **DFS-based approach** to check if a valid topological sort is possible.

---

### Solution: Using Kahn’s Algorithm (BFS Approach)

1. **Build the graph**: First, we build a directed graph where each node represents a course, and the edges represent the prerequisite relationships.
2. **Compute in-degrees**: Calculate the in-degree (number of incoming edges) for each node.
3. **Perform Kahn’s Algorithm**:

   * Start with nodes having in-degree 0 (i.e., nodes with no prerequisites).
   * Process each node, reduce the in-degree of its neighbors, and add any node with in-degree 0 to the processing queue.
   * If at the end, all nodes have been processed, return `true` (indicating no cycle).
   * If some nodes remain unprocessed, return `false` (indicating a cycle).

#### Code Implementation:

```js
function canFinish(n, prerequisites) {
    // Step 1: Build the graph and in-degree array
    const graph = new Array(n).fill(0).map(() => []);
    const inDegree = new Array(n).fill(0);
    
    for (let [course, prereq] of prerequisites) {
        graph[prereq].push(course); // prereq -> course
        inDegree[course]++; // Increase in-degree of the course
    }

    // Step 2: Initialize the queue with nodes having no prerequisites (in-degree 0)
    const queue = [];
    for (let i = 0; i < n; i++) {
        if (inDegree[i] === 0) {
            queue.push(i);
        }
    }

    // Step 3: Process the nodes
    let processedCount = 0;

    while (queue.length > 0) {
        const course = queue.shift();
        processedCount++;

        // Reduce in-degree for all dependent courses
        for (let nextCourse of graph[course]) {
            inDegree[nextCourse]--;
            if (inDegree[nextCourse] === 0) {
                queue.push(nextCourse);
            }
        }
    }

    // Step 4: If we processed all courses, return true, otherwise false
    return processedCount === n;
}

// Example usage:
let n1 = 2;
let prerequisites1 = [[1, 0]];  // Course 1 depends on Course 0
console.log(canFinish(n1, prerequisites1));  // Output: true

let n2 = 2;
let prerequisites2 = [[1, 0], [0, 1]];  // Course 1 depends on Course 0 and vice versa (cycle)
console.log(canFinish(n2, prerequisites2));  // Output: false
```

### Explanation of Code:

1. **Graph Construction**:

   * We create a directed graph where each node represents a course, and edges represent the prerequisites.
   * We also maintain an `inDegree` array to count the number of prerequisites for each course.

2. **Queue Initialization**:

   * All nodes (courses) with `inDegree = 0` are added to the queue. These are courses with no prerequisites, and they can be taken immediately.

3. **Processing the Queue**:

   * We process each node in the queue, decrementing the `inDegree` of all its neighbors (courses that depend on it).
   * If any neighbor’s `inDegree` becomes 0, it is added to the queue (as it can now be taken).
   * We maintain a counter (`processedCount`) to track how many courses we've processed.

4. **Cycle Detection**:

   * If the number of processed courses equals `n`, then it means we have successfully completed the topological sort, and there are no cycles.
   * If not all courses are processed, then there is a cycle, and it's impossible to complete the courses.

### Time and Space Complexity:

* **Time Complexity**: `O(V + E)`, where `V` is the number of courses (vertices) and `E` is the number of prerequisites (edges). This is because we iterate through each edge and vertex once.
* **Space Complexity**: `O(V + E)`, where `V` is the number of courses and `E` is the number of prerequisites. The space is used by the graph and in-degree array.

---

### Alternative DFS-based Solution:

Another approach is to use **DFS** to detect cycles in the graph. In the DFS approach, we explore each node and mark it as visiting, and if we encounter a node that is already being visited, a cycle is detected.

### Code Implementation (DFS):

```js
function canFinish(n, prerequisites) {
    const graph = new Array(n).fill(0).map(() => []);
    const visited = new Array(n).fill(0); // 0 = unvisited, 1 = visiting, 2 = visited

    for (let [course, prereq] of prerequisites) {
        graph[prereq].push(course);
    }

    function dfs(course) {
        if (visited[course] === 1) return false; // Cycle detected
        if (visited[course] === 2) return true;  // Already visited

        // Mark as visiting (in progress)
        visited[course] = 1;

        // Visit all neighbors (prerequisites)
        for (let nextCourse of graph[course]) {
            if (!dfs(nextCourse)) return false;
        }

        // Mark as visited
        visited[course] = 2;
        return true;
    }

    for (let i = 0; i < n; i++) {
        if (visited[i] === 0) {
            if (!dfs(i)) return false; // If a cycle is detected, return false
        }
    }

    return true;
}

// Example usage:
let n1 = 2;
let prerequisites1 = [[1, 0]];  // Course 1 depends on Course 0
console.log(canFinish(n1, prerequisites1));  // Output: true

let n2 = 2;
let prerequisites2 = [[1, 0], [0, 1]];  // Course 1 depends on Course 0 and vice versa (cycle)
console.log(canFinish(n2, prerequisites2));  // Output: false
```

### Explanation of DFS Code:

1. **Graph Construction**: Same as in Kahn's algorithm.
2. **DFS Traversal**: For each course, if it's not yet visited, start DFS.

   * During DFS, mark nodes as "visiting" (1).
   * If we encounter a "visiting" node again, it indicates a cycle, so return `false`.
   * Once a node and all its neighbors are processed, mark it as "visited" (2).
3. **Cycle Detection**: If DFS detects a cycle, we return `false`. If no cycles are detected, we return `true`.

---

### Conclusion:

* The **Kahn’s Algorithm** (BFS approach) is easier to implement and typically used for topological sorting when dealing with dependency-based problems.
* The **DFS-based approach** is also valid but more complex because of cycle detection.

Both solutions have the same time and space complexity, and both are suitable for solving the **Course Schedule** problem.

Let me know if you need further explanation or examples!


# binary tree bfs

### Binary Tree BFS (Breadth-First Search)

**Breadth-First Search (BFS)** on a binary tree is an approach where we traverse the tree level by level, visiting all the nodes at one level before moving on to the next. This is also known as **level-order traversal**.

### Steps for BFS on Binary Tree:

1. Start with the **root** node.
2. Use a **queue** (FIFO structure) to store the nodes as we visit them.
3. For each node, do the following:

   * Process the node (e.g., print the node value or perform some other operation).
   * Add the left child (if it exists) to the queue.
   * Add the right child (if it exists) to the queue.
4. Repeat this process until the queue is empty.

This ensures that we visit the nodes level by level.

### BFS Example:

Given the binary tree:

```
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

The **level-order traversal** (BFS) will give:

```
[1, 2, 3, 4, 5, 6, 7]
```

### BFS Implementation (Level-order Traversal):

```js
function bfs(root) {
    if (root === null) {
        return [];
    }

    const result = [];
    const queue = [root];  // Start with the root in the queue

    while (queue.length > 0) {
        const currentNode = queue.shift();  // Remove the node from the front of the queue
        result.push(currentNode.val);  // Process the current node

        // Add the left and right children to the queue (if they exist)
        if (currentNode.left) {
            queue.push(currentNode.left);
        }
        if (currentNode.right) {
            queue.push(currentNode.right);
        }
    }

    return result;
}

// Binary Tree Node Definition
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

// Example usage
const root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);
root.right.left = new TreeNode(6);
root.right.right = new TreeNode(7);

console.log(bfs(root));  // Output: [1, 2, 3, 4, 5, 6, 7]
```

### Explanation of Code:

1. **TreeNode Constructor**: A simple constructor for creating a binary tree node with a `val`, `left` child, and `right` child.

2. **BFS Function**:

   * We initialize a `queue` with the root node.
   * While the queue is not empty, we dequeue the front node, process it (by adding it to the `result` array), and enqueue its left and right children if they exist.

3. **Output**: The result array will contain the values of the nodes in level-order (BFS) traversal.

---

### BFS with Multiple Levels (Queue and Level Tracking):

You can also extend BFS to track nodes level by level. For example, if you want to print the values of each level on a new line, you can modify the approach to process each level separately.

#### Example:

```
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

The output (level by level) would be:

```
[1]
[2, 3]
[4, 5, 6, 7]
```

#### Code for Level-wise BFS:

```js
function bfsLevelOrder(root) {
    if (root === null) {
        return [];
    }

    const result = [];
    const queue = [root];

    while (queue.length > 0) {
        const levelSize = queue.length;
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift();
            currentLevel.push(currentNode.val);

            if (currentNode.left) {
                queue.push(currentNode.left);
            }
            if (currentNode.right) {
                queue.push(currentNode.right);
            }
        }

        result.push(currentLevel);
    }

    return result;
}

// Example usage
const root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);
root.right.left = new TreeNode(6);
root.right.right = new TreeNode(7);

console.log(bfsLevelOrder(root));  // Output: [[1], [2, 3], [4, 5, 6, 7]]
```

### Explanation of Level-wise BFS:

* For each level, we first determine how many nodes are in that level by checking the `queue` size.
* We process all the nodes at that level, collect their values, and then add their children to the queue.
* The `result` array will contain an array for each level, representing the values of nodes at that level.

---

### BFS Time and Space Complexity:

* **Time Complexity**: `O(n)`, where `n` is the number of nodes in the binary tree. We visit each node once.
* **Space Complexity**: `O(n)`, where `n` is the number of nodes. In the worst case, the queue will contain all nodes at the last level of the tree, which could be up to `n/2` nodes in a full binary tree.

---

### Conclusion:

BFS on a binary tree is a useful technique for level-order traversal, which can be applied in problems where you need to explore nodes level by level (e.g., finding the shortest path in an unweighted graph, tree traversal, etc.).

Let me know if you'd like to explore more BFS-related problems or need further clarification!

# binary tree level order traversal

### Binary Tree Level Order Traversal (BFS)

**Level Order Traversal** is the process of visiting each node of the binary tree level by level, starting from the root and moving down to the next levels.

This is equivalent to **Breadth-First Search (BFS)** for trees. The basic idea is to process each level of the tree one by one, and for each level, we process each node at that level before moving to the next one.

### Example:

For the following binary tree:

```
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

The **level order traversal** of this tree will be:

```
[1]
[2, 3]
[4, 5, 6, 7]
```

### Steps to Implement Binary Tree Level Order Traversal:

1. **Start at the root** node and process it.
2. **Use a queue** to store nodes at each level. Begin by adding the root node to the queue.
3. **Process the queue**:

   * For each level, record all the nodes at that level (i.e., dequeue each node, process it, and enqueue its children if they exist).
4. Repeat until all levels have been processed.

### BFS Code Implementation for Level Order Traversal:

```js
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

function levelOrderTraversal(root) {
    if (root === null) {
        return [];
    }

    const result = [];
    const queue = [root];  // Start with the root node in the queue

    while (queue.length > 0) {
        const levelSize = queue.length;  // Number of nodes at the current level
        const currentLevel = [];

        // Process each node at the current level
        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift();  // Dequeue node
            currentLevel.push(currentNode.val);  // Process the node value

            // Enqueue left and right children if they exist
            if (currentNode.left) {
                queue.push(currentNode.left);
            }
            if (currentNode.right) {
                queue.push(currentNode.right);
            }
        }

        // Add the current level to the result array
        result.push(currentLevel);
    }

    return result;
}

// Example usage:

const root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);
root.right.left = new TreeNode(6);
root.right.right = new TreeNode(7);

console.log(levelOrderTraversal(root));
// Output: [[1], [2, 3], [4, 5, 6, 7]]
```

### Explanation of the Code:

1. **TreeNode Definition**: A simple definition of a tree node with a `val`, `left`, and `right` pointer.

2. **levelOrderTraversal Function**:

   * **Input**: The root of the binary tree.
   * **Queue**: We initialize a queue and start with the root node.
   * **While Loop**: The loop continues as long as there are nodes in the queue (i.e., there are still nodes to process).
   * **Level Size**: For each iteration, we calculate the number of nodes at the current level (the size of the queue).
   * **Processing Each Level**: We dequeue each node at the current level, add its value to the `currentLevel` array, and enqueue its children.
   * **Final Result**: We collect the values at each level in the `result` array, which will be returned at the end.

3. **Example Usage**: A binary tree is created, and the `levelOrderTraversal` function is called. The output is the level-order traversal of the tree.

---

### Output:

For the input binary tree:

```
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

The output will be:

```js
[[1], [2, 3], [4, 5, 6, 7]]
```

This is the correct level-order traversal, where:

* The first level has only the root node `1`.
* The second level has nodes `2` and `3`.
* The third level has nodes `4`, `5`, `6`, and `7`.

---

### Time and Space Complexity:

* **Time Complexity**: `O(n)`, where `n` is the number of nodes in the binary tree. We visit each node once and process it.
* **Space Complexity**: `O(n)`, where `n` is the number of nodes. In the worst case, the queue will contain all the nodes at the last level, which could be half of the total nodes in a complete binary tree.

---

### Variations of Level Order Traversal:

1. **Spiral (Zigzag) Level Order**: Instead of visiting levels in a straight order, visit alternate levels in reverse order.
2. **Print Level by Level Without Using Extra Space**: This can be done by using a `levelSize` counter in the queue to control the number of nodes at the current level.

Let me know if you want to explore more variations or need additional examples!


## [3,9,20,null,null,15,7]

Given the binary tree represented by the array `[3,9,20,null,null,15,7]`, we need to build the tree and perform **Level Order Traversal (BFS)**.

### Step 1: Construct the Binary Tree

The array `[3,9,20,null,null,15,7]` represents the following binary tree:

```
        3
      /   \
     9     20
          /  \
         15   7
```

* `3` is the root.
* `9` is the left child of `3`.
* `20` is the right child of `3`.
* `15` is the left child of `20`.
* `7` is the right child of `20`.

### Step 2: Perform Level Order Traversal (BFS)

We will use the **BFS (Breadth-First Search)** approach to visit the nodes level by level.

### Code Implementation:

```js
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

function levelOrderTraversal(root) {
    if (root === null) {
        return [];
    }

    const result = [];
    const queue = [root];  // Start with the root node in the queue

    while (queue.length > 0) {
        const levelSize = queue.length;  // Number of nodes at the current level
        const currentLevel = [];

        // Process each node at the current level
        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift();  // Dequeue node
            currentLevel.push(currentNode.val);  // Process the node value

            // Enqueue left and right children if they exist
            if (currentNode.left) {
                queue.push(currentNode.left);
            }
            if (currentNode.right) {
                queue.push(currentNode.right);
            }
        }

        // Add the current level to the result array
        result.push(currentLevel);
    }

    return result;
}

// Build the tree from the given array [3,9,20,null,null,15,7]
const root = new TreeNode(3);
root.left = new TreeNode(9);
root.right = new TreeNode(20);
root.right.left = new TreeNode(15);
root.right.right = new TreeNode(7);

// Perform level order traversal
console.log(levelOrderTraversal(root));  // Output: [[3], [9, 20], [15, 7]]
```

### Explanation:

1. **TreeNode Definition**: We define the basic structure of a binary tree node with a `val`, `left`, and `right` property.

2. **levelOrderTraversal Function**:

   * We initialize a queue with the root node.
   * For each level, we process each node and add its left and right children to the queue.
   * The result array contains the nodes level by level.

3. **Tree Construction**: We manually construct the binary tree from the given array `[3, 9, 20, null, null, 15, 7]`.

4. **Output**: After performing the level-order traversal, the output will be the node values grouped by levels.

### Output:

For the input binary tree:

```
        3
      /   \
     9     20
          /  \
         15   7
```

The level-order traversal (BFS) will give:

```js
[[3], [9, 20], [15, 7]]
```

* The first level has only the root node: `[3]`.
* The second level has nodes `9` and `20`: `[9, 20]`.
* The third level has nodes `15` and `7`: `[15, 7]`.

---

### Time and Space Complexity:

* **Time Complexity**: `O(n)`, where `n` is the number of nodes in the binary tree. We process each node exactly once.
* **Space Complexity**: `O(n)`, where `n` is the number of nodes. In the worst case, the queue will contain all the nodes at the last level, which could be up to `n/2` nodes in a complete binary tree.

Let me know if you need any more explanations or if you'd like to explore other tree traversal techniques!

# two pointer parttern

### Two Pointer Pattern

The **Two Pointer Pattern** is a common technique in algorithmic problems, especially when dealing with arrays or linked lists. The idea behind the Two Pointer Pattern is to use two pointers to traverse the data structure, often starting from opposite ends or in a synchronized manner, to solve problems efficiently.

Two pointers are often used in situations where:

* You want to reduce the time complexity of problems.
* The problem can be solved by processing elements in pairs or with comparisons.
* The array or list is sorted or can be sorted.

### Common Types of Two Pointer Approaches

1. **Two pointers moving in opposite directions**:

   * This is typically used when you want to explore pairs from both ends of an array.
   * **Example Problem**: Find two numbers that sum up to a specific target in a sorted array.

2. **Two pointers moving in the same direction**:

   * This is used when one pointer iterates over the elements and the other pointer moves conditionally based on the state of the first pointer.
   * **Example Problem**: Removing duplicates from a sorted array, finding the longest subarray with a sum less than or equal to a target.

### Example 1: **Two Sum (Sorted Array)**

Problem: Given a sorted array, find two numbers that sum up to a specific target.

Example:

```js
Input: nums = [1, 2, 3, 4, 5, 6], target = 7
Output: [1, 6]
Explanation: The two numbers that sum up to 7 are 1 and 6.
```

#### Solution using Two Pointers:

* Start with one pointer at the beginning (`left`) and the other at the end (`right`).
* Calculate the sum of the values at the two pointers:

  * If the sum is equal to the target, return the pair.
  * If the sum is less than the target, move the left pointer to the right (increase the sum).
  * If the sum is greater than the target, move the right pointer to the left (decrease the sum).

```js
function twoSum(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    
    while (left < right) {
        const sum = nums[left] + nums[right];
        if (sum === target) {
            return [nums[left], nums[right]];
        } else if (sum < target) {
            left++;  // Increase left pointer to increase the sum
        } else {
            right--;  // Decrease right pointer to decrease the sum
        }
    }
    return [];  // No solution
}

// Example usage:
console.log(twoSum([1, 2, 3, 4, 5, 6], 7));  // Output: [1, 6]
```

### Explanation:

* The `left` pointer starts at the beginning of the array, and the `right` pointer starts at the end.
* As you move the pointers inward, you try to find the two numbers whose sum matches the target.

### Time and Space Complexity:

* **Time Complexity**: `O(n)`, where `n` is the number of elements in the array. We only traverse the array once.
* **Space Complexity**: `O(1)`, since we only use a few variables and no additional space is required.

---

### Example 2: **Removing Duplicates from a Sorted Array**

Problem: Given a sorted array, remove duplicates in-place such that each element appears only once and return the new length.

Example:

```js
Input: nums = [1, 1, 2, 2, 3]
Output: 3
Explanation: After removing duplicates, the array is [1, 2, 3].
```

#### Solution using Two Pointers:

* Use one pointer (`i`) to iterate through the array and a second pointer (`j`) to track where the unique elements should be placed.

```js
function removeDuplicates(nums) {
    if (nums.length === 0) return 0;
    
    let i = 0; // Pointer to the last unique element
    
    for (let j = 1; j < nums.length; j++) {
        if (nums[i] !== nums[j]) {
            i++;
            nums[i] = nums[j]; // Place the unique element at the new position
        }
    }
    
    return i + 1; // Return the new length of the array
}

// Example usage:
let nums = [1, 1, 2, 2, 3];
console.log(removeDuplicates(nums));  // Output: 3
console.log(nums);  // Output: [1, 2, 3, 2, 3]
```

### Explanation:

* Pointer `i` tracks the last unique element.
* Pointer `j` iterates through the array to check if the element at `nums[j]` is different from `nums[i]`. If so, `i` is incremented and the unique element is moved to `nums[i]`.

### Time and Space Complexity:

* **Time Complexity**: `O(n)`, where `n` is the number of elements in the array.
* **Space Complexity**: `O(1)`, as we are modifying the array in-place without using extra space.

---

### Example 3: **Maximum Subarray with Sum ≤ K**

Problem: Given an array of integers, find the maximum sum of a subarray such that the sum is less than or equal to `k`.

Example:

```js
Input: nums = [2, 1, 3, 4, 5], k = 7
Output: 7
Explanation: The subarray [2, 1, 3] has a sum of 6 which is less than 7, and is the maximum sum.
```

#### Solution using Two Pointers (Sliding Window):

* Use two pointers to maintain a sliding window. Expand the window by moving the right pointer, and shrink it by moving the left pointer when the sum exceeds `k`.

```js
function maxSubArraySum(nums, k) {
    let left = 0;
    let sum = 0;
    let maxSum = 0;
    
    for (let right = 0; right < nums.length; right++) {
        sum += nums[right]; // Add the current element to the sum
        
        while (sum > k) {
            sum -= nums[left]; // Shrink the window from the left
            left++;
        }
        
        maxSum = Math.max(maxSum, sum); // Update maxSum if we find a larger sum
    }
    
    return maxSum;
}

// Example usage:
console.log(maxSubArraySum([2, 1, 3, 4, 5], 7));  // Output: 7
```

### Explanation:

* The `right` pointer expands the window by moving to the right and adding elements to the sum.
* If the sum exceeds `k`, the `left` pointer shrinks the window by moving to the right and subtracting elements from the sum.
* The maximum sum that is less than or equal to `k` is tracked in `maxSum`.

### Time and Space Complexity:

* **Time Complexity**: `O(n)`, where `n` is the number of elements in the array. Both pointers move across the array once.
* **Space Complexity**: `O(1)`, since we only use a few variables for tracking the sum and indices.

---

### Common Problems Solved Using the Two Pointer Pattern:

1. **Finding pairs or triplets in an array that satisfy a certain condition** (e.g., sum, product, etc.).
2. **Finding subarrays or subsequences that satisfy a condition**.
3. **Reversing elements in-place** (in a sorted array, for example).
4. **Sliding window problems** (e.g., longest subarray with a sum ≤ k).
5. **Partitioning arrays** (e.g., Dutch National Flag problem).

---

### Summary:

The **Two Pointer Pattern** is highly effective in solving problems related to arrays, linked lists, and other sequential data structures. It allows us to optimize the time complexity by reducing the problem to a single pass or by processing elements in pairs.

## 3sum
### **3Sum Problem**

The **3Sum** problem is a classic algorithmic problem where you are given an array of integers, and your goal is to find all unique triplets in the array that sum up to a specific target (usually 0).

### Problem Definition:

Given an array `nums` of **n** integers, return all the unique triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k` and:

```
nums[i] + nums[j] + nums[k] == 0
```

### Example:

```js
Input: nums = [-1, 0, 1, 2, -1, -4]
Output: [[-1, -1, 2], [-1, 0, 1]]
Explanation: The solution set is:
[
  [-1, -1, 2],
  [-1, 0, 1]
]
```

### Approach to Solve 3Sum:

To solve the problem efficiently, you can use a combination of the **two-pointer** technique with **sorting**. Here's how:

1. **Sort the array**: Sorting the array helps you to skip duplicates and makes it easier to find valid triplets.
2. **Fix one element** (using a loop): For each element `nums[i]`, treat it as the first element of the triplet and try to find two other numbers using the two-pointer technique.
3. **Two-pointer technique**: After fixing one element, use two pointers (`left` and `right`) to find the other two numbers that sum up to zero.
4. **Skip duplicates**: To ensure uniqueness, skip duplicate elements while moving the pointers and when fixing the element.

### Steps:

1. Sort the array to enable the two-pointer technique.
2. Iterate over each element. For each element `nums[i]`, apply the two-pointer approach on the subarray to the right of `i` (i.e., `nums[i+1:]`).
3. If the sum of the three elements is zero, record the triplet. Move the left pointer to the right and the right pointer to the left to check for other potential pairs.
4. Skip duplicate elements to avoid counting the same triplet multiple times.

### Code Implementation:

```js
function threeSum(nums) {
    const result = [];
    nums.sort((a, b) => a - b);  // Step 1: Sort the array

    for (let i = 0; i < nums.length - 2; i++) {
        // Skip duplicates
        if (i > 0 && nums[i] === nums[i - 1]) continue;

        let left = i + 1;  // Left pointer
        let right = nums.length - 1;  // Right pointer
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];

            if (sum === 0) {
                // If we found a valid triplet
                result.push([nums[i], nums[left], nums[right]]);
                
                // Skip duplicates for left and right
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                
                left++;
                right--;
            } else if (sum < 0) {
                left++;  // Need a larger sum, so move left pointer to the right
            } else {
                right--;  // Need a smaller sum, so move right pointer to the left
            }
        }
    }

    return result;
}

// Example usage:
const nums = [-1, 0, 1, 2, -1, -4];
console.log(threeSum(nums)); 
// Output: [[-1, -1, 2], [-1, 0, 1]]
```

### Explanation:

1. **Sorting**: We sort the input array `nums` to make it easier to find the triplets and skip duplicates.

2. **Iterating through the array**: The outer loop iterates through each number in the array (`nums[i]`), treating it as the first element of the triplet.

3. **Two-pointer approach**:

   * We use two pointers (`left` and `right`) to find two numbers that sum up to the negative of `nums[i]`.
   * If the sum is zero, we add the triplet `[nums[i], nums[left], nums[right]]` to the result array.
   * If the sum is less than zero, move the `left` pointer to the right to increase the sum.
   * If the sum is greater than zero, move the `right` pointer to the left to decrease the sum.

4. **Skip duplicates**: After finding a valid triplet, we skip any duplicate values by moving the pointers (`left` and `right`) to avoid counting the same triplet multiple times.

5. **Return the result**: The result array will contain all the unique triplets that sum up to zero.

### Time and Space Complexity:

* **Time Complexity**: `O(n^2)`, where `n` is the number of elements in the array. The outer loop runs `n` times, and for each iteration, the inner while loop runs in `O(n)` time due to the two pointers.

* **Space Complexity**: `O(1)` if we disregard the space needed for the input and output, as the algorithm only uses a constant amount of extra space. However, since we're returning an array with the triplets, the space complexity for storing the results will be proportional to the number of valid triplets, which is `O(k)` where `k` is the number of triplets.

### Example Walkthrough:

For the input array `[-1, 0, 1, 2, -1, -4]`:

1. **Step 1 - Sort the array**:
   Sorted array: `[-4, -1, -1, 0, 1, 2]`

2. **Step 2 - Iterate with two pointers**:

   * For `nums[i] = -4`, the two pointers start at `left = -1` and `right = 2`. The sum is `-4 + (-1) + 2 = -3`, which is less than zero, so we move the `left` pointer to the right.
   * For `nums[i] = -1`, the two pointers start at `left = -1` and `right = 2`. The sum is `-1 + (-1) + 2 = 0`, which is a valid triplet `[-1, -1, 2]`, so we record it and move the pointers.
   * Continue the process to find other valid triplets.

3. **Final result**: The valid triplets are `[[-1, -1, 2], [-1, 0, 1]]`.

---

### Edge Cases:

* **No valid triplets**: If there are no triplets that sum up to zero, return an empty array.
* **All duplicates**: The algorithm efficiently handles arrays with many duplicate numbers by skipping over duplicates during the iteration and when moving the two pointers.
* **Empty or short arrays**: If the array has fewer than 3 elements, return an empty array since no triplet can be formed.

---

Let me know if you'd like further clarifications or more examples!

## [-1,0,1,2,-1,-4]

Let's go through the **3Sum** problem step by step for the input array `[-1, 0, 1, 2, -1, -4]`.

### Problem Recap:

We need to find all unique triplets in the array such that the sum of the triplet equals 0.

Given:

```
Input: nums = [-1, 0, 1, 2, -1, -4]
Output: [[-1, -1, 2], [-1, 0, 1]]
```

### Steps to Solve:

1. **Sort the array**: Sorting helps to easily manage duplicates and makes it possible to use the two-pointer technique efficiently.

   After sorting `nums = [-1, 0, 1, 2, -1, -4]`, we get:

   ```
   Sorted nums = [-4, -1, -1, 0, 1, 2]
   ```

2. **Iterate through the array**: We iterate through each number `nums[i]` and use two pointers (`left` and `right`) to find two other numbers such that their sum with `nums[i]` equals 0.

3. **Two Pointer Technique**:

   * For each `nums[i]`, set `left = i + 1` and `right = nums.length - 1`.
   * Calculate the sum of the three numbers: `nums[i] + nums[left] + nums[right]`.

     * If the sum equals 0, record the triplet.
     * If the sum is less than 0, move the `left` pointer to the right to increase the sum.
     * If the sum is greater than 0, move the `right` pointer to the left to decrease the sum.

4. **Skip Duplicates**: After finding a triplet, make sure to skip any duplicate values by checking adjacent values of the same `nums[i]`, `nums[left]`, and `nums[right]`.

### Code Implementation:

```js
function threeSum(nums) {
    const result = [];
    nums.sort((a, b) => a - b);  // Step 1: Sort the array

    for (let i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for the current element
        if (i > 0 && nums[i] === nums[i - 1]) continue;

        let left = i + 1;  // Left pointer
        let right = nums.length - 1;  // Right pointer
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];

            if (sum === 0) {
                // If we found a valid triplet
                result.push([nums[i], nums[left], nums[right]]);
                
                // Skip duplicates for left and right pointers
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                
                left++;
                right--;
            } else if (sum < 0) {
                left++;  // Need a larger sum, so move left pointer to the right
            } else {
                right--;  // Need a smaller sum, so move right pointer to the left
            }
        }
    }

    return result;
}

// Example usage:
const nums = [-1, 0, 1, 2, -1, -4];
console.log(threeSum(nums)); 
// Output: [[-1, -1, 2], [-1, 0, 1]]
```

### Walkthrough of the Algorithm:

1. **Initial Array**:

   ```
   nums = [-1, 0, 1, 2, -1, -4]
   ```

2. **Step 1: Sort the array**:

   ```
   Sorted nums = [-4, -1, -1, 0, 1, 2]
   ```

3. **Step 2: Iterate and Use Two Pointers**:

   * **Iteration 1 (i = 0)**:

     * `nums[i] = -4`

     * **Left Pointer**: `left = 1` (points to `-1`)

     * **Right Pointer**: `right = 5` (points to `2`)

     * **Sum**: `-4 + (-1) + 2 = -3` (sum is less than 0, so move the left pointer to the right)

     * New left pointer: `left = 2`

     * **New Sum**: `-4 + (-1) + 2 = -3` (again, sum is less than 0, move left to the right)

     * New left pointer: `left = 3`

     * **New Sum**: `-4 + 0 + 2 = -2` (again, move left to the right)

     * New left pointer: `left = 4`

     * **New Sum**: `-4 + 1 + 2 = -1` (again, move left to the right)

     * New left pointer: `left = 5`

     * **No valid triplet found for i = 0**.

   * **Iteration 2 (i = 1)**:

     * `nums[i] = -1`
     * **Left Pointer**: `left = 2` (points to `-1`)
     * **Right Pointer**: `right = 5` (points to `2`)
     * **Sum**: `-1 + (-1) + 2 = 0` (valid triplet found: `[-1, -1, 2]`)

       * Add it to the result.
       * Move `left` to the right and `right` to the left to avoid duplicates.
       * New left pointer: `left = 3` (points to `0`)
       * New right pointer: `right = 4` (points to `1`)
     * **New Sum**: `-1 + 0 + 1 = 0` (valid triplet found: `[-1, 0, 1]`)

       * Add it to the result.
       * Move `left` to the right and `right` to the left to avoid duplicates.
       * New left pointer: `left = 4`
       * New right pointer: `right = 3`
     * **No more valid triplets for i = 1**.

   * **Iteration 3 (i = 2)**:

     * `nums[i] = -1` (skip since `nums[i] == nums[i-1]`, to avoid duplicates).

   * **Iteration 4 (i = 3)**:

     * `nums[i] = 0`
     * The `left` pointer would be greater than `right` here, so no triplet is found.

4. **Step 3: Return Result**:
   The result array contains the unique triplets:

   ```
   [[-1, -1, 2], [-1, 0, 1]]
   ```

### Time and Space Complexity:

* **Time Complexity**: `O(n^2)`, where `n` is the number of elements in the array. The outer loop runs `n` times, and for each iteration, the inner while loop uses two pointers and processes each element at most once.

* **Space Complexity**: `O(k)`, where `k` is the number of valid triplets. We only store the triplets in the result array, so the space complexity is proportional to the number of triplets.

### Final Answer:

For the input `[-1, 0, 1, 2, -1, -4]`, the output will be:

```js
[[-1, -1, 2], [-1, 0, 1]]
```

Let me know if you'd like further clarifications!

