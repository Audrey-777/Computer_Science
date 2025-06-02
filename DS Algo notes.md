## Lecture 4 提示词工程&链表
### Prompt Engineering
1. generate prompts with LLMs
	以markdown格式输出、符合用户需求和知识储备、清晰准确保持高质量的同时尽可能简洁、只输出提示词不做多余的解释
2. LLM - expertise in certain field
3. existing problems:
	1. citing resources
	2. bias
	3. hallucinations
	4. math&commonsense
	5. prompt hacking
4. prompt engineering techniques
	1. single prompt techniques
		1. chain of thought/CoT(google)-'please reason step by step... '
		2. program-aided language models(PAL) - 将思考过程呈现为有代码的自然语言
	2. multiple prompt techniques
		1. Self Consistency - different reasoning chain, vote for the most possible answer
		2. Prompt Chaining - Question->Q1, Q2, ...
		3. Least to Most Prompting - use LLM to break down the questions
		4. Tree of Thoughts/ToT - better for traceback
			1. break down to smaller steps
			2. potential thoughts
			3. enlightening evaluation
			4. search algorithm
	3. LLM with external tools
		1. Retrieval-Augmented Generation/RAG
			1. Retrieval: search for probable documents that may solve the problem
			2. Generation: pass down the documents+the original problems as context to LLM
		2. ReAct framework(reason + act)
5. Prompt hacking
	1. why reasoning models conduct repeated reasoning? - only the correct answers will get the reward, no matter how long it thinks.
### 链表

## Lecture 5 经典排序算法

#### 1. 冒泡排序（Bubble Sort）
**思想**：重复遍历待排序数组，每次比较相邻元素并交换，使较大（或较小）元素逐步移动到数组一端。  
**时间复杂度**：
- 最优：O(n)（数组已排序）
- 平均：O(n²)
- 最差：O(n²)
**空间复杂度**：O(1)（原地排序）  
**是否稳定**：✅（相同元素相对顺序不变）
```python
def bubble_sort(arr):
    for i in range(len(arr) - 1):
        for j in range(len(arr) - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

```

---

#### 2. 选择排序（Selection Sort）
**思想**：每次遍历找到最小（或最大）元素，并将其放到数组的起始位置。  
**时间复杂度**：O(n²)（无论初始状态如何）  
**空间复杂度**：O(1)（原地排序）  
**是否稳定**：❌（相同元素可能被交换导致顺序改变）
```Python
def selection_sort(arr):
    for i in range(len(arr)):
        min_idx = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
```
---
#### 3. 插入排序（Insertion Sort）
**思想**：从第二个元素开始，每次将当前元素插入到前面已排序的部分，使之仍然有序。  
**时间复杂度**：
- 最优：O(n)（数组已排序）
- 平均：O(n²)
- 最差：O(n²)
**空间复杂度**：O(1)（原地排序）  
**是否稳定**：✅
```Python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key, j = arr[i], i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```

---
#### 4. 希尔排序（Shell Sort）
**思想**：对插入排序的改进，通过**分组**进行插入排序，逐步减少步长，最终进行一次标准插入排序。  
**时间复杂度**：
- 最优：O(n)（依赖步长序列）
- 平均：O(nlogn) ~ O(n²)
- 最差：O(n²)
**空间复杂度**：O(1)  
**是否稳定**：❌（分组导致相对顺序变化）
```Python
def shell_sort(arr):
    gap = len(arr) // 2
    while gap > 0:
        for i in range(gap, len(arr)):
            temp, j = arr[i], i
            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]
                j -= gap
            arr[j] = temp
        gap //= 2
```
---
#### 5. 归并排序（Merge Sort）
**思想**：递归地将数组分成两半，分别排序后合并（分治思想）。  
**时间复杂度**：O(nlogn)（每次划分 O(logn)，合并 O(n)）  
**空间复杂度**：O(n)（需要额外数组存放合并结果）  
**是否稳定**：✅
```Python
def merge_sort(arr):
    if len(arr) < 2:
        return arr
    mid = len(arr) // 2
    left, right = merge_sort(arr[:mid]), merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    while left and right:
        result.append(left.pop(0) if left[0] < right[0] else right.pop(0))
    return result + left + right
```
---
#### 6. 快速排序（Quick Sort）
**思想**：选择**基准值**，将小于基准的放左边，大于基准的放右边，然后递归排序左右两部分。  
**时间复杂度**：
- 最优：O(nlogn)（均匀分割）
- 平均：O(nlogn)
- 最差：O(n²)（极端情况，如已排序数组）
**空间复杂度**：O(logn)（递归调用栈）  
**是否稳定**：❌（交换元素可能破坏相对顺序）
```Python
def quick_sort(arr):
    if len(arr) < 2:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    mid = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + mid + quick_sort(right)
```
---
#### 7. 堆排序（Heap Sort）
**思想**：构建**大根堆**（升序）或**小根堆**（降序），每次取出堆顶元素，并调整堆。  
**时间复杂度**：O(nlogn)（建堆 O(n)，调整 O(logn)）  
**空间复杂度**：O(1)（原地排序）  
**是否稳定**：❌（堆调整时可能改变相同元素顺序）
```Python
import heapq
def heap_sort(arr):
    heapq.heapify(arr)
    return [heapq.heappop(arr) for _ in range(len(arr))]
```
---
#### 8. 计数排序（Counting Sort）
**思想**：适用于**整数**排序，统计每个元素出现的次数，根据统计信息直接填充排序结果。  
**时间复杂度**：O(n + k)（k 为值域范围）  
**空间复杂度**：O(k)（需要额外数组存储计数信息）  
**是否稳定**：✅  
**适用场景**：元素范围小且非负整数，如考试成绩排序。
```Python
def counting_sort(arr):
    if not arr: return []
    count = [0] * (max(arr) + 1)
    for num in arr:
        count[num] += 1
    return [i for i in range(len(count)) for _ in range(count[i])]
```
---
#### 9. 基数排序（Radix Sort）
**思想**：对**整数**按照位（个位、十位、百位...）进行多轮**稳定排序**（通常用**计数排序**）。  
**时间复杂度**：O(nk)（k 为位数）  
**空间复杂度**：O(n + k)（需要额外空间存储桶）  
**是否稳定**：✅  
**适用场景**：适合处理**位数不多**的整数排序。
```Python
def radix_sort(arr):
    max_digit = len(str(max(arr)))
    for d in range(max_digit):
        buckets = [[] for _ in range(10)]
        for num in arr:
            buckets[(num // (10 ** d)) % 10].append(num)
        arr = [num for bucket in buckets for num in bucket]
    return arr
```
---
#### 10. 桶排序（Bucket Sort）
**思想**：将数据分配到多个**桶**中，每个桶分别排序（可用插入排序或快排），最后合并桶。  
**时间复杂度**：O(n + k)（取决于桶的数量 k）  
**空间复杂度**：O(n + k)（需要额外桶空间）  
**是否稳定**：✅（取决于桶内排序算法）  
**适用场景**：数据分布较均匀，如[0, 1)之间的小数。
```Python
def bucket_sort(arr, bucket_size=5):
    if not arr: return []
    min_val, max_val = min(arr), max(arr)
    buckets = [[] for _ in range((max_val - min_val) // bucket_size + 1)]
    for num in arr:
        buckets[(num - min_val) // bucket_size].append(num)
    return [num for bucket in buckets for num in sorted(bucket)]
```
---

#### 总结对比

| 排序算法   | 时间复杂度（最优/平均/最差） | 空间复杂度 | 稳定性 | 适用场景 |
|-----------|--------------------|---------|------|--------|
| 冒泡排序   | O(n) / O(n²) / O(n²) | O(1)    | ✅  | 适用于小规模数据 |
| 选择排序   | O(n²) / O(n²) / O(n²) | O(1)    | ❌  | 适用于小规模数据 |
| 插入排序   | O(n) / O(n²) / O(n²) | O(1)    | ✅  | 适用于基本有序数据 |
| 希尔排序   | O(n) / O(nlogn) / O(n²) | O(1)    | ❌  | 适用于中等规模数据 |
| 归并排序   | O(nlogn) / O(nlogn) / O(nlogn) | O(n)    | ✅  | 适用于大规模数据，稳定性要求高 |
| 快速排序   | O(nlogn) / O(nlogn) / O(n²) | O(logn) | ❌  | 适用于一般数据，平均表现优秀 |
| 堆排序    | O(nlogn) / O(nlogn) / O(nlogn) | O(1)    | ❌  | 适用于优先队列、TOP-K 问题 |
| 计数排序   | O(n+k) / O(n+k) / O(n+k) | O(k)    | ✅  | 适用于小范围整数排序 |
| 基数排序   | O(nk) / O(nk) / O(nk) | O(n+k)  | ✅  | 适用于整数或定长字符串排序 |
| 桶排序    | O(n+k) / O(n+k) / O(n²) | O(n+k)  | ✅  | 适用于数据均匀分布场景 |

