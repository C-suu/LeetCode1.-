# LeetCode1.两数之和：


## 给定一个整数数组 nums 和一个整数目标值 target，请在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。可以假设每种输入只会对应一个答案，并且不能使用两次相同的元素。可以按任意顺序返回答案。



## 时间复杂度 O(n)，符合要求的：“你可以想出一个时间复杂度小于 O(n2) 的算法吗？”

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}  # 用来记录：某个数 -> 它的下标

        for i, x in enumerate(nums):
            need = target - x
            if need in seen:
                return [seen[need], i]
            seen[x] = i
```

---

# 1）对这段代码的理解

需要找两个数 `a + b = target`。每次拿到一个数 `x`，就去想：

> “如果拿到了 `x`，那还差 `need = target - x` 才能凑成 target。”

然后用一个 **字典（dict）** 存储 **我之前见过的数** 和它们的 **下标** ：

* 如果 `need` 以前出现过，那答案就找到了：返回 `[need 的下标, 当前 x 的下标]`
* 否则就把当前 `x` 的下标存起来，继续往后看。

---

# 2）拆解代码

---

## A. 第一行

```python
from typing import List
```

* `from`：从……里面
* `typing`：一个“类型提示”的库（不影响运行，主要让力扣/编辑器更清楚）
* `import`：导入
* `List`：表示“列表类型”

> 也可以不写这行，但写了更规范。

---

## B. 类定义

```python
class Solution:
```

* `class`：定义一个“类”
* `Solution`：类名
* `:`：表示“下面开始是这个类的内容”

---

## C. 函数定义（要写的核心入口，在类的外部调用就是调用函数名称）

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
```

逐个符号解释：

* `def`：定义一个函数
* `twoSum`：函数名字
* `( ... )`：括号里是参数
* `self`：类里的函数都会带它，先当成“固定写法”。注意，self并不总是出现在def里面。类外部的不需要self，可以参考离线IQL的期望回归写法。
```
def expectile_loss(diff, expectile=0.7):  # IQL的期望回归，一般定义在IQL的类外部，无需self
    weight = torch.where(diff > 0, expectile, (1 - expectile))
    return weight * (diff**2)
```
* `nums`：输入的数组（列表）
* `:`：冒号后面是类型提示
* `List[int]`：表示 nums 是“整数列表”
* `target`：目标值
* `target: int`：表示 target 是整数
* `-> List[int]`：表示这个函数会返回“整数列表”
* 最后的 `:`：表示函数体开始

---

## D. 字典 seen

```python
seen = {}  # 用来记录：某个数 -> 它的下标
```

* `seen`：变量名，你可以理解为“已经见过的数”
* `=`：赋值
* `{}`：空字典（dict）
* `#`：注释

字典的作用：
`seen[值] = 下标`
比如你见到 `7` 在下标 `1`，就记为：`seen[7] = 1`

---

## E. 遍历数组（关键）

```python
for i, x in enumerate(nums):
```

逐字解释：

* `for`：循环
* `i, x`：一次取两个东西

  * `i`：下标 index
  * `x`：值 value
* `in`：在……里面
* `enumerate(nums)`：把 nums 变成“(下标, 值)”的序列
* `:`：循环体开始

举例：`nums = [2,7,11,15]`
`enumerate(nums)` 会依次产生：

* `(0, 2)`
* `(1, 7)`
* `(2, 11)`
* `(3, 15)`

---

## F. 计算“还差多少”

```python
need = target - x
```

* `need`：我还需要的那个数
* `target - x`：目标减去当前数

例如 `target=9`，当前 `x=2`
`need = 9 - 2 = 7`

---

## G. 判断 need 是否出现过

```python
if need in seen:
```

* `if`：如果
* `need in seen`：need 这个“键”是否在字典 seen 里
* `:`：if 的代码块开始

---

## H. 找到就返回答案

```python
return [seen[need], i]
```

逐个解释：

* `return`：返回（函数结束）
* `[ ... , ... ]`：列表
* `seen[need]`：在字典里找到 need 对应的“下标”
* `i`：当前这个 x 的下标

为什么顺序是 `[seen[need], i]`？

* `need` 是之前出现过的数（在 seen 里）
* `i` 是当前这个数的下标

---

## I. 如果没找到，就把当前数记下来

```python
seen[x] = i
```

意思是：

* 现在看到一个数 `x`
* 它的下标是 `i`
* 把它记进字典，给后面用

---

# 3）用示例走一遍（要真正懂）

例子：`nums = [2,7,11,15]`, `target = 9`

* seen = {}

### 第1轮

* i=0, x=2
* need = 9-2 = 7
* 7 不在 seen
* 记录 seen[2]=0
  seen = {2:0}

### 第2轮

* i=1, x=7
* need = 9-7 = 2
* 2 在 seen！
* 返回 [seen[2], 1] = [0,1]

结束 ✅

---

# 4）作为小白，怎么学会这种题？

只要记住这句话：

> “边走边记：每次看一个数，就把它存起来；同时检查我需要的另一个数之前有没有出现过。”

这是“哈希表（字典）”最经典套路之一。
