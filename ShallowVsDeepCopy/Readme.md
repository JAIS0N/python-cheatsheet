
Readme · MD
Copy

# 🐍 Python Basics: Shallow Copy & Deep Copy
 
> Understanding the differences between shallow copy and deep copy, and when to use each.
 
---
 
## 📋 Table of Contents
 
- [The Problem You Might Not Expect](#-the-problem-you-might-not-expect)
- [How Assignments Work in Python](#-how-assignments-work-in-python)
- [What Happens When We Copy a List](#-what-happens-when-we-copy-a-list)
- [Shallow Copy vs Deep Copy](#-shallow-copy-vs-deep-copy)
- [How to Perform Shallow Copy](#-how-to-perform-shallow-copy)
- [How to Perform Deep Copy](#-how-to-perform-deep-copy)
- [Quick Reference](#-quick-reference)
 
---
 
## ❓ The Problem You Might Not Expect
 
Guess the output of this program:
 
```python
list_a = ['a', 'b']
list_b = list_a
list_b[0] = 'c'
print(list_b)
print(list_a)
```
 
You might expect:
 
```python
['c', 'b']  # list_b
['a', 'b']  # list_a — unchanged?
```
 
But the actual result is:
 
```python
['c', 'b']  # list_b
['c', 'b']  # list_a — also changed! 😱
```
 
`list_a` changes as `list_b` changes. This README explains why — and how to fix it.
 
---
 
## 🔗 How Assignments Work in Python
 
When you write `y = x`, Python does **not** create a new variable. Instead, `y` points to the **same memory location** as `x`.
 
```python
>>> x = 5
>>> y = x
>>> id(x)
4297637024
>>> id(y)
4297637024  # same address!
```
 
Reassigning `y` to a new value gives it its own memory location:
 
```python
>>> y = 1
>>> id(y)
4297636896  # different now
>>> id(x)
4297637024  # x is unchanged
```
 
> **Note:** Assignment statements in Python do not copy objects — they create **bindings** between a target and an object.
 
---
 
## 📦 What Happens When We Copy a List
 
Reassigning the list variable entirely works fine:
 
```python
list_a = ['a', 'b']
list_b = list_a
list_b = ['e', 'f']   # new assignment
print(list_a)         # ['a', 'b'] ✅
```
 
But **mutating an element** is problematic:
 
```python
list_a = ['a', 'b']
list_b = list_a
list_b[0] = 'c'       # mutation
print(list_a)         # ['c', 'b'] ❌ both changed!
```
 
Mutating an element does not give the list a new memory address — both variables still point to the same location. This is where we need an **actual copy**.
 
---
 
## ⚖️ Shallow Copy vs Deep Copy
 
Both are used to create independent copies. The difference only matters for **compound objects** (objects containing other objects, like nested lists).
 
| | Shallow Copy | Deep Copy |
|---|---|---|
| New outer object | ✅ Yes | ✅ Yes |
| Copies child objects | ❌ No (references) | ✅ Yes (recursively) |
| Safe for flat lists | ✅ Yes | ✅ Yes |
| Safe for nested lists | ❌ No | ✅ Yes |
 
---
 
## 🔁 How to Perform Shallow Copy
 
### Method 1 — `copy` module
 
```python
import copy
copy.copy(x)
```
 
### Method 2 — Factory functions
 
```python
new_list = list(original_list)
new_dict = dict(original_dict)
new_set  = set(original_set)
```
 
### Method 3 — List slicing
 
```python
list1 = ['a', 'b', 'c', 'd']
list2 = list1[:]
list2[1] = 'x'
print(list2)  # ['a', 'x', 'c', 'd']
print(list1)  # ['a', 'b', 'c', 'd'] ✅ unchanged
```
 
### ⚠️ Shallow copy fails with nested lists
 
```python
list1 = ['a', 'b', ['ccc', 'ddd']]
list2 = list1[:]
list2[2][0] = 'E'
print(list2)  # ['a', 'b', ['E', 'ddd']]
print(list1)  # ['a', 'b', ['E', 'ddd']] ❌ also changed!
```
 
> **Note:** Shallow copy is only **one level deep**. It does not recursively copy child objects. For nested structures, use deep copy.
 
---
 
## 🔃 How to Perform Deep Copy
 
```python
import copy
copy.deepcopy(x)
```
 
Deep copy handles nested structures correctly:
 
```python
import copy
 
list1 = ['a', 'b', ['ccc', 'ddd']]
list2 = copy.deepcopy(list1)
list2[2][0] = 'E'
 
print(list2)  # ['a', 'b', ['E', 'ddd']]
print(list1)  # ['a', 'b', ['ccc', 'ddd']] ✅ unchanged!
```
 
---
 
## 📌 Quick Reference
 
| Situation | Use |
|---|---|
| Flat list, simple mutation | Shallow copy |
| Nested list / compound object | Deep copy |
| `copy.copy(x)` | Shallow copy |
| `list(x)` / `x[:]` | Shallow copy |
| `copy.deepcopy(x)` | Deep copy |
| `y = x` | ❌ Not a copy — just a binding! |
 
---
