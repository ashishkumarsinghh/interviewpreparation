# Programming Patterns for FAANG Interviews

## 1. Two Pointers
Use two pointers to solve array-related problems efficiently. Typically used with sorted arrays or linked lists.

**Intuition**: Instead of using nested loops (O(n²)), maintain two pointers that move toward each other or in the same direction.

**Time Complexity**: Usually O(n) as we process each element at most once
**Space Complexity**: Usually O(1) as we use only pointer variables

**Example Problems**:
- Two Sum (in sorted array)
- Container With Most Water
- 3Sum, 4Sum
- Remove Duplicates
- Palindrome Check

```python
# Finding a pair with a given sum in a sorted array
def two_sum(nums, target):
    left, right = 0, len(nums) - 1
    while left < right:
        current_sum = nums[left] + nums[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1  # Need a larger value
        else:
            right -= 1  # Need a smaller value
    return [-1, -1]  # No solution found
```

## 2. Sliding Window
Useful for array/string problems involving contiguous sequences. Create a window that slides through the data.

**Intuition**: Maintain a window of elements and slide it through the array/string while tracking some condition.

**Types**:
- Fixed size window: Window size remains constant
- Variable size window: Window expands/contracts based on conditions

**Time Complexity**: Usually O(n) as we process each element at most twice
**Space Complexity**: Usually O(1) or O(k) where k is window size or alphabet size

**Example Problems**:
- Maximum Sum Subarray of Size K
- Longest Substring with K Distinct Characters
- Minimum Size Subarray Sum
- Longest Substring Without Repeating Characters

```python
# Find maximum sum subarray of size k
def max_sum_subarray(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i-k]  # Add next element and remove first element
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

## 3. Fast and Slow Pointers
Great for cycle detection problems and solving linked list problems with constant space.

**Intuition**: Two pointers moving at different speeds will eventually meet if there's a cycle.

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Example Problems**:
- Detect Cycle in Linked List
- Find Cycle Start Point
- Find Middle of Linked List
- Palindrome Linked List
- Happy Number

```python
# Detect cycle in linked list
def has_cycle(head):
    if not head or not head.next:
        return False
        
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next       # Move one step
        fast = fast.next.next  # Move two steps
        
        if slow == fast:  # If pointers meet, cycle exists
            return True
            
    return False
```

## 4. Merge Intervals
For problems involving intervals, ranges, or merging overlapping segments.

**Intuition**: Sort intervals by start time, then process them in order, merging if necessary.

**Time Complexity**: O(n log n) due to sorting
**Space Complexity**: O(n) for storing the result

**Example Problems**:
- Merge Overlapping Intervals
- Insert Interval
- Meeting Rooms (I, II)
- Employee Free Time
- Interval List Intersections

```python
# Merge overlapping intervals
def merge(intervals):
    if not intervals:
        return []
        
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    merged = [intervals[0]]
    
    for i in range(1, len(intervals)):
        current = intervals[i]
        previous = merged[-1]
        
        # If current interval overlaps with previous
        if current[0] <= previous[1]:
            # Merge by extending the end time if needed
            previous[1] = max(previous[1], current[1])
        else:
            # No overlap, add the current interval
            merged.append(current)
            
    return merged
```

## 5. Cyclic Sort
Useful when array contains values in a given range (e.g., 1 to n). Can be used to solve missing/duplicate number problems efficiently.

**Intuition**: If values are in range 1 to n, each value should be at index (value-1).

**Time Complexity**: O(n)
**Space Complexity**: O(1) - in-place sorting

**Example Problems**:
- Missing Number
- Find All Missing Numbers
- Find the Duplicate Number
- Find All Duplicates
- First Missing Positive

```python
# Sort an array containing numbers 1 to n
def cyclic_sort(nums):
    i = 0
    while i < len(nums):
        # If the number is not at its correct position
        correct_pos = nums[i] - 1  # For 1-indexed numbers
        if nums[i] != nums[correct_pos]:
            # Swap to put it at the right place
            nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        else:
            i += 1
    return nums
```

## 6. In-place Reversal of Linked List
Reverse parts or all of a linked list without extra space.

**Intuition**: Track previous, current, and next nodes while iterating through the list to reverse pointers.

**Time Complexity**: O(n) where n is the number of nodes
**Space Complexity**: O(1)

**Example Problems**:
- Reverse a Linked List
- Reverse a Sub-list
- Reverse Nodes in k-Group
- Reverse Alternating k-Groups
- Reorder List

```python
# Reverse a linked list
def reverse_linked_list(head):
    prev = None
    current = head
    
    while current:
        next_temp = current.next  # Store next node
        current.next = prev       # Reverse current node's pointer
        prev = current            # Move prev pointer one step forward
        current = next_temp       # Move current pointer one step forward
        
    return prev  # New head of the reversed list
```

## 7. Tree BFS (Breadth-First Search)
Level-by-level traversal of trees.

**Intuition**: Use a queue to process nodes level by level, adding children as we go.

**Time Complexity**: O(n) where n is number of nodes
**Space Complexity**: O(w) where w is maximum width of tree

**Example Problems**:
- Level Order Traversal
- Zigzag Traversal
- Level Averages
- Minimum Depth
- Right Side View

```python
# Level order traversal of binary tree
from collections import deque

def level_order(root):
    result = []
    if not root:
        return result
        
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
                
        result.append(current_level)
        
    return result
```

## 8. Tree DFS (Depth-First Search)
Pre-order, in-order, and post-order traversals.

**Intuition**: Recursively explore a branch to its leaf before backtracking to explore other branches.

**Time Complexity**: O(n) where n is number of nodes
**Space Complexity**: O(h) where h is height of tree (due to recursion stack)

**Example Problems**:
- Path Sum
- All Paths for a Sum
- Binary Tree Maximum Path Sum
- Diameter of Binary Tree
- Serialize and Deserialize Binary Tree

```python
# Different tree traversals
def preorder(root, result=[]):
    if not root:
        return
    
    result.append(root.val)  # Visit root
    preorder(root.left, result)   # Visit left subtree
    preorder(root.right, result)  # Visit right subtree
    return result

def inorder(root, result=[]):
    if not root:
        return
    
    inorder(root.left, result)    # Visit left subtree
    result.append(root.val)  # Visit root
    inorder(root.right, result)   # Visit right subtree
    return result
    
def postorder(root, result=[]):
    if not root:
        return
    
    postorder(root.left, result)  # Visit left subtree
    postorder(root.right, result) # Visit right subtree
    result.append(root.val)  # Visit root
    return result
```

## 9. Two Heaps
Using two heaps to find median or manage two parts of data.

**Intuition**: Use a max-heap for smaller half and min-heap for larger half of data stream.

**Time Complexity**: O(log n) for insertions, O(1) for finding median
**Space Complexity**: O(n)

**Example Problems**:
- Find Median from Data Stream
- Sliding Window Median
- Maximum Capital
- Next Interval
- Find Right Interval

```python
# Find median from data stream
import heapq

class MedianFinder:
    def __init__(self):
        # Max heap for the smaller half
        self.small = []  # Invert values for max heap behavior
        # Min heap for the larger half
        self.large = []
        
    def addNum(self, num):
        # Always add to max heap first
        heapq.heappush(self.small, -num)
        
        # Ensure all elements in small are <= those in large
        if self.small and self.large and -self.small[0] > self.large[0]:
            heapq.heappush(self.large, -heapq.heappop(self.small))
            
        # Rebalance heaps if necessary
        if len(self.small) > len(self.large) + 1:
            heapq.heappush(self.large, -heapq.heappop(self.small))
        elif len(self.large) > len(self.small):
            heapq.heappush(self.small, -heapq.heappop(self.large))
            
    def findMedian(self):
        if len(self.small) > len(self.large):
            return -self.small[0]
        else:
            return (-self.small[0] + self.large[0]) / 2
```

## 10. Subsets (Combinatorial)
Generate all combinations and permutations.

**Intuition**: Use iterative or recursive approaches to build all possible combinations.

**Time Complexity**: O(2^n) for subsets, O(n!) for permutations
**Space Complexity**: O(2^n) for subsets, O(n!) for permutations

**Example Problems**:
- Subsets
- Permutations
- Combinations
- Letter Combinations of a Phone Number
- Palindrome Partitioning

```python
# Generate all subsets of a set
def subsets(nums):
    result = [[]]
    
    for num in nums:
        # For each existing subset, add a new subset with the current number
        result += [curr + [num] for curr in result]
        
    return result

# Generate all permutations
def permutations(nums):
    result = []
    
    def backtrack(current, remaining):
        if not remaining:
            result.append(current[:])
            return
            
        for i in range(len(remaining)):
            # Choose one element
            current.append(remaining[i])
            # Explore without the chosen element
            backtrack(current, remaining[:i] + remaining[i+1:])
            # Unchoose (backtrack)
            current.pop()
    
    backtrack([], nums)
    return result
```

## 11. Modified Binary Search
Variations of binary search for various purposes.

**Intuition**: Adapt the standard binary search to handle different scenarios.

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

**Example Problems**:
- Search in Rotated Sorted Array
- Find First and Last Position
- Search in a Sorted Infinite Array
- Peak Element
- Find Minimum in Rotated Sorted Array

```python
# Find first and last position of element in sorted array
def search_range(nums, target):
    first = binary_search(nums, target, True)
    last = binary_search(nums, target, False)
    return [first, last]

def binary_search(nums, target, find_first):
    left, right = 0, len(nums) - 1
    result = -1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] < target:
            left = mid + 1
        elif nums[mid] > target:
            right = mid - 1
        else:  # nums[mid] == target
            result = mid  # Potential answer found
            
            if find_first:
                right = mid - 1  # Continue searching on the left
            else:
                left = mid + 1   # Continue searching on the right
                
    return result
```

## 12. Top K Elements
Using heap to find top/bottom K elements.

**Intuition**: Maintain a heap of size K to efficiently track the K most extreme elements.

**Time Complexity**: O(n log k) where n is input size and k is the number of elements to find
**Space Complexity**: O(k)

**Example Problems**:
- Kth Largest Element in an Array
- K Closest Points to Origin
- Top K Frequent Elements
- Sort Characters By Frequency
- Kth Smallest Element in a Sorted Matrix

```python
import heapq

# Find K largest elements
def find_k_largest(nums, k):
    return heapq.nlargest(k, nums)
    
# Find K smallest elements
def find_k_smallest(nums, k):
    return heapq.nsmallest(k, nums)

# Find Kth largest element efficiently
def find_kth_largest(nums, k):
    # Use min heap of size k
    heap = []
    for num in nums:
        if len(heap) < k:
            heapq.heappush(heap, num)
        elif num > heap[0]:
            heapq.heapreplace(heap, num)  # Pop smallest and push new element
    return heap[0]  # Kth largest will be at the top
```

## 13. K-way Merge
Merge K sorted arrays or lists.

**Intuition**: Use a min-heap to efficiently track the smallest element across K sequences.

**Time Complexity**: O(N log k) where N is total elements and k is number of arrays
**Space Complexity**: O(k)

**Example Problems**:
- Merge K Sorted Lists
- K Pairs with Smallest Sums
- Kth Smallest Element in a Sorted Matrix
- Smallest Range Covering Elements from K Lists
- Find K Closest Elements

```python
import heapq

# Merge K sorted lists
def merge_k_lists(lists):
    dummy = ListNode(0)
    current = dummy
    # Create a min heap of (value, list_index, node)
    heap = []
    
    # Add the first node from each list to the heap
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst.val, i, lst))
    
    # Process until heap is empty
    while heap:
        val, list_idx, node = heapq.heappop(heap)
        
        # Add node to result list
        current.next = node
        current = current.next
        
        # Add next node from the same list to heap
        if node.next:
            heapq.heappush(heap, (node.next.val, list_idx, node.next))
    
    return dummy.next
```

## 14. Topological Sort
For problems involving dependencies or ordering.

**Intuition**: Process nodes with no incoming edges first, then remove their outgoing edges.

**Time Complexity**: O(V + E) where V is vertices and E is edges
**Space Complexity**: O(V + E)

**Example Problems**:
- Course Schedule
- Alien Dictionary
- Task Scheduling
- Sequence Reconstruction
- Minimum Height Trees

```python
from collections import defaultdict, deque

# Topological sorting of a directed acyclic graph
def topological_sort(vertices, edges):
    # Build adjacency list
    graph = defaultdict(list)
    in_degree = {i: 0 for i in range(vertices)}
    
    for src, dest in edges:
        graph[src].append(dest)
        in_degree[dest] += 1
    
    # Add all vertices with 0 in-degree to queue
    queue = deque()
    for vertex, degree in in_degree.items():
        if degree == 0:
            queue.append(vertex)
    
    result = []
    while queue:
        vertex = queue.popleft()
        result.append(vertex)
        
        # Reduce in-degree of neighbors
        for neighbor in graph[vertex]:
            in_degree[neighbor] -= 1
            # If in-degree becomes 0, add to queue
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    # If topological sort is possible, result will have all vertices
    return result if len(result) == vertices else []
```

## 15. Dynamic Programming
Solve optimization problems by breaking them into simpler subproblems.

**Intuition**: Store solutions to subproblems to avoid recalculating them.

**Types**:
- Top-down (Memoization): Recursive with caching
- Bottom-up (Tabulation): Iterative building from base cases

**Time Complexity**: Varies by problem, often O(n²) or O(n*m)
**Space Complexity**: Typically O(n) or O(n²)

**Example Problems**:
- Fibonacci Number
- Climbing Stairs
- Coin Change
- Longest Increasing Subsequence
- Knapsack Problem

```python
# Fibonacci using dynamic programming
def fibonacci(n):
    # Bottom-up approach
    dp = [0] * (n + 1)
    dp[0], dp[1] = 0, 1
    
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
        
    return dp[n]

# Knapsack problem
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                # Max of including vs excluding current item
                dp[i][w] = max(values[i-1] + dp[i-1][w-weights[i-1]], dp[i-1][w])
            else:
                # Can't include current item
                dp[i][w] = dp[i-1][w]
                
    return dp[n][capacity]
```

## 16. Trie (Prefix Tree)
For efficient retrieval of strings with common prefixes.

**Intuition**: Tree structure where each node represents a character in a word.

**Time Complexity**: O(m) for insert/search where m is word length
**Space Complexity**: O(n*m) where n is number of words, m is average word length

**Example Problems**:
- Implement Trie (Prefix Tree)
- Word Search II
- Design Add and Search Words Data Structure
- Replace Words
- Maximum XOR of Two Numbers in an Array

```python
# Implementation of Trie
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
    
    def starts_with(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

## 17. Union Find (Disjoint Set)
Efficiently tracks connected components in a graph.

**Intuition**: Represent elements as part of sets, with efficient operations for union and finding representatives.

**Time Complexity**: 
- Find: Amortized O(α(n)) ≈ O(1)
- Union: Amortized O(α(n)) ≈ O(1)
  where α is the inverse Ackermann function (grows extremely slowly)

**Space Complexity**: O(n)

**Example Problems**:
- Number of Connected Components
- Redundant Connection
- Accounts Merge
- Largest Component Size by Common Factor
- Regions Cut By Slashes

```python
# Union Find implementation
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            # Path compression
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        
        if root_x == root_y:
            return False
        
        # Union by rank
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1
            
        return True
```

## 18. Bit Manipulation
Solving problems using bitwise operations.

**Intuition**: Use binary operations (AND, OR, XOR, shifts) for efficient calculations.

**Time Complexity**: Typically O(1) or O(n)
**Space Complexity**: Typically O(1)

**Example Problems**:
- Single Number
- Counting Bits
- Hamming Distance
- Reverse Bits
- Power of Two

```python
# Count set bits in an integer
def count_set_bits(n):
    count = 0
    while n:
        n &= (n - 1)  # Clear the least significant bit set
        count += 1
    return count

# Check if number is power of two
def is_power_of_two(n):
    return n > 0 and (n & (n - 1)) == 0

# Find missing number in array containing 0 to n
def missing_number(nums):
    missing = len(nums)
    for i, num in enumerate(nums):
        missing ^= i ^ num
    return missing
```

## 19. Greedy Algorithms
Make locally optimal choices at each stage.

**Intuition**: At each step, choose the option that looks best at the moment.

**Time Complexity**: Varies by problem, often O(n log n) due to sorting
**Space Complexity**: Usually O(1) or O(n)

**Example Problems**:
- Jump Game
- Gas Station
- Task Scheduler
- Non-overlapping Intervals
- Minimum Number of Arrows to Burst Balloons

```python
# Activity selection problem
def max_activities(start, finish):
    # Sort activities by finish time
    activities = sorted(zip(start, finish), key=lambda x: x[1])
    
    selected = [activities[0]]
    last_finish_time = activities[0][1]
    
    for i in range(1, len(activities)):
        # If this activity starts after the last selected activity finishes
        if activities[i][0] >= last_finish_time:
            selected.append(activities[i])
            last_finish_time = activities[i][1]
            
    return selected
```

## 20. Divide and Conquer
Break problem into non-overlapping subproblems.

**Intuition**: Recursively break a problem into smaller problems, solve them independently, and combine solutions.

**Time Complexity**: Often O(n log n)
**Space Complexity**: Often O(log n) due to recursion

**Example Problems**:
- Merge Sort
- Quick Sort
- Maximum Subarray
- Count of Smaller Numbers After Self
- Skyline Problem

```python
# Merge sort
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
        
    # Divide
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    # Conquer
    return merge(left, right)
    
def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
            
    # Add remaining elements
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

## 21. Backtracking
Build solutions incrementally and abandon paths that fail to satisfy constraints.

**Intuition**: Explore all possible solutions, backtracking when a dead end is reached.

**Time Complexity**: Often exponential O(b^d) where b is branching factor and d is depth
**Space Complexity**: O(d) for recursion stack depth

**Example Problems**:
- N-Queens
- Sudoku Solver
- Combination Sum
- Word Search
- Generate Parentheses

```python
# N-Queens problem
def solve_n_queens(n):
    result = []
    
    def is_safe(board, row, col):
        # Check column
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        
        # Check upper left diagonal
        for i, j in zip(range(row-1, -1, -1), range(col-1, -1, -1)):
            if board[i][j] == 'Q':
                return False
        
        # Check upper right diagonal
        for i, j in zip(range(row-1, -1, -1), range(col+1, n)):
            if board[i][j] == 'Q':
                return False
                
        return True
    
    def backtrack(board, row):
        if row == n:
            result.append([''.join(row) for row in board])
            return
            
        for col in range(n):
            if is_safe(board, row, col):
                board[row][col] = 'Q'
                backtrack(board, row + 1)
                board[row][col] = '.'  # Backtrack
    
    # Initialize empty board
    board = [['.' for _ in range(n)] for _ in range(n)]
    backtrack(board, 0)
    return result
```

## 22. Graph Algorithms
Solve problems involving graphs.

**Intuition**: Use BFS, DFS, and specialized algorithms to traverse and search graphs efficiently.

**Time Complexity**: Varies by algorithm, BFS/DFS is O(V + E)
**Space Complexity**: Typically O(V)

**Example Problems**:
- Clone Graph
- Number of Islands
- Word Ladder
- Course Schedule
- Network Delay Time

```python
from collections import defaultdict, deque

# BFS for shortest path in unweighted graph
def shortest_path(graph, start, end):
    if start == end:
        return [start]
        
    visited = set([start])
    queue = deque([(start, [start])])  # (node, path)
    
    while queue:
        node, path = queue.popleft()
        
        for neighbor in graph[node]:
            if neighbor == end:
                return path + [neighbor]
                
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
                
    return None  # No path found

# Dijkstra's algorithm for shortest path in weighted graph
import heapq

def dijkstra(graph, start):
    # Initialize distances
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    priority_queue = [(0, start)]
    
    while priority_queue:
        current_distance, current_node = heapq.heappop(priority_queue)
        
        # If we've already found a shorter path, skip
        if current_distance > distances[current_node]:
            continue
            
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            
            # If we found a shorter path
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(priority_queue, (distance, neighbor))
                
    return distances
```

## 23. Monotonic Stack/Queue
Maintain monotonicity for efficient operations.

**Intuition**: Keep elements in increasing or decreasing order to solve next greater/smaller element problems.

**Time Complexity**: O(n) - each element is pushed and popped at most once
**Space Complexity**: O(n) in worst case

**Example Problems**:
- Next Greater Element
- Daily Temperatures
- Largest Rectangle in Histogram
- Remove K Digits
- Trapping Rain Water

```python
# Next greater element using monotonic stack
def next_greater_element(nums):
    result = [-1] * len(nums)  # Default to -1 (no next greater element)
    stack = []  # Store indices
    
    for i in range(len(nums)):
        # While stack has elements and current number is greater than top of stack
        while stack and nums[i] > nums[stack[-1]]:
            result[stack.pop()] = nums[i]  # Update result for popped index
        stack.append(i)
    
    return result
```

## 24. Line Sweep
Process events in order along a dimension.

**Intuition**: Sort events by position and process them in order, tracking state as we go.

**Time Complexity**: O(n log n) due to sorting
**Space Complexity**: O(n)

**Example Problems**:
- Maximum Number of Events That Can Be Attended
- Meeting Rooms II
- The Skyline Problem
- Rectangle Area
- Car Fleet

```python
# Find maximum number of overlapping intervals
def max_overlaps(intervals):
    # Create events: 1 for start, -1 for end
    events = []
    for start, end in intervals:
        events.append((start, 1))
        events.append((end, -1))
    
    # Sort events by time, then by event type (end before start)
    events.sort(key=lambda x: (x[0], x[1]))
    
    current = 0
    max_overlap = 0
    
    for time, event_type in events:
        current += event_type
        max_overlap = max(max_overlap, current)
    
    return max_overlap
```

## 25. Prefix Sum
Precompute cumulative sums for efficient range queries.

**Intuition**: Calculate running sum to answer range sum queries in O(1) time.

**Time Complexity**: O(n) for preprocessing, O(1) for queries
**Space Complexity**: O(n)

**Example Problems**:
- Range Sum Query
- Continuous Subarray Sum
- Maximum Size Subarray Sum Equals K
- Subarray Sum Equals K
- Random Pick with Weight

```python
# Calculate prefix sum array
def prefix_sum(nums):
    prefix = [0] * (len(nums) + 1)
    for i in range(len(nums)):
        prefix[i + 1] = prefix[i] + nums[i]
    return prefix

# Get sum of range [i, j] (inclusive)
def range_sum(prefix, i, j):
    return prefix[j + 1] - prefix[i]

# Find subarray with given sum
def subarray_with_sum(nums, target_sum):
    prefix_sum = 0
    sum_index = {0: -1}  # Map sum to index
    
    for i, num in enumerate(nums):
        prefix_sum += num
        
        # If (prefix_sum - target_sum) exists, we found our subarray
        if prefix_sum - target_sum in sum_index:
            return [sum_index[prefix_sum - target_sum] + 1, i]
            
        sum_index[prefix_sum] = i
    
    return [-1, -1]  # No solution found
```

## 26. Reservoir Sampling
Sample elements from a stream with uniform probability.

**Intuition**: Maintain a reservoir of k elements, replacing them with decreasing probability as stream grows.

**Time Complexity**: O(n)
**Space Complexity**: O(k)

**Example Problems**:
- Random Pick Index
- Linked List Random Node
- Random Pick with Weight
- Random Point in Non-overlapping Rectangles
- Random Pick with Blacklist

```python
import random

# Reservoir sampling to select k elements from a stream
def reservoir_sample(stream, k):
    reservoir = []
    stream_count = 0
    
    for item in stream:
        stream_count += 1
        
        # Fill the reservoir until we have k elements
        if len(reservoir) < k:
            reservoir.append(item)
        else:
            # Replace elements with decreasing probability
            j = random.randrange(stream_count)
            if j < k:
                reservoir[j] = item
                
    return reservoir
```

## 27. Binary Indexed Tree (Fenwick Tree)
Efficiently update elements and calculate prefix sums.

**Intuition**: Represent array as a tree where each node stores sum of some range, enabling efficient updates and queries.

**Time Complexity**: O(log n) for update and query
**Space Complexity**: O(n)

**Example Problems**:
- Range Sum Query - Mutable
- Count of Smaller Numbers After Self
- Count of Range Sum
- Create Sorted Array through Instructions
- Reverse Pairs

```python
# Binary Indexed Tree implementation
class FenwickTree:
    def __init__(self, n):
        self.size = n
        self.tree = [0] * (n + 1)
    
    # Add value at position idx
    def update(self, idx, val):
        while idx <= self.size:
            self.tree[idx] += val
            idx += idx & -idx  # Next position: Add least significant bit
    
    # Get sum from 1 to idx
    def query(self, idx):
        total = 0
        while idx > 0:
            total += self.tree[idx]
            idx -= idx & -idx  # Previous position: Remove least significant bit
        return total
    
    # Get sum from start to end (inclusive)
    def range_sum(self, start, end):
        return self.query(end) - self.query(start - 1)
```

## 28. LRU Cache
Implement a cache with O(1) access and eviction.

**Intuition**: Combine hash map for O(1) lookup with doubly linked list for O(1) insertion/deletion.

**Time Complexity**: O(1) for both get and put
**Space Complexity**: O(capacity)

**Example Problems**:
- LRU Cache
- LFU Cache
- Design In-Memory File System
- First Unique Number
- Design Most Recently Used Queue

```python
# LRU Cache implementation
class LRUCache:
    class Node:
        def __init__(self, key, val):
            self.key = key
            self.val = val
            self.prev = None
            self.next = None
    
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = {}  # Map key to node
        
        # Dummy head and tail nodes
        self.head = self.Node(0, 0)
        self.tail = self.Node(0, 0)
        self.head.next = self.tail
        self.tail.prev = self.head
    
    # Add node after head
    def _add(self, node):
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node
    
    # Remove a node from the list
    def _remove(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev
    
    def get(self, key):
        if key in self.cache:
            # Move to front (most recently used)
            node = self.cache[key]
            self._remove(node)
            self._add(node)
            return node.val
        return -1
    
    def put(self, key, value):
        # Remove if key exists
        if key in self.cache:
            self._remove(self.cache[key])
        
        # Add new node
        node = self.Node(key, value)
        self._add(node)
        self.cache[key] = node
        
        # Evict least recently used if over capacity
        if len(self.cache) > self.capacity:
            lru = self.tail.prev
            self._remove(lru)
            del self.cache[lru.key]
```

## 29. Segment Tree
Query and update intervals efficiently.

**Intuition**: Tree structure where each node represents a range of the array, supporting range queries and updates.

**Time Complexity**: O(log n) for query and update
**Space Complexity**: O(n)

**Example Problems**:
- Range Sum Query - Mutable
- Range Minimum Query
- Range Maximum Query
- Count of Range Sum
- Rectangle Area II

```python
# Segment Tree for range minimum query
class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        # Height of segment tree
        self.height = (self.n - 1).bit_length()
        # Maximum size of segment tree
        self.max_size = 2 * (1 << self.height) - 1
        # Initialize with infinity
        self.tree = [float('inf')] * self.max_size
        self._build(arr, 0, 0, self.n - 1)
    
    def _build(self, arr, index, start, end):
        # Leaf node
        if start == end:
            self.tree[index] = arr[start]
            return arr[start]
        
        mid = (start + end) // 2
        left = self._build(arr, 2 * index + 1, start, mid)
        right = self._build(arr, 2 * index + 2, mid + 1, end)
        self.tree[index] = min(left, right)
        return self.tree[index]
    
    def query(self, start, end):
        return self._query(0, 0, self.n - 1, start, end)
    
    def _query(self, index, seg_start, seg_end, qry_start, qry_end):
        # Complete overlap
        if qry_start <= seg_start and seg_end <= qry_end:
            return self.tree[index]
        
        # No overlap
        if seg_end < qry_start or qry_end < seg_start:
            return float('inf')
        
        # Partial overlap - query both children
        mid = (seg_start + seg_end) // 2
        left = self._query(2 * index + 1, seg_start, mid, qry_start, qry_end)
        right = self._query(2 * index + 2, mid + 1, seg_end, qry_start, qry_end)
        return min(left, right)
    
    def update(self, pos, value):
        self._update(0, 0, self.n - 1, pos, value)
    
    def _update(self, index, seg_start, seg_end, pos, value):
        # Reach the leaf
        if seg_start == seg_end:
            self.tree[index] = value
            return
        
        mid = (seg_start + seg_end) // 2
        if pos <= mid:
            self._update(2 * index + 1, seg_start, mid, pos, value)
        else:
            self._update(2 * index + 2, mid + 1, seg_end, pos, value)
            
        # Update current node with children's values
        self.tree[index] = min(self.tree[2 * index + 1], self.tree[2 * index + 2])
```

## 30. String Algorithms
Efficient string matching and manipulation.

**Intuition**: Specialized algorithms for string operations to avoid naive O(n²) approaches.

**Time Complexity**: KMP is O(n+m), Rabin-Karp average case O(n+m)
**Space Complexity**: O(m) where m is pattern length

**Example Problems**:
- Implement strStr()
- Longest Duplicate Substring
- Shortest Palindrome
- Distinct Subsequences
- Minimum Window Substring

```python
# KMP Pattern Matching Algorithm
def kmp_search(text, pattern):
    if not pattern:
        return 0
    
    # Build failure function
    failure = [0] * len(pattern)
    j = 0
    
    for i in range(1, len(pattern)):
        while j > 0 and pattern[i] != pattern[j]:
            j = failure[j-1]
        if pattern[i] == pattern[j]:
            j += 1
        failure[i] = j
    
    # Match pattern
    matches = []
    j = 0
    
    for i in range(len(text)):
        while j > 0 and text[i] != pattern[j]:
            j = failure[j-1]
        if text[i] == pattern[j]:
            j += 1
        if j == len(pattern):
            matches.append(i - j + 1)
            j = failure[j-1]
    
    return matches

# Rabin-Karp Algorithm
def rabin_karp(text, pattern):
    n, m = len(text), len(pattern)
    if m > n:
        return []
    
    # Prime number for hash
    q = 101
    d = 256  # Number of characters
    
    # Calculate hash for pattern and first window
    p_hash = 0
    t_hash = 0
    h = 1
    
    # Calculate h = d^(m-1) % q
    for i in range(m-1):
        h = (h * d) % q
    
    # Calculate hash for pattern and first window
    for i in range(m):
        p_hash = (d * p_hash + ord(pattern[i])) % q
        t_hash = (d * t_hash + ord(text[i])) % q
    
    matches = []
    
    # Slide pattern over text
    for i in range(n - m + 1):
        # Check if hashes match
        if p_hash == t_hash:
            # Verify character by character
            match = True
            for j in range(m):
                if text[i+j] != pattern[j]:
                    match = False
                    break
            if match:
                matches.append(i)
        
        # Calculate hash for next window
        if i < n - m:
            t_hash = (d * (t_hash - ord(text[i]) * h) + ord(text[i+m])) % q
            # Handle negative hash
            if t_hash < 0:
                t_hash += q
    
    return matches
```
