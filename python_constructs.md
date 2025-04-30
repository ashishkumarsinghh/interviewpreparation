# Python Constructs for Coding Interviews

## Core Data Structures

### 1. Lists
```python
# Initialize
arr = [1, 2, 3, 4, 5]

# Access: O(1)
first_element = arr[0]
last_element = arr[-1]

# Slice: O(k) where k is the slice size
sub_array = arr[1:4]  # [2, 3, 4]

# Insert: O(n)
arr.append(6)  # [1, 2, 3, 4, 5, 6]
arr.insert(1, 10)  # [1, 10, 2, 3, 4, 5, 6]

# Remove: O(n)
arr.pop()  # Remove last element: O(1)
arr.pop(0)  # Remove first element: O(n)
arr.remove(3)  # Remove first occurrence of value 3: O(n)

# Check existence: O(n)
contains_3 = 3 in arr  # True or False

# Length: O(1)
length = len(arr)

# Sort: O(n log n)
arr.sort()  # In-place
sorted_arr = sorted(arr)  # Returns new list
arr.sort(reverse=True)  # Descending order
```

### 2. Dictionaries (Hash Maps)
```python
# Initialize
dict1 = {}
dict2 = {"key1": "value1", "key2": "value2"}

# Access: O(1) average
value = dict2["key1"]
safe_value = dict2.get("key3", "default")  # Returns default if key doesn't exist

# Insert/Update: O(1) average
dict2["key3"] = "value3"
dict2.update({"key4": "value4", "key5": "value5"})

# Remove: O(1) average
del dict2["key1"]
value = dict2.pop("key2")  # Removes and returns value

# Check if key exists: O(1)
exists = "key3" in dict2

# Iterate
for key in dict2:
    print(key, dict2[key])

for key, value in dict2.items():
    print(key, value)

# Dictionary comprehension
squares = {x: x*x for x in range(6)}  # {0:0, 1:1, 2:4, 3:9, 4:16, 5:25}
```

### 3. Sets
```python
# Initialize
set1 = set()
set2 = {1, 2, 3, 4, 5}

# Add: O(1) average
set1.add(1)
set1.add(2)

# Remove: O(1) average
set1.remove(1)  # Raises KeyError if not found
set1.discard(1)  # No error if not found

# Set operations
set_a = {1, 2, 3, 4}
set_b = {3, 4, 5, 6}

union = set_a | set_b  # {1, 2, 3, 4, 5, 6}
intersection = set_a & set_b  # {3, 4}
difference = set_a - set_b  # {1, 2}
symmetric_diff = set_a ^ set_b  # {1, 2, 5, 6}

# Check membership: O(1)
contains = 1 in set_a

# Set comprehension
even_squares = {x*x for x in range(10) if x % 2 == 0}
```

### 4. Tuples (Immutable)
```python
# Initialize
tuple1 = ()
tuple2 = (1, 2, 3)
single_item_tuple = (1,)  # Comma is needed for single item tuples

# Access: O(1)
item = tuple2[1]  # 2

# Cannot modify tuples
# tuple2[0] = 5  # TypeError

# Unpacking
a, b, c = tuple2  # a=1, b=2, c=3
```

### 5. Deque (Double-ended Queue)
```python
from collections import deque

# Initialize
queue = deque()
queue = deque([1, 2, 3])

# Add: O(1)
queue.append(4)  # [1, 2, 3, 4]
queue.appendleft(0)  # [0, 1, 2, 3, 4]

# Remove: O(1)
queue.pop()  # Remove from right -> [0, 1, 2, 3]
queue.popleft()  # Remove from left -> [1, 2, 3]

# Rotate
queue.rotate(1)  # Rotate right -> [3, 1, 2]
queue.rotate(-1)  # Rotate left -> [1, 2, 3]
```

### 6. Heaps (Priority Queues)
```python
import heapq

# Initialize (min-heap)
heap = []
nums = [3, 1, 4, 1, 5, 9, 2, 6]

# Create heap: O(n)
heapq.heapify(nums)  # In-place

# Add: O(log n)
heapq.heappush(heap, 4)
heapq.heappush(heap, 1)
heapq.heappush(heap, 3)

# Remove min element: O(log n)
min_element = heapq.heappop(heap)  # 1

# Peek min element without removing: O(1)
min_element = heap[0]  # 3

# Max-heap workaround (negate values)
max_heap = []
heapq.heappush(max_heap, -1)
heapq.heappush(max_heap, -5)
heapq.heappush(max_heap, -3)
max_element = -heapq.heappop(max_heap)  # 5

# Get n largest/smallest elements
largest = heapq.nlargest(3, nums)  # [9, 6, 5]
smallest = heapq.nsmallest(3, nums)  # [1, 1, 2]
```

### 7. Counter
```python
from collections import Counter

# Initialize
counter = Counter([1, 2, 3, 1, 2, 1])  # {1: 3, 2: 2, 3: 1}
counter = Counter("hello")  # {'h': 1, 'e': 1, 'l': 2, 'o': 1}

# Access count: O(1)
count_of_1 = counter[1]  # 3
count_of_x = counter['x']  # 0 (returns 0 for non-existent keys)

# Update counts
counter.update([1, 1, 1])  # {1: 6, 2: 2, 3: 1}

# Most common elements
most_common = counter.most_common(2)  # [(1, 6), (2, 2)]

# Convert to dict if needed
count_dict = dict(counter)
```

### 8. DefaultDict
```python
from collections import defaultdict

# Initialize with default factory
int_dict = defaultdict(int)  # Default value is 0
list_dict = defaultdict(list)  # Default value is []
set_dict = defaultdict(set)  # Default value is set()

# Usage
int_dict['a'] += 1  # No KeyError, creates key with default 0 first
list_dict['b'].append(1)  # No KeyError, creates key with empty list first
set_dict['c'].add(2)  # No KeyError, creates key with empty set first

# Result
# int_dict = {'a': 1}
# list_dict = {'b': [1]}
# set_dict = {'c': {2}}
```

## List Comprehensions and Functional Programming

### 9. List Comprehensions
```python
# Basic list comprehension
squares = [x*x for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# With condition
even_squares = [x*x for x in range(10) if x % 2 == 0]  # [0, 4, 16, 36, 64]

# Nested list comprehension (flattening)
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [num for row in matrix for num in row]  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Nested list comprehension (transforming)
transposed = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
# [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

# Conditional expressions
result = ["even" if x % 2 == 0 else "odd" for x in range(5)]
# ["even", "odd", "even", "odd", "even"]
```

### 10. Map, Filter, Reduce
```python
# Map function
numbers = [1, 2, 3, 4, 5]
squares = list(map(lambda x: x*x, numbers))  # [1, 4, 9, 16, 25]

# Filter function
evens = list(filter(lambda x: x % 2 == 0, numbers))  # [2, 4]

# Reduce function
from functools import reduce
product = reduce(lambda x, y: x * y, numbers)  # 120 (1*2*3*4*5)
```

### 11. Generator Expressions
```python
# Generator expression (more memory efficient than list comprehension)
gen = (x*x for x in range(10))

# Usage
for square in gen:
    print(square)

# Convert to list if needed
squares = list((x*x for x in range(10)))
```

### 12. Enumerate
```python
fruits = ['apple', 'banana', 'cherry']

# Basic usage
for i, fruit in enumerate(fruits):
    print(i, fruit)  # 0 apple, 1 banana, 2 cherry

# With custom start index
for i, fruit in enumerate(fruits, start=1):
    print(i, fruit)  # 1 apple, 2 banana, 3 cherry
```

### 13. Zip
```python
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 35]
cities = ['New York', 'London', 'Paris']

# Basic zip
for name, age in zip(names, ages):
    print(name, age)  # Alice 25, Bob 30, Charlie 35

# Multiple iterables
for name, age, city in zip(names, ages, cities):
    print(name, age, city)  # Alice 25 New York, Bob 30 London, Charlie 35 Paris

# Convert to list of tuples
combined = list(zip(names, ages, cities))
# [('Alice', 25, 'New York'), ('Bob', 30, 'London'), ('Charlie', 35, 'Paris')]

# Unzip
names_copy, ages_copy, cities_copy = zip(*combined)
```

## Advanced Python Features

### 21. Collections Module
```python
from collections import namedtuple, OrderedDict

# NamedTuple (immutable with named fields)
Point = namedtuple('Point', ['x', 'y'])
p1 = Point(1, 2)
print(p1.x, p1.y)  # 1 2

# OrderedDict (maintains insertion order) - less useful since Python 3.7
od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3
od.move_to_end('a')  # Move to end
od.popitem(last=False)  # Remove first item
```

### 22. Itertools Module
```python
import itertools

# Combinations
combinations = list(itertools.combinations([1, 2, 3, 4], 2))
# [(1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]

# Permutations
permutations = list(itertools.permutations([1, 2, 3], 2))
# [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]

# Product
product = list(itertools.product([1, 2], ['a', 'b']))
# [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]

# Count (infinite)
counter = itertools.count(start=10, step=2)
# 10, 12, 14, 16, ...

# Cycle (infinite)
cycler = itertools.cycle([1, 2, 3])
# 1, 2, 3, 1, 2, 3, ...

# Chain (combine multiple iterables)
combined = list(itertools.chain([1, 2], [3, 4], [5, 6]))
# [1, 2, 3, 4, 5, 6]

# GroupBy
data = [('a', 1), ('a', 2), ('b', 1), ('b', 2)]
for key, group in itertools.groupby(data, key=lambda x: x[0]):
    print(key, list(group))
# a [('a', 1), ('a', 2)]
# b [('b', 1), ('b', 2)]
```

### 23. Bisect Module (Binary Search Operations)
```python
import bisect

# Create a sorted list
sorted_list = [1, 4, 6, 8, 10]

# Find insertion point (maintains sorted order)
index = bisect.bisect_left(sorted_list, 5)  # 2
bisect.insort_left(sorted_list, 5)  # [1, 4, 5, 6, 8, 10]

# bisect_left vs bisect_right (bisect)
# bisect_left: first position where element can go
# bisect_right: last position where element can go
index_left = bisect.bisect_left(sorted_list, 5)  # 2
index_right = bisect.bisect_right(sorted_list, 5)  # 3
```

### 24. Custom Sort Key
```python
# Sort by length
words = ['apple', 'banana', 'cherry', 'date']
words.sort(key=len)  # ['date', 'apple', 'cherry', 'banana']

# Sort by multiple criteria
people = [
    {'name': 'Alice', 'age': 25},
    {'name': 'Bob', 'age': 25},
    {'name': 'Charlie', 'age': 20}
]

# Sort by age, then name
people.sort(key=lambda x: (x['age'], x['name']))
# [{'name': 'Charlie', 'age': 20}, {'name': 'Alice', 'age': 25}, {'name': 'Bob', 'age': 25}]

# Sort by age ascending, name descending
people.sort(key=lambda x: (x['age'], -ord(x['name'][0])))

# Using itemgetter for better performance
from operator import itemgetter
people.sort(key=itemgetter('age', 'name'))
```

### 25. Caching with @lru_cache
```python
from functools import lru_cache

# Memoization with lru_cache
@lru_cache(maxsize=None)  # Unlimited cache size
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Usage
result = fibonacci(100)  # Very fast due to caching

# Clear cache if needed
fibonacci.cache_clear()
```

### 26. Context Managers
```python
# File handling with context manager (auto-closes file)
with open('file.txt', 'w') as f:
    f.write('Hello')
    
# Custom context manager
from contextlib import contextmanager

@contextmanager
def timer():
    import time
    start = time.time()
    yield
    end = time.time()
    print(f"Time elapsed: {end - start:.2f} seconds")

# Usage
with timer():
    # Code to time
    result = sum(range(1000000))
```

### 27. Lambda Functions
```python
# Simple lambda
square = lambda x: x * x
print(square(4))  # 16

# Immediately invoked
result = (lambda x, y: x + y)(5, 3)  # 8

# With higher order functions
numbers = [1, 2, 3, 4, 5]
evens = list(filter(lambda x: x % 2 == 0, numbers))  # [2, 4]
doubled = list(map(lambda x: x * 2, numbers))  # [2, 4, 6, 8, 10]
total = reduce(lambda x, y: x + y, numbers)  # 15
```

### 28. Generators and Yield
```python
# Simple generator function
def count_up_to(max):
    count = 1
    while count <= max:
        yield count
        count += 1

# Usage
for num in count_up_to(5):
    print(num)  # 1, 2, 3, 4, 5

# Generator pipeline (memory efficient)
def read_large_file(file_path):
    with open(file_path) as f:
        for line in f:
            yield line.strip()

def grep(pattern, lines):
    for line in lines:
        if pattern in line:
            yield line

# Usage (processes one line at a time)
file_lines = read_large_file('large_file.txt')
matching_lines = grep('important', file_lines)
```

### 29. Decorators
```python
# Simple decorator
def timing_decorator(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.2f} seconds")
        return result
    return wrapper

# Usage
@timing_decorator
def slow_function(n):
    import time
    time.sleep(n)
    return n

result = slow_function(1)  # slow_function took 1.00 seconds
```

### 30. Object-Oriented Patterns
```python
# Class with properties
class Person:
    def __init__(self, name, age):
        self._name = name
        self._age = age
        
    @property
    def name(self):
        return self._name
        
    @name.setter
    def name(self, value):
        if not value:
            raise ValueError("Name cannot be empty")
        self._name = value
        
    @property
    def age(self):
        return self._age
        
    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value

# Magic methods (dunder methods)
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        
    def __repr__(self):
        return f"Point({self.x}, {self.y})"
        
    def __str__(self):
        return f"({self.x}, {self.y})"
        
    def __eq__(self, other):
        if not isinstance(other, Point):
            return False
        return self.x == other.x and self.y == other.y
        
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
```

## Python Interview Tips and Pitfalls

### Time Complexity Awareness
- Always analyze and be prepared to discuss the time and space complexity of your solutions.
- Know the complexity of common operations for Python's built-in data structures:
  - List indexing: O(1), insertion/deletion: O(n)
  - Dict/Set lookup/insertion/deletion: O(1) average, O(n) worst case
  - Sorting: O(n log n)

### Common Pitfalls to Avoid

1. **Mutable Default Arguments**
   ```python
   # BAD
   def append_to_list(item, my_list=[]):
       my_list.append(item)
       return my_list
   
   # GOOD
   def append_to_list(item, my_list=None):
       if my_list is None:
           my_list = []
       my_list.append(item)
       return my_list
   ```

2. **Late Binding Closures**
   ```python
   # BAD
   funcs = []
   for i in range(5):
       funcs.append(lambda: i)  # All functions will refer to the same i
   
   # GOOD
   funcs = []
   for i in range(5):
       funcs.append(lambda x=i: x)  # Captures current value of i
   ```

3. **Copy vs Deepcopy**
   ```python
   import copy
   
   # Shallow copy
   list1 = [[1, 2], [3, 4]]
   list2 = list1.copy()  # or list(list1) or list1[:]
   list2[0][0] = 99  # Changes list1 too!
   
   # Deep copy
   list3 = copy.deepcopy(list1)
   list3[0][0] = 100  # Doesn't affect list1
   ```

4. **Using `is` vs `==`**
   ```python
   # `is` checks if objects are the same object (identity)
   # `==` checks if objects have the same value (equality)
   
   a = [1, 2, 3]
   b = [1, 2, 3]
   c = a
   
   print(a == b)  # True (same values)
   print(a is b)  # False (different objects)
   print(a is c)  # True (same object)
   ```

### Interview Best Practices

1. **Talk Through Your Approach**
   - Explain your thinking process before coding
   - Discuss trade-offs between different approaches
   - Consider time and space complexity

2. **Start with Brute Force**
   - Begin with a simple working solution
   - Optimize incrementally
   - Use helper functions to break down complex logic

3. **Test Your Code**
   - Include edge cases: empty inputs, single elements, etc.
   - Trace through with small examples
   - Check for off-by-one errors

4. **Use Python's Built-in Features Wisely**
   - Collections module for specialized data structures
   - List/dict comprehensions for concise code
   - Enumerate, zip, and other Pythonic constructs

5. **Handle Errors Gracefully**
   - Check for None values and empty collections
   - Use try/except when appropriate
   - Validate inputs when necessary

6. **Know When to Use Which Data Structure**
   - Lists: Ordered collections with random access
   - Sets: Fast membership testing, removing duplicates
   - Dictionaries: Key-value lookups
   - Heaps: Priority queue operations
   - Deques: Efficient operations at both ends

7. **Keep Code Readable**
   - Use meaningful variable names
   - Add comments for complex logic
   - Prefer clarity over extreme brevity

### Additional Python Tips for Interviews

1. **String Manipulation**
   ```python
   # Join strings (efficient)
   words = ["Hello", "world"]
   sentence = " ".join(words)  # "Hello world"
   
   # Split string
   parts = sentence.split()  # ["Hello", "world"]
   
   # String methods
   s = "  Hello, World!  "
   s.strip()  # "Hello, World!"
   s.lower()  # "  hello, world!  "
   s.replace(",", "")  # "  Hello World!  "
   ```

2. **Check if all/any elements satisfy a condition**
   ```python
   nums = [2, 4, 6, 8]
   all_even = all(num % 2 == 0 for num in nums)  # True
   any_over_five = any(num > 5 for num in nums)  # True
   ```

3. **Use Counter for frequency counting**
   ```python
   from collections import Counter
   
   text = "mississippi"
   freq = Counter(text)  # {'i': 4, 's': 4, 'p': 2, 'm': 1}
   most_common = freq.most_common(2)  # [('i', 4), ('s', 4)]
   ```

4. **Infinity in Python**
   ```python
   inf = float('inf')
   neg_inf = float('-inf')
   
   # Useful in algorithms
   min_value = float('inf')  # Initialize with "infinity" for min tracking
   ```

5. **Unpacking and Multiple Assignment**
   ```python
   # Swap values
   a, b = b, a
   
   # Unpack first and rest
   first, *rest = [1, 2, 3, 4]  # first=1, rest=[2, 3, 4]
   
   # Unpack first, middle and last
   first, *middle, last = [1, 2, 3, 4, 5]  # first=1, middle=[2, 3, 4], last=5
   ```

