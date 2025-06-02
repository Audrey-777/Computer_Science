# Assignment #1: 虚拟机，Shell & 大模型

Updated 1317 GMT+8 Feb 20, 2025

2025 spring, Complied by 陈怀瑜 工学院



> 作业的各项评分细则及对应的得分情况
>
> | 标准                                 | 等级                                                         | 得分 |
> | ------------------------------------ | ------------------------------------------------------------ | ---- |
> | 按时提交                             | 完全按时提交：1分<br/>提交有请假说明：0.5分<br/>未提交：0分  | 1 分 |
> | 源码、耗时（可选）、解题思路（可选） | 提交了4个或更多题目且包含所有必要信息：1分<br/>提交了2个或以上题目但不足4个：0.5分<br/>没有提供源码：0分 | 1 分 |
> | AC代码截图                           | 包含清晰的Canvas头像、PDF文件以及MD或DOC格式的附件：1分<br/>缺少上述三项中的任意一项：0.5分<br/>缺失两项或以上：0分 | 1 分 |
> | 清晰头像、PDF文件、MD/DOC附件        | 包含清晰的Canvas头像、PDF文件以及MD或DOC格式的附件：1分<br/>缺少上述三项中的任意一项：0.5分<br/>缺失两项或以上：0分 | 1 分 |
> | 学习总结和个人收获                   | 提交了学习总结和个人收获：1分<br/>未提交学习总结或内容不详：0分 | 1 分 |
> | 总得分： 5                           | 总分满分：5分                                                |      |
>
> 
>
> **说明：**
>
> 1. **解题与记录：**
>    
>    - 对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 2. **课程平台与提交安排：**
>
>    - 我们的课程网站位于Canvas平台（https://pku.instructure.com ）。该平台将在第2周选课结束后正式启用。在平台启用前，请先完成作业并将作业妥善保存。待Canvas平台激活后，再上传你的作业。
>    
>    - 提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 3. **延迟提交：**
>
>    - 如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/

思路：
无论输入分数和相加之后的分数是否达到最简，先用基本的通分法则将其相加，得到数值上正确的答案。然后用math.gcd求结果分子分母的最大公因数，同时除以它，就是最简的分子和分母。
后来才知道此题旨在学会类的定义。学习了。

代码：

```python
import math  
n1, d1, n2, d2 = [int(x) for x in input().split()]  
  
d = d1 * d2  
n = d1 * n2 + d2 * n1  
gcd = math.gcd(d, n)  
if gcd > 1:  
    d //= gcd  
    n //= gcd  
print(str(n)+'/'+str(d))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![[Pasted image 20250222220603.png]]




### 1760.袋子里最少数目的球

 https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/

思路：【经ChatGPT提示 （没有让它直接提供代码实现）】
目标量（最小化的最大开销）x为【1，max（所有袋子中最大球数）】中的值。每个x，唯一对应其需要的最少操作数min_operations。x作为index，min_operations作为值（自然形成有序序列），对于每个袋子进行检验，可用binary search解决。


代码：

```python
class Solution:  
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:  
        def can_div(m: int):  
            import math  
            counter = 0  
            for num in nums:  
                counter += math.ceil(num / m) - 1#求出所有袋子对于m的最小操作次数之和  
            if counter <= maxOperations:  
                return True  
            else:  
                return False  
  
        left = 1  
        right = max(nums)  
        while left < right:  
            mid = (left + right) // 2  
            if can_div(mid):  
                right = mid  
            else:  
                left = mid + 1  
        return right
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![[Pasted image 20250223231418.png]]




### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135

思路：
和上题很类似。对于给定的“开销最多的fajo月的开销的最低值“m，都可以计算确定它最少需要多少的fajo月。再对给定的m的范围进行binary search的套路即可。
注意can_div函数的临界条件。

代码：

```python
N, M = map(int, input().split())  
expense_lst = [int(input()) for _ in range(N)]  
  
def can_div(m:int):  
    operations = 1 
    current_sum = 0  
    for exp in expense_lst:  
        current_sum += exp  
        if current_sum > m:
            operations += 1  
            current_sum = exp  
    return operations <= M  
  
left = max(expense_lst)  
right = sum(expense_lst)  
while left < right:  
    mid = (left + right) // 2  
    if can_div(mid):  
        right = mid  
    else:  
        left = mid + 1  
print(left)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![[Pasted image 20250223235817.png]]


### 27300: 模型整理

http://cs101.openjudge.cn/practice/27300/

思路：
自己的办法很朴素，用字典，key=模型名称，value=该名称对应的所有模型的参数量列表。再依次对key和value中列表值排序，输出即可。
ChatGPT也没有思路上更好些的方法（可能这题本就无技巧可言？）。唯一的收获是
dict.setdefault(key, default=None)
此处dic.setdefault(name, [])
这个用法可以简化我向字典里添键值对的代码。


代码：

```python
n = int(input())  
dic = {}  
for _ in range(n):  
    name, amount =input().split('-')  
    if name in dic:  
        dic[name].append(amount)  
    else:  
        dic[name] = []  
        dic[name].append(amount)  
  
def amounts(m:str):  
    a = float(m[:-1])  
    if m.endswith('B'):  
        return a * (10 ** 9)  
    elif m.endswith('M'):  
        return a * (10 ** 6)  
  
for name in sorted(dic.keys()):  
    dic[name].sort(key = amounts)  
    result = ', '.join(dic[name])  
    print(f'{name}: {result}')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>
![[Pasted image 20250224221703.png]]




### Q5. 大语言模型（LLM）部署与测试

本任务旨在本地环境或通过云虚拟机（如 https://clab.pku.edu.cn/ 提供的资源）部署大语言模型（LLM）并进行测试。用户界面方面，可以选择使用图形界面工具如 https://lmstudio.ai 或命令行界面如 https://www.ollama.com 来完成部署工作。

测试内容包括选择若干编程题目，确保这些题目能够在所部署的LLM上得到正确解答，并通过所有相关的测试用例（即状态为Accepted）。选题应来源于在线判题平台，例如 OpenJudge、Codeforces、LeetCode 或洛谷等，同时需注意避免与已找到的AI接受题目重复。已有的AI接受题目列表可参考以下链接：
https://github.com/GMyhf/2025spring-cs201/blob/main/AI_accepted_locally.md

请提供你的最新进展情况，包括任何关键步骤的截图以及遇到的问题和解决方案。这将有助于全面了解项目的推进状态，并为进一步的工作提供参考。

借助lmstudio本地部署了deepseek R1 7b版，占用内存似乎还好，但是很慢（所以没有尝试很多题）。02524:宗教信仰 经过提醒也AC不了…但是部署的过程挺有趣，比想象中简单些。
看到新闻说Ollama默认配置存在未授权访问与模型窃取等安全隐患？
### Q6. 阅读《Build a Large Language Model (From Scratch)》第一章

作者：Sebastian Raschka

请整理你的学习笔记。这应该包括但不限于对第一章核心概念的理解、重要术语的解释、你认为特别有趣或具有挑战性的内容，以及任何你可能有的疑问或反思。通过这种方式，不仅能巩固你自己的学习成果，也能帮助他人更好地理解这一部分内容。


## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>
出于提升计算机水平的决心报了这个班，然而由于上个学期计概B摆烂+没有做寒假选做，有点跟不上。第一周有补寒假的每日选做，主要学习了贪心和栈（也是上学期准备笔试时思考了很久的一道程序理解题）和二分查找，做了相关题目。近期打算再补上dp、链表、二叉树。
由于基础薄弱，做题时尽量先独立思考。在自学新算法和debug的过程中ai真的帮了大忙。不过最终还是得自己能完整实现。
除了《Build a Large Language Model (From Scratch)》，还读了推荐的《黄仁勋：英伟达之芯》，感受了黄仁勋的非凡才能和个人魅力，尤其对“工程师思维”有了更多理解，挺有收获。