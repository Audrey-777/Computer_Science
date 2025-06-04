# 准备
## Pycharm调整：
1. 实现按住 ctrl +滑动鼠标滚轮实现代码窗口字体大小调整：
	**File** 一>**Settings** 一>**Editor**一>**General**里 的**Mouse Control**把**Change font size with Ctrl+Mouse Wheel**打上对勾，点击**OK**即可
2. 确保编译文件是current file
# 一些错误总结：
## Compile Error：
1. 这个多半是变量名字打错了，或者多打；：之类的，这个好查，一般本地都运行不了。
2. OJ的pylint是静态检查，有时候报的compile error不对。解决方法有两种，如下：
​		1）第一行加# pylint: skip-file
​		2）方法二：如果函数内使用全局变量（变量类型是immutable，如int），则需要在程序最开始声明一下。如果是全局变量是list类型，则不受影响。
## Runtime Error：
1. 指针越界，比如长度为5（index为0，1，2，3，4）的数组你去获取list[5]，但是注意list[-5]是合法的（index可以为-1，-2，-3，-4，-5）。
2. 数组开太大了，比如开到1000000000（9个0）就会
3. 递归爆栈：这个用以下代码解决
```python
from sys import setrecurisonlimit
setrecursionlimit(10000)#python 默认 200
```
4. 输入读取错误，一般是输入没读完就exit（）    （所以要谨慎使用这个函数）
5. 除以0，检查一下变量是不是可以是0
## Wrong Answer
​	这个需要好好审一遍代码
​	如果没有逻辑性的错误，就查一下边界值，比如0啊1啊什么的，很可能在这些地方出错
​	如果还不行，就仔细审题，看看哪个地方理解错了
## Presentation Error
​	只遇到过一次，就是该输出空行的时候没输出空行
## Time Limit Exceeded
1. 死循环（这种在BFS类里容易碰到，要注意visited的逻辑是否正确）
2. 算法问题（注意每次读题时注意数据量，以便快速确定算法）
3. 卡常数，这种情况用pypy3就可以了，或者再优化一下代码，去除一些鸡肋的O（n）操作
## Memory Limit Exceeded
​	数组开太大了，一般DP难题会遇到，这时候就要考虑压缩空间了。
​	还有就是BFS的时候queue可能会超，要考虑进堆的数据的优化
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

# 双指针
## Binary Search-二分查找
### 模板
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

## 数据流中的中位数
```python
class MedianFinder:  
    def __init__(self):  
        self.left = [] #左堆：用负值做最大堆  
        self.right = [] #右堆：最小堆  
  
    def addNum(self, num: int) -> None:  
        if len(self.left) == len(self.right): #左右等长，加入左堆  
            temp = -heapq.heappushpop(self.right, num) #取并且pop包括num在内的右侧所有元素最小值  
            heapq.heappush(self.left, temp) #加到左堆，更新完成。  
        else:  
            temp = -heapq.heappushpop(self.left, -num)  
            heapq.heappush(self.right, temp)  
  
    def findMedian(self) -> float:  
        if len(self.left) > len(self.right): #奇数个数  
            ans = -self.left[0]  
        else:  
            ans = (-self.left[0] + self.right[0]) / 2 #因为是数据流所以不可以pop否则影响后续。  
        return ans
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
## 最近公共祖先
```python
class Solution:  
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':  
        if not root or root == p or root == q:  
            return root  
        left = self.lowestCommonAncestor(root.left, p, q)  
        right = self.lowestCommonAncestor(root.right, p, q)  
        if not left:  
            return right  
        if not right:  
            return left  
        return root
```
## 扩展二叉树（建树读数据参考）
```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def build_tree(s, index):
    # 如果当前字符为'.'，表示空结点，返回None，并将索引后移一位
    if s[index] == '.':
        return None, index + 1
    # 否则创建一个结点
    node = Node(s[index])
    index += 1
    # 递归构造左子树
    node.left, index = build_tree(s, index)
    # 递归构造右子树
    node.right, index = build_tree(s, index)
    return node, index

def inorder(node, res):
    if node is None:
        return
    inorder(node.left, res)
    res.append(node.val)
    inorder(node.right, res)

def postorder(node, res):
    if node is None:
        return
    postorder(node.left, res)
    postorder(node.right, res)
    res.append(node.val)

def main():
    s = input().strip()
    root, _ = build_tree(s, 0)
    
    in_res = []
    inorder(root, in_res)
    
    post_res = []
    postorder(root, post_res)
    
    print("".join(in_res))
    print("".join(post_res))

if __name__ == '__main__':
    main()
```
# Heap-堆

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

# Summary
## 1. 二分查找

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

### Shunting-yard算法
```python
中序转后序Shunting Yard：

基本步骤：

1. 初始化运算符栈和输出栈为空。
2. 从左到右遍历中缀表达式的每个符号。
   - 如果是操作数（数字），则将其添加到输出栈。
   - 如果是左括号，则将其推入运算符栈。
   - 如果是运算符：
     - 如果运算符的优先级大于运算符栈顶的运算符，或者运算符栈顶是左括号，则将当前运算符推入运算符栈。
     - 否则，将运算符栈顶的运算符弹出并添加到输出栈中，直到满足上述条件（或者运算符栈为空）。
     - 将当前运算符推入运算符栈。
   - 如果是右括号，则将运算符栈顶的运算符弹出并添加到输出栈中，直到遇到左括号。将左括号弹出但不添加到输出栈中。
3. 如果还有剩余的运算符在运算符栈中，将它们依次弹出并添加到输出栈中。
4. 输出栈中的元素就是转换后的后缀表达式。

```python
def turn(s):
    fuhao={'+':1,'-':1,'*':2,'/':2}
    stack=[]
    ans=[]
    num=''
    for i in s:
        if i in '0123456789.':
            num+=i
        else:
            if num:
                ans.append(num)
                num=''
            if i in '+-*/':
                while stack and stack[-1] in '+-*/' and fuhao[i]<=fuhao[stack[-1]]:
                    ans.append(stack.pop())
                stack.append(i)
            elif i=='(':
                stack.append(i)
            elif i==')':
                while stack and stack[-1]!='(':
                    ans.append(stack.pop())
                stack.pop()
    if num:
        ans.append(num)
    while stack:
        ans.append(stack.pop())
    return ' '.join(str(i) for i in ans)
case=int(input())
for _ in range(case):
    s=input()
    print(turn(s))

```
### 单调栈例题：柱状图中的最大矩形
```python
class Solution:  
    def largestRectangleArea(self, heights: List[int]) -> int:  
        l = len(heights)  
  
        left = [-1] * l #找到i处左边最早高度比height[i]小的地方  
        stack = []  
        for i, h in enumerate(heights):  
            while stack and h <= heights[stack[-1]]: #访问栈顶。若没能比i处的高度小，就把它pop掉。  
                stack.pop()  
            if stack: #如果还有，就取到最后一个入栈的（最靠近i的），记录它的index  
                left[i] = stack[-1]  
            stack.append(i)  
  
        right = [l] * l #找到i处右边最早高度比height[i]小的地方  
        stack.clear()  
        for i in range(l - 1, -1, -1): #倒着遍历  
            h = heights[i]  
            while stack and h <= heights[stack[-1]]:  
                stack.pop()  
            if stack:  
                right[i] = stack[-1]  
            stack.append(i)  
  
        ans = 0  
        for h, l, r in zip(heights, left, right):  
            ans = max(ans, h * (r - l - 1))  
        return ans
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
        self.parent = {element: element for element in elements}  
        self.size = {element: 1 for element in elements}  
  
    def find_parent(self, i):  
        if self.parent[i] == i:  
            return i  
        self.parent[i] = self.find_parent(self.parent[i])  
        return self.parent[i]  
  
    def union(self, i, j):  
        root_i = self.find_parent(i)  
        root_j = self.find_parent(j)  
        if root_i != root_j:  
            if self.size[root_i] < self.size[root_j]:  
                root_i, root_j = root_j, root_i  
            self.parent[root_j] = root_i  
            self.size[root_i] += self.size[root_j]  
            return True  
        return False
```

- **路径压缩**（`find` 里递归压平） + **按秩合并** → 接近 O(α(n)) ≈ O(1)。
---

## 6. 前缀树（Trie）
```python
class Node:  
    __slots__ = 'son', 'end'  
    def __init__(self):  
        self.son = {} #self.son[root] = node  
        self.end = False  
  
class Trie:  
    def __init__(self):  
        self.root = Node() #初始化一个node  
  
    def insert(self, word: str) -> None:  
        curr = self.root  
        for c in word:  
            if c not in curr.son:  
                curr.son[c] = Node() #形成新的分叉  
            curr = curr.son[c]  
        curr.end = True  
  
    def find(self, word: str) -> int:  
        curr = self.root  
        for c in word:  
            if c not in curr.son:  
                return 0 #道不同不相为谋  
            curr = curr.son[c]  
        return 2 if curr.end else 1 # 2: perfectly matched; 1: prefix matched  
  
    def search(self, word: str) -> bool:  
        return self.find(word) == 2  
  
    def startsWith(self, prefix: str) -> bool:  
        return self.find(prefix) != 0
```
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
## 8. 前缀和
```python
class NumArray:  
    def __init__(self, nums: List[int]):  
        s = [0] * （len(nums) + 1） #s[0] = 0利于递推：s[i + 1] = s[i] + x  
        for i, x in enumerate(nums):  
            s[i + 1] = s[i] + x  
        self.sum = s  
  
    def sumRange(self, left: int, right: int) -> int:  
        return self.sum[right + 1] - self.sum[left]
```
二叉树版本
```python
class Solution:  
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:  
        cnt = defaultdict(int)  # 根节点到node处，前缀和为curr_sum的node个数  
        cnt[0] = 1  # 记得初始化：前缀和为0的有1个（包括进前缀和本身即为targetSum的情况）  
        self.ans = 0  
  
        def dfs(node, curr_sum) -> None:  
            if node is None:  
                return  
            curr_sum += node.val  
            self.ans += cnt[curr_sum - targetSum]  
            cnt[curr_sum] += 1  
            
            dfs(node.left, curr_sum)  
            dfs(node.right, curr_sum)  
            cnt[curr_sum] -= 1  # backtracking  
            
        dfs(root, 0)  
        return self.ans
```

## 9. 滑动窗口
最长不重复子串
```python
def sliding_window(s: str) -> int:
    seen = set()
    left = 0
    res = 0
    for right in range(len(s)):
        while s[right] in seen:
            seen.remove(s[left])
            left += 1
        seen.add(s[right])
        res = max(res, right - left + 1)
    return res
```

## 10. Backtracking
path为列表
```python
def backtrack(path, 选择列表):
    if 满足结束条件:
        结果.append(path[:])
        return
    for 选择 in 选择列表:
        # 做选择
        path.append(选择)
        # 递归
        backtrack(path, 新的选择列表)
        # 撤销选择
        path.pop()
```
path为字符串
```python
def backtracking(k: int, path: str) -> None:  
    if k == l:  
        ans.append(path)  
        return  
    for ch in dic[int(digits[k])]:  
        path = path + ch  
        backtracking(k + 1, path)  
        path = path[:-1:]  
  
  
path = ''  
backtracking(0, path)
```
# 经典图算法
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
记录路径：
```python
while priority_queue:  
        curr_dist, curr_pos = heapq.heappop(priority_queue)  
        if curr_dist > distance[curr_pos]:  
            continue  
  
        if curr_pos == end:  
            break  
  
        for neighbor, weight in maps[curr_pos]:  
            nd = curr_dist + weight  
            if nd < distance[neighbor]:  
                distance[neighbor] = nd  
                path[neighbor] = path[curr_pos] + f'->({weight})->' + neighbor  
                heapq.heappush(priority_queue, (nd, neighbor))  
  
    return path[end]  
```
## 无向图判环
```python
def dfs(u, parent):
    visited[u] = True # 1. 标记当前节点 u 为已访问
    for v in graph[u]: # 2. 遍历节点 u 的所有邻居 v
        if not visited[v]: # 3. 如果邻居 v 没有被访问过
            if dfs(v, u): # 4. 递归地对 v 进行 DFS 搜索，并传入 u 作为 v 的父节点
                return True  # 如果从 v 开始的子树中发现了环，就直接返回 True
        elif v != parent: # 5. 如果邻居 v 已经被访问过，并且 v 不是 u 的父节点
            return True  # 6. 说明找到了一个环
    return False # 7. 如果遍历完所有邻居都没有发现环，则返回 False
```

## 有向图判环&Topological Sorting
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

## 最小生成树（MST）
这样定义<mark>安全的边</mark>：它的一端是生成树中的顶点，另一端是还不在生成树中的顶点。这保证了构建的树不会出现循环。
### Kruskal
```python
class DisjointSet:
    def __init__(self, num_vertices):
        self.parent = list(range(num_vertices))
        self.rank = [0] * num_vertices

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x != root_y:
            if self.rank[root_x] < self.rank[root_y]:
                self.parent[root_x] = root_y
            elif self.rank[root_x] > self.rank[root_y]:
                self.parent[root_y] = root_x
            else:
                self.parent[root_x] = root_y
                self.rank[root_y] += 1


def kruskal(graph):
    num_vertices = len(graph)
    edges = []

    # 构建边集
    for i in range(num_vertices):
        for j in range(i + 1, num_vertices):
            if graph[i][j] != 0:
                edges.append((i, j, graph[i][j]))

    # 按照权重排序
    edges.sort(key=lambda x: x[2])

    # 初始化并查集
    disjoint_set = DisjointSet(num_vertices)

    # 构建最小生成树的边集
    minimum_spanning_tree = []

    for edge in edges:
        u, v, weight = edge
        if disjoint_set.find(u) != disjoint_set.find(v):
            disjoint_set.union(u, v)
            minimum_spanning_tree.append((u, v, weight))

    return minimum_spanning_tree
```
### Prim
```python
import heapq

visited = {0}  
heap = graph[0]  
heapq.heapify(heap)  
total_cost = 0  
edges_used = 0  
  
while heap and edges_used < n - 1:  
    distance, node = heapq.heappop(heap)  # 根据distance排序的最小堆  
    if node not in visited:  
        visited.add(node)  
        total_cost += distance  
        edges_used += 1  
        for neighbor_distance, neighbor in graph[node]:  
            if neighbor not in visited:  
                heapq.heappush(heap, (neighbor_distance, neighbor))
return total_cost
```

### Prim(MST, OOP)
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

## Floyd-Warshall
```python
def floyd_warshall(graph):
    n = len(graph)
    dist = [[float('inf')] * n for _ in range(n)]

    for i in range(n):
        for j in range(n):
            if i == j:
                dist[i][j] = 0
            elif j in graph[i]:
                dist[i][j] = graph[i][j]

    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])

    return dist
```
# 小function与模板们
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

## try...except...
```python
while True:  
    try:  
        n = int(input())  
        graph = Graph(n) #初始化一个Graph实例  
        for i in range(n):  
            row = list(map(int, input().split()))  
            for j in range(i + 1, n):  
                graph.add_edge(i, j, row[j])  
        print(graph.prim())  
  
    except EOFError:  
        break
```