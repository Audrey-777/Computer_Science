# Stack
## 字符串解码
**输入:** `s = "3[a]2[bc]"`
**输出：**`"aaabcbc"`
```python
def decodeString(self, s: str) -> str:  
    stack, res, multi = [], '', 0  
    for c in s:  
        if c == '[':  
            stack.append([multi, res])  
            res, multi = '', 0  # 碰到'['就将res和multi入栈并置零  
        elif c == ']':  
            cur_multi, last_res = stack.pop()  
            res = last_res + cur_multi * res  
        elif '0' <= c <= '9':  
            multi = multi * 10 + int(c)  
        else:  
            res += c  
  
    return res
```

样例输入
`[2b[3a]c]`
样例输出
`baaacbaaac`
```python
def decode_string(s: str) -> str:  
    i = 0  
    stack = []  
    curr_string = ''  
    times = 0  
  
    while i < len(s):  
        if s[i] == '[':  
            i += 1  
            while s[i].isdigit():  
                times = times * 10 + int(s[i])  
                i += 1  
            stack.append((curr_string, times))  
            curr_string = ''  
            times = 0  
  
        elif s[i] == ']':  
            prev_string, time = stack.pop()  
            curr_string = prev_string + curr_string * time  
            i += 1  
  
        else:  
            curr_string += s[i]  
            i += 1  
  
    return curr_string  
  
s = str(input())  
print(decode_string(s))
```
# OOP-面向对象编程

## 3. 创建实例（对象）
通过类创建对象的过程称为**实例化**。
### 示例：实例化并使用
```python
# 创建 Dog 类的实例
my_dog = Dog("Buddy", "Labrador")

# 调用方法
my_dog.bark()  # 输出: Buddy the Labrador says Woof!

# 访问属性
print(my_dog.name)   # 输出: Buddy
print(my_dog.breed)  # 输出: Labrador
```

- **`my_dog = Dog("Buddy", "Labrador")`**：调用类的构造方法，创建对象。
- **`my_dog.bark()`**：调用实例的方法，`self` 自动绑定到 `my_dog`。
- **`my_dog.name`**：直接访问实例的属性。

---

## 4. 属性（Attributes）
属性分为：
- **实例属性**：每个实例独有的数据。
- **类属性**：类本身拥有的属性，所有实例共享。

### 示例：实例属性和类属性
```python
class Dog:
    species = "Canis familiaris"  # 类属性，所有 Dog 实例共享

    def __init__(self, name):
        self.name = name          # 实例属性，每个实例独立

dog1 = Dog("Buddy")
dog2 = Dog("Max")

print(dog1.name)      # 输出: Buddy
print(dog2.name)      # 输出: Max
print(dog1.species)   # 输出: Canis familiaris
print(dog2.species)   # 输出: Canis familiaris
```

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):                # 实例方法
        print(f"{self.name} barks!")

    @classmethod
    def get_species(cls):          # 类方法
        return "Canis familiaris"

    @staticmethod
    def info():                    # 静态方法
        return "Dogs are loyal animals."

# 使用
dog = Dog("Buddy")
dog.bark()            # 输出: Buddy barks!
print(Dog.get_species())  # 输出: Canis familiaris
print(Dog.info())     # 输出: Dogs are loyal animals.
```

#### 方法类型：
1. **实例方法**：
   - 需要 `self` 参数，操作实例属性。
   - 如 `bark(self)`。

2. **类方法**：
   - 用 `@classmethod` 装饰器，接收 `cls`（类本身）作为参数。
   - 如 `get_species(cls)`。

3. **静态方法**：
   - 用 `@staticmethod` 装饰器，不需要 `self` 或 `cls`，与类逻辑相关但不依赖实例或类状态。
   - 如 `info()`。

---

## 6. 封装（Encapsulation）
封装是将数据和操作数据的方法绑定在一起，并控制外部访问的权限。

### 示例：私有属性
```python
class Dog:
    def __init__(self, name, age):
        self.name = name        # 公开属性
        self.__age = age        # 私有属性（双下划线）

    def get_age(self):          # 获取私有属性的方法
        return self.__age

    def set_age(self, age):     # 修改私有属性的方法
        if age > 0:
            self.__age = age

dog = Dog("Buddy", 3)
print(dog.name)      # 输出: Buddy
print(dog.get_age()) # 输出: 3
# print(dog.__age)   # 错误！无法直接访问私有属性
dog.set_age(4)
print(dog.get_age()) # 输出: 4
```

- **`__age`**：双下划线表示**私有属性**，外部无法直接访问。
- **getter/setter 方法**：通过方法控制对私有属性的访问和修改。

---

## 7. 继承（Inheritance）
继承允许一个类（子类）继承另一个类（父类）的属性和方法。
### 示例：继承
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print("I can speak!")

class Dog(Animal):  # Dog 继承 Animal
    def bark(self):
        print(f"{self.name} barks!")

dog = Dog("Buddy")
dog.speak()  # 输出: I can speak!（继承自 Animal）
dog.bark()   # 输出: Buddy barks!（Dog 自己的方法）
```

- **`Dog(Animal)`**：`Dog` 类继承了 `Animal` 类。
- 子类可以直接使用父类的属性和方法，也可以添加自己的方法。

#### 方法重写（Override）
```python
class Dog(Animal):
    def speak(self):  # 重写父类方法
        print(f"{self.name} says Woof!")

dog = Dog("Buddy")
dog.speak()  # 输出: Buddy says Woof!（覆盖了父类的 speak）
```

---

## 8. 多态（Polymorphism）
多态是指不同类的对象可以用相同的方式调用方法，但表现出不同的行为。

### 示例：多态
```python
class Cat(Animal):
    def speak(self):
        print(f"{self.name} says Meow!")

animals = [Dog("Buddy"), Cat("Whiskers")]
for animal in animals:
    animal.speak()  # 输出: Buddy says Woof! 和 Whiskers says Meow!
```

- **`speak()`**：不同的类**实现了同一个方法名，但行为不同。**
- 多态允许用统一的接口处理不同类型的对象。

# Binary Search-二分查找
```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:               # 注意 <= 保证最后一个元素也会被查到
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid            # 找到，返回索引
        elif arr[mid] < target:
            lo = mid + 1         # 中点偏小，舍弃左半区
        else:
            hi = mid - 1         # 中点偏大，舍弃右半区
    return -1                     # 未找到
```
# 链表
小工具：
## 反转链表
```python
class Solution:  
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:  
        prev = None  
        curr = head  
        while curr:  
            nxt = curr.next #为了保存curr下一个的node  
            curr.next = prev  
            prev = curr  
            curr = nxt  
        return prev
```
## 找到中点
```python
def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]: #找到中间节点（偶数2k个时，找到k+1）  
	        slow = fast = head  
	        while fast and fast.next: #边界条件：快指针到最后一个node  
	            slow = slow.next #慢针走1步  
	            fast = fast.next.next #快针走2步  
	        return slow
```
# Tree-树🌲
## 定义🌲的类
1. 二叉树
	```Python
	class TreeNode:
		     def __init__(self, val=0, left=None, right=None):
		         self.val = val
		         self.left = left
		         self.right = right
	```
2. 多叉树
	```Python
	class Node: #建立树的节点类，包含：值&子节点  
	    def __init__(self, val):  
	        self.val = val  
	        self.children = []  
	```
## 递归遍历
1. 前序遍历
	```python
	def preorder_traversal(root):
	    if root:
	        print(root.val)  # 访问根节点
	        preorder_traversal(root.left)  # 递归遍历左子树
	        preorder_traversal(root.right)  # 递归遍历右子树
	```
2. 层序遍历
	```python
	# Definition for a binary tree node.
	# class TreeNode:
	#     def __init__(self, val=0, left=None, right=None):
	#         self.val = val
	#         self.left = left
	#         self.right = right
	class Solution:
	    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
	        if not root:
	            return []
	
	        result = []
	        queue = deque([root])
	
	        while queue:
	            level_size = len(queue)
	            level = []
	
	            for _ in range(level_size):
	                node = queue.popleft()
	                level.append(node.val)
	                if node.left:
	                    queue.append(node.left)
	                if node.right:
	                    queue.append(node.right)
	
	            result.append(level)
	
	        return result
	        
	```
## 深度
```Python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def tree_depth(node):
            if node is None:
                return 0
            left_depth = tree_depth(node.left)
            right_depth = tree_depth(node.right)
            return max(left_depth, right_depth) + 1

        return tree_depth(root)
```
## 直径
```python
class Solution:  
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:  
        self.ans = 0  # 初始化最大直径  
  
        def cal_depth(node) -> int:  # 一个计算某点处最大深度的函数，在递归过程中一边计算每个node的左右最大深度（从而得到node的最大直径）一边更新答案值  
            if node is None:  
                return -1  
            left = cal_depth(node.left)  
            right = cal_depth(node.right)  
            self.ans = max(self.ans, left + right + 2)  
            return max(left, right) + 1  
  
        cal_depth(root)  
        return self.ans
```
## 建立平衡树
```python
class Solution:  
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:  
        if not nums:  
            return None  
        m = len(nums) // 2  
        left = self.sortedArrayToBST(nums[:m]) #分治！左半边和右半边分别继续建树  
        right = self.sortedArrayToBST(nums[m + 1:])  
        return TreeNode(nums[m], left, right)
```

## Heap-堆

#### 堆的数组表示
- 对于索引 `i` 的节点：
  - 父节点：`(i-1) // 2`。
  - 左子节点：`2*i + 1`。
  - 右子节点：`2*i + 2`。
- 示例：最小堆 `[1, 2, 3, 4, 5]`：
  ```
      1
     / \
    2   3
   / \
  4   5
  ```
### 3. Python 中的堆用法
Python 提供内置模块 `heapq`，默认实现**最小堆**。它基于列表操作，非常简单易用。
#### 基本操作
| 操作          | 方法                              | 功能            | 时间复杂度    |
| ----------- | ------------------------------- | ------------- | -------- |
| 创建空堆        | `heap = []`                     | 初始化一个空列表作为堆   | O(1)     |
| 插入元素        | `heapq.heappush(heap, item)`    | 向堆中插入元素并调整堆   | O(log n) |
| 弹出最小元素      | `heapq.heappop(heap)`           | 移除并返回堆中最小元素   | O(log n) |
| 堆化列表        | `heapq.heapify(list)`           | 将列表原地转为最小堆    | O(n)     |
| 查看最小元素（不弹出） | `heap[0]`                       | 返回堆顶（最小值），不移除 | O(1)     |
| 替换堆顶        | `heapq.heapreplace(heap, item)` | 替换堆顶并返回原堆顶    | O(log n) |

### 8. 注意事项
- **最小堆默认**：`heapq` 只能直接实现最小堆，**最大堆需要取负数**。
- **无环检查**：堆操作不会引入环（不像链表）。
- **动态调整**：插入和删除后，堆会自动调整以保持堆性质。
- **索引从 0 开始**：`heap[0]` 是堆顶。
# 小function的模板们
## 判断质数
```Python
def is_prime_number(num):  
    if num < 2:  
        return False  
    for i in range(2,int(num ** 0.5) + 1):  
        if num % i == 0:  
            return False  
    return True
```
## defaultdict
```Python
from collections import defaultdict

# 创建一个默认值为 int 的 defaultdict
# 默认值为 0
default_dict = defaultdict(int)

# 创建一个默认值为 list 的 defaultdict
# 默认值为 []
default_dict_list = defaultdict(list)

# 创建一个默认值为 str 的 defaultdict
# 默认值为 ''
default_dict_str = defaultdict(str)
```

# Summary
## 0. 通用复杂度与常见术语

- **时间复杂度 (Time Complexity)**：操作次数随输入规模增长的上界，常见有 O(1)、O(log n)、O(n)、O(n log n)、O(n²)。
    
- **空间复杂度 (Space Complexity)**：额外使用内存随输入规模增长的上界。
    
- **均摊分析**：对某些摊销操作（如动态数组扩容、摊还插入堆），平均到每次操作的成本。
    

---

## 1. 二分查找（修正模板）

```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:               # 注意 <= 保证最后一个元素也会被查到
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid            # 找到，返回索引
        elif arr[mid] < target:
            lo = mid + 1         # 中点偏小，舍弃左半区
        else:
            hi = mid - 1         # 中点偏大，舍弃右半区
    return -1                     # 未找到
```

- **坑点**：闭区间 `[lo, hi]` vs. 开区间 `[lo, hi)`。
    
- **变体**：找第一个 ≥ target、最后一个 ≤ target 等，可在判断分支里微调边界。
    

---

## 2. 栈（Stack）、队列（Queue）、双端队列（Deque）

```python
# 栈：后进先出
stack = []
stack.append(x)    # push
top = stack.pop()  # pop

# 队列：先进先出
from collections import deque
queue = deque()
queue.append(x)        # enqueue
front = queue.popleft()# dequeue

# 双端队列：两端都可操作
dq = deque()
dq.appendleft(x); dq.append(x)
dq.popleft(); dq.pop()
```

---

## 3. 哈希表（Hash Table）

- Python 中即 `dict` 和 `set`，平均 O(1) 查、增、删。
    

```python
d = {}         
d[key] = val   # 插入/更新
val = d.get(key, default)  # 安全地取值
if key in d:   # 判断存在性
    del d[key] # 删除

s = set()      
s.add(x); s.remove(x)
```

---

## 4. 排序算法

|算法|平均时间|最坏时间|空间|稳定性|
|---|---|---|---|---|
|快速排序|O(n log n)|O(n²)|O(log n)|不稳定|
|归并排序|O(n log n)|O(n log n)|O(n)|稳定|
|堆排序|O(n log n)|O(n log n)|O(1)|不稳定|
|插入排序|O(n²)|O(n²)|O(1)|稳定|

简单示例：归并排序

```python
def merge_sort(arr):
    if len(arr) <= 1: return arr
    mid = len(arr)//2
    L = merge_sort(arr[:mid])
    R = merge_sort(arr[mid:])
    res = []
    i = j = 0
    while i < len(L) and j < len(R):
        if L[i] <= R[j]:
            res.append(L[i]); i += 1
        else:
            res.append(R[j]); j += 1
    res.extend(L[i:]); res.extend(R[j:])
    return res
```

---

## 5. 并查集（Union-Find）

用于处理连通性／集合合并问题（如岛屿计数、网络连通）

```python
class UnionFind:
    def __init__(self, elements):
        # parent 字典：存储每个元素的父节点
        # 初始时，每个元素的父节点就是它自己，表示各自是一个独立的集合
        self.parent = {element: element for element in elements}
        # size 字典：存储每个根节点所代表的集合的大小
        # 初始时，每个集合的大小都是 1
        self.size = {element: 1 for element in elements}
        # 记录当前最大的集合大小，方便最后直接获取结果
        self.max_size = 1

    def find(self, i):
        # `find` 操作的目的是找到元素 i 所在集合的代表元素（根节点）
        # 路径压缩优化：在查找过程中，将路径上的所有节点的父节点直接指向根节点
        if self.parent[i] == i:
            # 如果当前节点的父节点就是它自己，说明它就是根节点
            return i
        # 递归地查找父节点，并进行路径压缩
        self.parent[i] = self.find(self.parent[i])
        return self.parent[i]

    def union(self, i, j):
        # `union` 操作的目的是合并元素 i 和元素 j 所在的两个集合
        # 1. 找到 i 和 j 各自的根节点
        root_i = self.find(i)
        root_j = self.find(j)

        # 2. 如果根节点相同，说明它们已经在同一个集合中，无需合并
        if root_i != root_j:
            # 3. 按大小合并优化：将小集合的根节点连接到大集合的根节点上
            # 这样可以保持树的深度较小，提高后续 find 操作的效率
            if self.size[root_i] < self.size[root_j]:
                root_i, root_j = root_j, root_i  # 交换，确保 root_i 总是较大集合的根

            self.parent[root_j] = root_i  # 将小集合的根连接到大集合的根
            self.size[root_i] += self.size[root_j]  # 更新大集合的大小

            # 更新当前找到的最大集合大小
            self.max_size = max(self.max_size, self.size[root_i])
            return True # 表示成功合并
        return False # 表示没有合并（已经在同一个集合中）
```

- **路径压缩**（`find` 里递归压平） + **按秩合并** → 接近 O(α(n)) ≈ O(1)。
---

## 6. 图（Graph）算法基础

---

## 7. 动态规划（DP）

- **五部曲**：
    
    1. 明确状态（state）
        
    2. 写出状态转移方程
        
    3. 确定初始状态和边界条件
        
    4. 选择「自顶向下」或「自底向上」
        
    5. 滚动数组 / 状态压缩（可选）
        
- **经典例子：爬楼梯**
    
    ```python
    def climb_stairs(n):
        if n <= 2: return n
        dp = [0]*(n+1)
        dp[1], dp[2] = 1, 2
        for i in range(3, n+1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
    ```

# 经典图算法
## Prim(MST, OOP)
```python
import heapq

class Graph:
    def __init__(self, vertices):
        self.vertices = vertices  # 顶点数量
        self.graph = {i: [] for i in range(vertices)}  # 存储图的邻接表
    
    def add_edge(self, u, v, weight):
        self.graph[u].append((v, weight))
        self.graph[v].append((u, weight))  # 因为是无向图，双向添加边

    def prim(self):
        # 使用一个最小堆来存储边 (权重, u, v)
        min_heap = []
        in_mst = [False] * self.vertices  # 记录每个顶点是否已经在生成树中
        mst_edges = []  # 存储生成树的边

        # 选择从第一个顶点开始
        def add_edges(vertex):
            in_mst[vertex] = True
            for neighbor, weight in self.graph[vertex]:
                if not in_mst[neighbor]:
                    heapq.heappush(min_heap, (weight, vertex, neighbor))
                    #把vertex没遍历过的neighbors全部入堆（注意按weight排序）

        # 初始选择顶点0
        add_edges(0)

        while min_heap:
            weight, u, v = heapq.heappop(min_heap)  # 取出最小边
            if in_mst[v]:
                continue  # 如果v已经在生成树中，跳过

            mst_edges.append((u, v, weight))  # 将边加入生成树
            add_edges(v)  # 将v的邻接边加入堆中

        return mst_edges

```

## Dijkstra
```python
import heapq
def dijkstra(start):
    dist = {u: float('inf') for u in graph}
    dist[start] = 0
    hq = [(0, start)]
    while hq:
        d, u = heapq.heappop(hq)
        if d > dist[u]:
	        continue
        for v, w in graph[u]:   # graph[u] 存 (邻点, 权重)
            if dist[v] > d + w:
                dist[v] = d + w
                heapq.heappush(hq, (dist[v], v))
    return dist[]
```

## Topological Sorting
### Kahn's
适用：【有向无环图DAG】
```python
from collections import defaultdict, deque

def topological_sort(num_vertices, edges):
    adj_list = defaultdict(list)
    in_degree = [0] * num_vertices

    # 构建邻接列表并计算入度
    for u, v in edges:
        adj_list[u].append(v)
        in_degree[v] += 1

    queue = deque()
    # 将所有入度为 0 的顶点加入队列
    for i in range(num_vertices):
        if in_degree[i] == 0:
            queue.append(i)

    topological_order = []
    
    # 循环处理队列
    while queue:
        u = queue.popleft()
        topological_order.append(u)

        # 遍历 u 的邻接顶点
        for v in adj_list[u]:
            in_degree[v] -= 1
            if in_degree[v] == 0:
                queue.append(v)

    # 判断是否有环
    if len(topological_order) == num_vertices:
        return topological_order
    else:
        return [] # 图中存在环
```
