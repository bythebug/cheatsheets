---
layout: sheet
title: DSA Cheat Sheet
description: Python 3 · Patterns · Templates · Complexity · 2026
toc: |
  - [Big O](#big-o-complexity)
  - [Arrays & Lists](#arrays--lists)
    - [Two Pointers](#two-pointers)
    - [Sliding Window](#sliding-window)
    - [Prefix Sums](#prefix-sums)
  - [Strings](#strings)
  - [Linked Lists](#linked-lists)
  - [Stacks & Queues](#stacks--queues)
    - [Monotonic Stack](#monotonic-stack)
    - [Monotonic Deque](#monotonic-deque)
  - [Hash Tables](#hash-tables)
  - [Trees](#trees)
    - [Traversals](#traversals)
    - [BST / LCA](#bst--lca)
  - [Tries](#tries)
  - [Heaps](#heaps)
    - [Two Heaps / Median](#two-heaps--median)
  - [Graphs](#graphs)
    - [DFS](#dfs)
    - [BFS](#bfs)
    - [Dijkstra & Topo Sort](#dijkstra--topological-sort)
    - [Union-Find](#union-find)
  - [Binary Search](#binary-search)
  - [Dynamic Programming](#dynamic-programming)
    - [1D DP](#1d-dp)
    - [2D DP & Knapsack](#2d-dp--knapsack)
  - [Backtracking](#backtracking)
  - [Intervals](#intervals)
  - [Bit Manipulation](#bit-manipulation)
  - [Sorting](#sorting)
  - [Pattern Guide](#pattern-guide)
---

## Big O Complexity

| Notation | Name | Example | n = 10⁶ |
|----------|------|---------|---------|
| `O(1)` | Constant | Array index | ~1 op |
| `O(log n)` | Logarithmic | Binary search | ~20 ops |
| `O(n)` | Linear | Single loop | 10⁶ ops |
| `O(n log n)` | Linearithmic | Merge sort | ~20M ops |
| `O(n²)` | Quadratic | Nested loop | 10¹² |
| `O(2ⁿ)` | Exponential | Subsets | Huge |
| `O(n!)` | Factorial | Permutations | ∞ |

<div class="tip">FAANG target: O(n) or O(n log n). Always justify O(n²) — there's usually a better way.</div>

| Structure | Access | Search | Insert | Delete |
|-----------|--------|--------|--------|--------|
| Array/List | O(1) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1) | O(1) |
| Hash Table | — | O(1)* | O(1)* | O(1)* |
| BST (balanced) | O(log n) | O(log n) | O(log n) | O(log n) |
| Heap | — | O(n) | O(log n) | O(log n) |
| Stack / Queue | — | O(n) | O(1) | O(1) |
| Trie | — | O(L) | O(L) | O(L) |

*amortized · L = key length*

---

## Arrays & Lists

### Two Pointers

<div class="note">Left + right pointers move toward each other (sorted array) or same direction (slow/fast). Turns O(n²) into O(n).</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">opposite ends — two-sum in sorted array</span></div>

```python
def two_sum_sorted(nums, target):
    l, r = 0, len(nums) - 1
    while l < r:
        s = nums[l] + nums[r]
        if   s == target: return [l, r]
        elif s < target:  l += 1
        else:             r -= 1
    return []
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">same direction — remove duplicates in-place</span></div>

```python
def remove_dups(nums):
    k = 1
    for i in range(1, len(nums)):
        if nums[i] != nums[i-1]:
            nums[k] = nums[i]; k += 1
    return k
```

</div>

### Sliding Window

<div class="note">Fixed window: slide by adding right and dropping left. Variable window: expand right always, shrink left when constraint breaks.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">fixed window — max sum of size k</span></div>

```python
def max_sum_k(nums, k):
    window = sum(nums[:k]); best = window
    for i in range(k, len(nums)):
        window += nums[i] - nums[i-k]
        best = max(best, window)
    return best
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">variable window — longest substring without repeat</span></div>

```python
def length_of_longest(s):
    seen = {}; l = best = 0
    for r, c in enumerate(s):
        if c in seen and seen[c] >= l:
            l = seen[c] + 1
        seen[c] = r
        best = max(best, r - l + 1)
    return best
```

</div>

### Prefix Sums

<div class="note">Build once O(n), then any range sum in O(1). Key: prefix[r+1] - prefix[l] = sum of nums[l..r].</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">build & query range sum</span></div>

```python
prefix = [0] * (len(nums) + 1)
for i, v in enumerate(nums):
    prefix[i+1] = prefix[i] + v

range_sum = prefix[r+1] - prefix[l]  # inclusive [l, r]
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">count subarrays with sum = k — O(n)</span></div>

```python
def subarray_sum(nums, k):
    # running total - k seen before? → found a subarray
    count = {0: 1}; total = ans = 0
    for n in nums:
        total += n
        ans += count.get(total - k, 0)
        count[total] = count.get(total, 0) + 1
    return ans
```

</div>

---

## Strings

<div class="note">Strings are immutable. Never use += in a loop — collect in list then join() at the end.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">common operations</span></div>

```python
s[::-1]               # reverse
" ".join(words)       # list → str  O(n)
s.split()             # str → list  O(n)
s.lower() / s.upper()
s.strip()             # trim whitespace
s.replace("a", "b")
s.startswith("he") / s.endswith("lo")
s.find("ll")          # index or -1
s.isalpha() / s.isdigit() / s.isalnum()
ord('a')  # 97    chr(65)  # 'A'
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">anagram, palindrome, frequency</span></div>

```python
from collections import Counter

Counter(s) == Counter(t)    # anagram  O(n)
s == s[::-1]                # palindrome

def is_palindrome(s):
    l, r = 0, len(s) - 1
    while l < r:
        if s[l] != s[r]: return False
        l += 1; r -= 1
    return True

# Efficient string build
res = []
for c in s: res.append(c)
result = "".join(res)       # O(n), not O(n²)
```

</div>

---

## Linked Lists

<div class="note">Always use a dummy head node to avoid null-pointer edge cases on head insertion/deletion.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">ListNode + reverse in-place</span></div>

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val; self.next = next

# Dummy head simplifies edge cases
dummy = ListNode(0); dummy.next = head
return dummy.next

# Reverse in-place  O(n) time  O(1) space
def reverse(head):
    prev, curr = None, head
    while curr:
        nxt = curr.next
        curr.next = prev
        prev, curr = curr, nxt
    return prev
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">middle, cycle detect, kth from end</span></div>

```python
# Middle of list
slow = fast = head
while fast and fast.next:
    slow = slow.next; fast = fast.next.next
# slow = middle

# Detect cycle  O(n) O(1)
slow = fast = head
while fast and fast.next:
    slow = slow.next; fast = fast.next.next
    if slow == fast: return True

# Kth from end (advance fast by k first)
fast = slow = head
for _ in range(k): fast = fast.next
while fast: slow = slow.next; fast = fast.next
# slow = kth from end
```

</div>

---

## Stacks & Queues

### Monotonic Stack

<div class="note">Use list as stack. Pop when current element breaks monotone order. Always stores indices, not values.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">stack ops + valid parentheses</span></div>

```python
stack = []
stack.append(x)  # push O(1)
stack.pop()      # pop  O(1)
stack[-1]        # peek O(1)

def valid_parens(s):
    pairs = {')':'(', '}':'{', ']':'['}
    stack = []
    for c in s:
        if c in pairs:
            if not stack or stack[-1] != pairs[c]:
                return False
            stack.pop()
        else: stack.append(c)
    return not stack
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">monotonic decreasing stack — next greater element</span></div>

```python
def next_greater(nums):
    res = [-1] * len(nums); stack = []
    for i, v in enumerate(nums):
        while stack and nums[stack[-1]] < v:
            res[stack.pop()] = v
        stack.append(i)
    return res
```

</div>

### Monotonic Deque

<div class="note">Deque for BFS. Monotonic deque for sliding window max/min — remove stale from front, pop smaller from back.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">deque ops</span></div>

```python
from collections import deque
q = deque()
q.append(x)       # right  O(1)
q.appendleft(x)   # left   O(1)
q.popleft()       # left   O(1)  ← use for BFS
q[0], q[-1]       # peek both ends
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">sliding window maximum — O(n)</span></div>

```python
def max_sliding_window(nums, k):
    dq = deque(); res = []
    for i, v in enumerate(nums):
        while dq and dq[0] < i-k+1: dq.popleft()
        while dq and nums[dq[-1]] < v: dq.pop()
        dq.append(i)
        if i >= k-1: res.append(nums[dq[0]])
    return res
```

</div>

<div class="warn">Never use <code>list.pop(0)</code> for queues — it's O(n). Use <code>deque.popleft()</code>.</div>

---

## Hash Tables

<div class="note">dict for O(1) lookup. defaultdict avoids KeyError. Counter for frequencies. set for O(1) membership.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">core operations</span></div>

```python
from collections import defaultdict, Counter

d.get(k, default)
d.setdefault(k, []).append(v)
k in d                   # O(1)

graph = defaultdict(list)  # adjacency list
freq  = defaultdict(int)   # frequency map

c = Counter(nums)
c.most_common(3)           # top-3

s = set(); s.add(x); s.discard(x)
s1 & s2  # intersect    s1 | s2  # union
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">two-sum, group anagrams, longest consecutive</span></div>

```python
# Two-sum  O(n)
def two_sum(nums, target):
    seen = {}
    for i, n in enumerate(nums):
        if target-n in seen: return [seen[target-n], i]
        seen[n] = i

# Group anagrams  O(n·k log k)
groups = defaultdict(list)
for s in strs: groups[tuple(sorted(s))].append(s)

# Longest consecutive  O(n) — only start from sequence start
s = set(nums); best = 0
for n in s:
    if n-1 not in s:
        length = 0
        while n+length in s: length += 1
        best = max(best, length)
```

</div>

---

## Trees

### Traversals

<div class="note">Inorder on BST = sorted output. Level-order uses a queue — track level size with for range(len(q)).</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">TreeNode + DFS + BFS</span></div>

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val=val; self.left=left; self.right=right

# Inorder L→Root→R  (BST = sorted!)
def inorder(root):
    if not root: return
    inorder(root.left); visit(root); inorder(root.right)

# Level-order / BFS
def level_order(root):
    q, res = deque([root] if root else []), []
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft(); level.append(node.val)
            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)
        res.append(level)
    return res
```

</div>

### BST / LCA

<div class="note">LCA: if p and q are in different subtrees, current node is LCA. BST validation: pass valid range (lo, hi) down.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">height, diameter, LCA, validate BST</span></div>

```python
# Height
def height(root):
    if not root: return 0
    return 1 + max(height(root.left), height(root.right))

# Diameter (post-order; track max l+r globally)
def diameter(root):
    best = [0]
    def dfs(n):
        if not n: return 0
        l, r = dfs(n.left), dfs(n.right)
        best[0] = max(best[0], l+r)
        return 1 + max(l, r)
    dfs(root); return best[0]

# LCA
def lca(root, p, q):
    if not root or root==p or root==q: return root
    l=lca(root.left,p,q); r=lca(root.right,p,q)
    return root if l and r else l or r

# Validate BST (pass valid range down)
def is_valid(root, lo=float('-inf'), hi=float('inf')):
    if not root: return True
    if not lo < root.val < hi: return False
    return is_valid(root.left,lo,root.val) and is_valid(root.right,root.val,hi)
```

</div>

---

## Tries

<div class="note">Each node stores children (dict) and is_end flag. Insert and search are O(L). Use for prefix search, autocomplete, word dictionary.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">insert · search · startsWith</span></div>

```python
class TrieNode:
    def __init__(self):
        self.children = {}; self.is_end = False

class Trie:
    def __init__(self): self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for c in word:
            node = node.children.setdefault(c, TrieNode())
        node.is_end = True

    def search(self, word):
        node = self.root
        for c in word:
            if c not in node.children: return False
            node = node.children[c]
        return node.is_end

    def starts_with(self, prefix):
        node = self.root
        for c in prefix:
            if c not in node.children: return False
            node = node.children[c]
        return True
```

</div>

---

## Heaps

<div class="note">Python's heapq is always a MIN-heap. Negate values for max-heap. Tuple heaps sort by first element.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">heapq operations + Kth largest</span></div>

```python
import heapq
heapq.heappush(h, v)        # O(log n)
heapq.heappop(h)            # O(log n) — smallest
h[0]                        # O(1) peek min
heapq.heapify(lst)          # O(n)

# Max-heap: negate values
heapq.heappush(h, -v); -heapq.heappop(h)

# Kth largest — keep min-heap of size k  O(n log k)
h = []
for n in nums:
    heapq.heappush(h, n)
    if len(h) > k: heapq.heappop(h)
# h[0] = kth largest
```

</div>

### Two Heaps / Median

<div class="note">Max-heap for lower half, min-heap for upper half. Balance so sizes differ by at most 1.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">MedianFinder — addNum O(log n), findMedian O(1)</span></div>

```python
class MedianFinder:
    def __init__(self):
        self.lo = []   # max-heap (negated) — lower half
        self.hi = []   # min-heap — upper half

    def addNum(self, num):
        heapq.heappush(self.lo, -num)
        heapq.heappush(self.hi, -heapq.heappop(self.lo))
        if len(self.hi) > len(self.lo):
            heapq.heappush(self.lo, -heapq.heappop(self.hi))

    def findMedian(self):
        if len(self.lo) > len(self.hi): return -self.lo[0]
        return (-self.lo[0] + self.hi[0]) / 2
```

</div>

---

## Graphs

<div class="note">Adjacency list is default. Grid: treat each cell as a node with 4 neighbors — check bounds before visiting.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">adjacency list + grid neighbors</span></div>

```python
graph = defaultdict(list)
graph[u].append(v)             # directed
graph[v].append(u)             # undirected
graph[u].append((v, weight))   # weighted

# Grid 4-directional neighbors
dirs = [(0,1),(0,-1),(1,0),(-1,0)]
for dr, dc in dirs:
    nr, nc = r+dr, c+dc
    if 0<=nr<rows and 0<=nc<cols:  # valid!
        pass
```

</div>

### DFS

<div class="note">Use for: paths, connectivity, cycle detection, topological order. Mark visited BEFORE pushing neighbors.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">iterative DFS + number of islands</span></div>

```python
# Iterative DFS
def dfs(graph, start):
    visited, stack = {start}, [start]
    while stack:
        node = stack.pop()
        for nei in graph[node]:
            if nei not in visited:
                visited.add(nei); stack.append(nei)

# Number of islands — flood fill
def num_islands(grid):
    rows, cols = len(grid), len(grid[0]); count = 0
    def dfs(r, c):
        if not (0<=r<rows and 0<=c<cols) or grid[r][c]!='1': return
        grid[r][c]='0'; dfs(r+1,c); dfs(r-1,c); dfs(r,c+1); dfs(r,c-1)
    for r in range(rows):
        for c in range(cols):
            if grid[r][c]=='1': dfs(r,c); count+=1
    return count
```

</div>

### BFS

<div class="note">BFS = shortest path in unweighted graphs. Multi-source BFS: seed the queue with all sources at once.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">single-source + multi-source BFS</span></div>

```python
# Shortest path  O(V+E)
def bfs(graph, start, end):
    q = deque([(start, 0)]); visited = {start}
    while q:
        node, dist = q.popleft()
        if node==end: return dist
        for nei in graph[node]:
            if nei not in visited:
                visited.add(nei); q.append((nei, dist+1))
    return -1

# Multi-source (rotten oranges, walls & gates)
q = deque(sources); visited = set(sources)
while q:
    node = q.popleft()
    for nei in graph[node]:
        if nei not in visited:
            visited.add(nei); q.append(nei)
```

</div>

### Dijkstra & Topological Sort

<div class="note">Dijkstra: always expand cheapest node. Skip stale heap entries with "if d > dist[u]: continue". Topo: if result length ≠ n, there's a cycle.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">Dijkstra O((V+E) log V)</span></div>

```python
def dijkstra(graph, src, n):
    dist = [float('inf')]*n; dist[src]=0; h=[(0,src)]
    while h:
        d, u = heapq.heappop(h)
        if d > dist[u]: continue   # stale entry
        for v, w in graph[u]:
            if dist[u]+w < dist[v]:
                dist[v]=dist[u]+w; heapq.heappush(h,(dist[v],v))
    return dist
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">Kahn's topological sort — detects cycle</span></div>

```python
def topo_sort(n, edges):
    indeg=[0]*n; g=defaultdict(list)
    for u,v in edges: g[u].append(v); indeg[v]+=1
    q=deque(i for i in range(n) if indeg[i]==0); order=[]
    while q:
        u=q.popleft(); order.append(u)
        for v in g[u]:
            indeg[v]-=1
            if indeg[v]==0: q.append(v)
    return order if len(order)==n else []
```

</div>

### Union-Find

<div class="note">Path compression + union by rank makes find() nearly O(1). count decrements on every successful union.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">Union-Find with path compression & rank</span></div>

```python
class UnionFind:
    def __init__(self, n):
        self.parent=list(range(n)); self.rank=[0]*n; self.count=n

    def find(self, x):
        if self.parent[x]!=x:
            self.parent[x]=self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        px,py=self.find(x),self.find(y)
        if px==py: return False
        if self.rank[px]<self.rank[py]: px,py=py,px
        self.parent[py]=px
        if self.rank[px]==self.rank[py]: self.rank[px]+=1
        self.count-=1; return True
```

</div>

---

## Binary Search

<div class="note">3 templates: exact match (l≤r), left-bound (l&lt;r, r=mid), answer-space (define feasible(x) and binary search on the answer range).</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">exact match O(log n)</span></div>

```python
def binary_search(nums, target):
    l, r = 0, len(nums)-1
    while l <= r:
        mid = l + (r-l)//2   # avoids overflow
        if   nums[mid]==target: return mid
        elif nums[mid]<target:  l=mid+1
        else:                   r=mid-1
    return -1
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">left-bound — first position where nums[i] ≥ target</span></div>

```python
def lower_bound(nums, target):
    l, r = 0, len(nums)   # r = len (open interval)
    while l < r:
        mid=(l+r)//2
        if nums[mid]<target: l=mid+1
        else:                r=mid
    return l
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">answer-space — "find min X where condition holds"</span></div>

```python
def solve(lo, hi, feasible):
    while lo < hi:
        mid=(lo+hi)//2
        if feasible(mid): hi=mid    # try smaller
        else:             lo=mid+1  # need more
    return lo

# bisect shortcuts
import bisect
bisect.bisect_left(a, x)   # leftmost position
bisect.bisect_right(a, x)  # rightmost position
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">search in rotated sorted array</span></div>

```python
def search_rotated(nums, target):
    l, r = 0, len(nums)-1
    while l<=r:
        mid=(l+r)//2
        if nums[mid]==target: return mid
        if nums[l]<=nums[mid]:            # left sorted
            if nums[l]<=target<nums[mid]: r=mid-1
            else:                         l=mid+1
        else:                             # right sorted
            if nums[mid]<target<=nums[r]: l=mid+1
            else:                         r=mid-1
    return -1
```

</div>

---

## Dynamic Programming

<div class="note">Ask: (1) what is the state? (2) what is the transition? (3) base cases? Top-down = recursion + cache. Bottom-up = fill table in order.</div>

### 1D DP

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">memoization template</span></div>

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def dp(i, j):          # state = (i, j)
    if base_case: return 0
    return min(dp(i-1,j), dp(i,j-1)) + cost
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">coin change, LIS, house robber</span></div>

```python
# Coin change — min coins  O(amount × |coins|)
dp=[float('inf')]*(amount+1); dp[0]=0
for coin in coins:
    for i in range(coin, amount+1):
        dp[i]=min(dp[i], dp[i-coin]+1)

# LIS  O(n log n) — patience sort via bisect
tails=[]
for n in nums:
    i=bisect.bisect_left(tails, n)
    if i==len(tails): tails.append(n)
    else: tails[i]=n
# len(tails) = LIS length

# House robber  O(n) O(1)
prev2=prev1=0
for n in nums: prev2,prev1=prev1,max(prev1,prev2+n)
```

</div>

### 2D DP & Knapsack

<div class="note">LCS: match chars → diagonal +1, else max of top/left. Knapsack: reverse inner loop (high to low) to prevent re-using same item.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">LCS + unique paths</span></div>

```python
# Longest Common Subsequence  O(m*n)
dp=[[0]*(n+1) for _ in range(m+1)]
for i in range(1,m+1):
    for j in range(1,n+1):
        if s1[i-1]==s2[j-1]: dp[i][j]=dp[i-1][j-1]+1
        else:                 dp[i][j]=max(dp[i-1][j], dp[i][j-1])

# Unique paths (grid)  O(m*n)
dp=[[1]*n for _ in range(m)]
for i in range(1,m):
    for j in range(1,n):
        dp[i][j]=dp[i-1][j]+dp[i][j-1]
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">0/1 knapsack — space-optimized 1D</span></div>

```python
# Reverse inner loop so each item used only once
dp=[0]*(capacity+1)
for w, v in zip(weights, values):
    for c in range(capacity, w-1, -1):
        dp[c]=max(dp[c], dp[c-w]+v)
```

</div>

---

## Backtracking

<div class="note">Always: choose → recurse → unchoose. Copy path when recording (path[:] not path). Sort candidates and prune with "if c > remaining: break".</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">subsets O(2ⁿ) · permutations O(n!) · combination sum</span></div>

```python
# Subsets
def subsets(nums):
    res=[]; path=[]
    def bt(start):
        res.append(path[:])
        for i in range(start,len(nums)):
            path.append(nums[i]); bt(i+1); path.pop()
    bt(0); return res

# Permutations
def permutations(nums):
    res=[]; path=[]; used=set()
    def bt():
        if len(path)==len(nums): res.append(path[:]); return
        for i,n in enumerate(nums):
            if i in used: continue
            used.add(i); path.append(n); bt(); path.pop(); used.discard(i)
    bt(); return res

# Combination sum (sorted + prune early)
def combo_sum(candidates, target):
    res=[]; candidates.sort()
    def bt(start, rem):
        if rem==0: res.append(path[:]); return
        for i in range(start,len(candidates)):
            c=candidates[i]
            if c>rem: break              # prune
            path.append(c); bt(i,rem-c); path.pop()
    path=[]; bt(0,target); return res
```

</div>

---

## Intervals

<div class="note">Sort by start time. Overlap if next.start ≤ merged.end. Min rooms = max overlapping meetings at any point.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">merge intervals O(n log n)</span></div>

```python
intervals.sort(key=lambda x: x[0])
merged=[intervals[0]]
for s, e in intervals[1:]:
    if s<=merged[-1][1]: merged[-1][1]=max(merged[-1][1],e)
    else: merged.append([s,e])
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">insert interval O(n)</span></div>

```python
def insert(intervals, new):
    res=[]; i=0; n=len(intervals)
    while i<n and intervals[i][1]<new[0]:
        res.append(intervals[i]); i+=1
    while i<n and intervals[i][0]<=new[1]:
        new[0]=min(new[0],intervals[i][0])
        new[1]=max(new[1],intervals[i][1]); i+=1
    return res+[new]+intervals[i:]
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">minimum meeting rooms — O(n log n)</span></div>

```python
def min_meeting_rooms(intervals):
    intervals.sort(); heap=[]
    for start, end in intervals:
        if heap and heap[0]<=start:
            heapq.heapreplace(heap, end)
        else:
            heapq.heappush(heap, end)
    return len(heap)
```

</div>

---

## Bit Manipulation

<div class="note">n & (n-1) clears the lowest set bit. XOR pairs cancel to 0 — use to find the single unique number.</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">fundamental operations</span></div>

```python
n & (n-1)        # clear lowest set bit
n & (-n)         # isolate lowest set bit
n & 1            # odd check (1=odd, 0=even)
(n & (n-1))==0   # power of 2? (and n > 0)
a ^ a            # 0  — XOR with self cancels
a ^ b ^ a        # b  — pair elimination

# Get / set / clear / toggle bit k
(n>>k) & 1       # get bit k
n | (1<<k)       # set bit k
n & ~(1<<k)      # clear bit k
n ^ (1<<k)       # toggle bit k

bin(n).count('1') # count set bits
```

</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">single number, missing number, count bits</span></div>

```python
from functools import reduce
import operator

# Single number — O(n) O(1)
reduce(operator.xor, nums)

# Missing number in [0..n] — O(n) O(1)
n=len(nums); n*(n+1)//2 - sum(nums)

# Count bits for every 0..n  O(n)
def count_bits(n):
    dp=[0]*(n+1)
    for i in range(1, n+1):
        dp[i]=dp[i>>1]+(i&1)
    return dp
```

</div>

---

## Sorting

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Python (Timsort) | O(n) | O(n log n) | O(n log n) | O(n) | Yes |
| Merge sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Counting sort | O(n+k) | O(n+k) | O(n+k) | O(k) | Yes |

<div class="note">Python's sort is Timsort — stable, adaptive, O(n) on nearly sorted data. Just use list.sort().</div>

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">custom sort keys</span></div>

```python
intervals.sort(key=lambda x: x[0])          # by start
words.sort(key=lambda x: (len(x), x))       # by len then alpha
nums.sort(key=lambda x: -x)                 # descending
sorted(items, key=lambda x: x[1], reverse=True)
```

</div>

---

## Pattern Guide

<div class="pt-row"><div class="pt-name">Two Pointers</div><div class="pt-when">Sorted array · find pair/triplet summing to target · remove duplicates · palindrome check</div></div>
<div class="pt-row"><div class="pt-name">Sliding Window</div><div class="pt-when">Contiguous subarray/substring · longest/shortest satisfying condition · max in window of size k</div></div>
<div class="pt-row"><div class="pt-name">Binary Search</div><div class="pt-when">Sorted or rotated array · "find min X satisfying condition" · answer-space search</div></div>
<div class="pt-row"><div class="pt-name">BFS</div><div class="pt-when">Shortest path (unweighted) · level-order tree · multi-source spread</div></div>
<div class="pt-row"><div class="pt-name">DFS / Backtracking</div><div class="pt-when">All paths/subsets/permutations · graph connectivity · maze · Sudoku · N-Queens</div></div>
<div class="pt-row"><div class="pt-name">Dynamic Programming</div><div class="pt-when">Optimal value (min/max/count) · overlapping subproblems · take or skip decision</div></div>
<div class="pt-row"><div class="pt-name">Heap</div><div class="pt-when">Kth largest/smallest · merge K sorted · scheduling · running median · Dijkstra</div></div>
<div class="pt-row"><div class="pt-name">Monotonic Stack</div><div class="pt-when">Next greater/smaller · largest rectangle in histogram · trapping rain water</div></div>
<div class="pt-row"><div class="pt-name">Monotonic Deque</div><div class="pt-when">Sliding window max/min in O(n)</div></div>
<div class="pt-row"><div class="pt-name">Prefix Sum</div><div class="pt-when">Subarray sum queries · range sum O(1) · count subarrays with sum k</div></div>
<div class="pt-row"><div class="pt-name">Fast & Slow Pointers</div><div class="pt-when">Cycle detection · find middle · palindrome linked list</div></div>
<div class="pt-row"><div class="pt-name">Trie</div><div class="pt-when">Autocomplete · Word Search II · longest common prefix</div></div>
<div class="pt-row"><div class="pt-name">Union-Find</div><div class="pt-when">Connected components · cycle detection · accounts merge</div></div>
<div class="pt-row"><div class="pt-name">Topological Sort</div><div class="pt-when">Dependency ordering · course schedule · detect cycle in directed graph</div></div>
<div class="pt-row"><div class="pt-name">Bit Manipulation</div><div class="pt-when">Find unique element · missing number · subset enumeration · XOR tricks</div></div>
<div class="pt-row"><div class="pt-name">Two Heaps</div><div class="pt-when">Running median · split data into smaller/larger halves simultaneously</div></div>

---

### Interview Checklist

**Before coding**
- Clarify constraints — size, sign, duplicates, sorted?
- Ask about edge cases — empty, single element, all same
- State brute-force first, then optimize
- Agree on expected complexity

**While coding**
- Name variables clearly
- Think out loud — explain what you're doing and why
- Handle edge cases explicitly

**After coding**
- Trace through an example by hand
- Test edge cases — empty, single, large
- State time & space complexity

---

### Python One-Liners

<div class="code-block">
<div class="code-label"><span class="lang">python</span><span class="desc">handy built-ins</span></div>

```python
float('inf'), float('-inf')      # infinity
7//2  # 3 (floor div)   7%2  # 1
a, b = b, a                      # swap
[[0]*cols for _ in range(rows)]  # 2D grid
[v for row in grid for v in row] # flatten

for i, v in enumerate(nums)      # (index, value)
for a, b in zip(l1, l2)

any(x>0 for x in nums)
all(x>0 for x in nums)
min(words, key=len)

from itertools import combinations, permutations
list(combinations([1,2,3], 2))   # [(1,2),(1,3),(2,3)]
```

</div>
