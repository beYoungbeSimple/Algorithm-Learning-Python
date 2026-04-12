### 链表
#### 1 基本概念
链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个属数据域，另一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向NULL。链接的入口点成为头节点，即head。
- 基本类型
  - 单链表：单链表节点只能指向节点的下一个节点；
  - 双链表：双链表的每一个节点有两个指针域，一个指向下一个节点，另一个指向上一个节点，可以向前和向后查询；
  - 循环链表：循环链表的最后一个节点指向头节点，形成一个环。
- 链表的存储方式
  - 数组在内存中连续分布，而链表是通过指针域的指针来链接内存中各个节点的。所以链表中的节点在内存中不是连续分布的，而是打乱在内存中的某地址上，分配机制取决于操作系统的内存管理。
- 在Python中，可以采用**引用+类**的方式实现链表；在C/C++中，通常采用**指针+结构体**的方式实现链表。

#### 2 链表Python实现
##### 2.1 链表的定义
将节点类定义成```Node```，```data```用来存储节点的值，```next```用来存储指向下一个节点的指针。


```python
from requests import head


class Node:
    def __init__(self, data = None, next = None):
        self.data = data
        self.next = next
node1 = Node(1)
node2 = Node(2)
node3 = Node(3)
node1.next = node2
node2.next = node3
```

##### 2.2 遍历链表
遍历```next```节点：查找、插入、删除元素。
- 查找


```python
def find(head, value):
    probe = head
    while probe is not None:
        if probe.data == value:
            return probe.next
    return None
```

- 插入


```python
def insert(node, value):
    if node is None:
        return
    new_node = Node(value)
    new_node.next = node.next
    node.next = new_node
```

- 删除


```python
def delete(head, value):
    dummy = Node(0)
    dummy.next = head
    prev, curr = dummy, head
    while curr is not None:
        if curr.data == value:
            prev.next = curr.next
            break
        prev, curr = curr, curr.next
    return dummy.next
```

#### 3 算法应用
##### 3.1 相交链表 <font color=red>LeetCode No.160</font>
给出两个单链表的头节点```headA```和```headB```，返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回```null```。

题解：
- 定义两个指针```pA```和```pB```，分别指向两个链表的头节点，同时往后走；
- 如果某个指针走到头，就跳到另一个链表的头继续走。
```python
pA = pA.next if pA else headB
pB = pB.next if pB else headA
```
- 最终要么在交点相遇，要么同时为None。

示例：
- A: a1->a2->c1->c2->c3
- B: b1->b2->b3->c1->c2->c3
- pA: a1->a2->c1->c2->c3->b1->b2->b3->**c1**->c2->c3
- pB: b1->b2->b3->c1->c2->c3->a1->a2->**c1**->c2->c3


```python
def getIntersectionNode(headA, headB):
    pA = headA
    pB = headB
    while pA != pB:
        pA = pA.next if pA else headB
        pB = pB.next if pB else headA
    return pA
```

##### 3.2 反转链表 <font color=red>LeetCode No.206</font>
给定一个单链表的头节点```head```，反转链表并返回反转后的头节点。
- 输入：```head = [1, 2, 3, 4, 5]```
- 输出：```[5, 4, 3, 2, 1]```


```python
def reverseList(head):
    prev = None
    curr = head

    while curr:
        next_temp = curr.next   # 保存下一个节点
        curr.next = prev        # 反转指针
        prev = curr             # prev前进
        curr = next_temp        # curr前进
    return prev
```

##### 3.3 回文链表 <font color=red>LeetCode No.234</font>
给定一个单链表的头节点```head```，判断链表是否是回文链表。
- 输入：```head = [1, 2, 2, 1]```
- 输出：```true```
- 输入：```head = [1, 2]```
- 输出：```false```

题解：
- 复制链表值到数组列表
- 使用双指针判断是否为回文


```python
def isPalindrome(head):
    vals = []
    currentNode = head
    while currentNode is not None:
        vals.append(currentNode.val)
        currentNode = currentNode.next
    return vals == vals[::-1]
```

##### 3.4 环形链表 <font color=red>LeetCode No.141</font>
给定一个单链表的头节点```head```，判断链表中是否有环。

方法一：使用哈希表记录访问过的节点，如果再次访问到已经访问过的节点，则说明链表中有环。
时间复杂度为O(n)，空间复杂度为O(n)。


```python
def hasCycleHash(head):
    seen = set()
    while head:
        if head in seen:
            return True
        seen.add(head)
        head = head.next
    return False
```

方法二：使用快慢指针，快指针每次移动两步，慢指针每次移动一步，如果链表中有环，则快慢指针最终会相遇。
时间复杂度为O(n)，空间复杂度为O(1)。


```python
def hasCycleTwoPointers(head):
    if not head or not head.next:
        return False
    slowPoint = head
    fastPoint = head.next
    while slowPoint != fastPoint:
        if not fastPoint or not fastPoint.next:
            return False
        slowPoint = slowPoint.next
        fastPoint = fastPoint.next.next
    return True
```
