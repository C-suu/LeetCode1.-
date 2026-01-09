为什么先 in 再 seen[need]，不然会报错。

好，我们就把这两行“掰开揉碎”讲清楚：为什么**必须先 `in` 再 `seen[need]`**。

---

## 代码片段

```python
if need in seen:
    return [seen[need], i]
```

---

# 1）`need in seen` 是什么意思？

逐个解释：

* `need`：“还差的那个数”
* `in`：在不在……里面
* `seen`：字典（我们记录过的数 → 下标）

所以：

```python
need in seen
```

意思是：

> `need` 这个“键 key”在不在字典 `seen` 里？

* 如果在：返回 `True`
* 如果不在：返回 `False`

---

# 2）为什么不能直接写 `seen[need]`？

因为字典取值的规则是：

```python
seen[key]
```

只有当 **key 确实存在** 时才合法。

如果 key 不存在，去取：

```python
seen[need]
```

Python 会直接报错：

> `KeyError`

意思是：**要的这个键不存在**。

---

# 3）最小例子（一看就懂）

```python
seen = {2: 0}   # 只记录了 2 在下标 0
need = 7
print(seen[need])
```

这里 `need=7`，但字典里没有 7。
所以会报错（类似）：

* `KeyError: 7`

这就是问的“为什么会报错”。

---

# 4）那为什么 `in` 能避免报错？

因为先问一句：

```python
if need in seen:
```

只有当它回答“在”（True）时，才去取：

```python
seen[need]
```

这样就保证了：**取的时候 key 一定存在**，自然不会 KeyError。

---

# 5）把它和现实类比一下（很好记）

字典 `seen` 像一个“电话簿”：

* 想查 “张三”的号码：`seen["张三"]`
* 但如果电话簿里根本没有“张三”，直接翻会翻不到（报错）

正确姿势：

1. 先看电话簿里有没有这个人：`"张三" in seen`
2. 有再查号码：`seen["张三"]`

---

# 6）如果真的想“不用 in 也不报错”可以吗？

可以，有两种常见写法，但新手先了解即可：

### 写法 A：`get`（不存在就给默认值）

```python
idx = seen.get(need)   # 不存在会返回 None
if idx is not None:
    return [idx, i]
```

### 写法 B：`try/except`（不推荐新手一开始用）

```python
try:
    return [seen[need], i]
except KeyError:
    seen[x] = i
```

力扣最常用、最清晰的仍然是：**先 `in` 再取值**。

---

## ✅ 一句话总结

> 因为 `seen[need]` 在 need 不存在时会触发 `KeyError`，所以必须先用 `need in seen` 检查它在不在，再安全地取 `seen[need]`。

如果愿意，我还可以把 `enumerate(nums)` 为什么能同时拿到 `(下标, 值)` 再“逐字符”讲一遍。
