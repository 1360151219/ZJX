# 算法日记

---

## 链表

### leetcode234.回文链表

**问题**：请判断一个链表是否为回文链表。

> 思路：利用快慢指针，慢指针一次走一步，快指针一次走两步。对于奇数链表，当快指针走到尾节点的时候，慢指针走到中间节点；对于偶数链表，当快指针走到倒数第二个节点的时候，慢指针走到前半段的尾节点处。同时，在慢指针走的时候讲前半段链表反转。（如果是奇数链表，慢指针再走一步，因为中间节点不需要比较）然后比较前半段反转的链表和后半段链表即可。

**代码实现**

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function (head) {
  let slow = head;
  let pre = null;
  let reverse = null;
  while (head && head.next) {
    pre = slow;
    slow = slow.next;
    head = head.next.next;
    /* 反转前半链表 */
    pre.next = reverse;
    reverse = pre;
  }
  if (head) slow = slow.next;
  while (slow) {
    if (slow.val === pre.val) {
      slow = slow.next;
      pre = pre.next;
    } else return false;
  }
  return true;
};
```

### 剑指 24.反转链表

> 思路：利用前中后 3 个指针，边遍历边反转。

**代码实现:**

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
  let pre = null;
  let current = head;
  if (!head) return head;
  let ahead = head.next;
  while (current) {
    current.next = pre;
    pre = current;
    current = ahead;
    if (ahead) {
      ahead = ahead.next;
    }
  }
  return pre;
};
```

### leetcode21.合并两顺序表

> 思路：主要思路如下图。这里我第一次出错的原因是循坏条件判断错误，我用的是`l1||l2`，企图想一次性把所有条件都写在一起，这样的话`l1.val`可能为空，则出错。第 2 次的话，是遍历的错误，我写成了`l=l1` / `l=l2` ，这样的话 l 根本就没有 next，整个链表就支零破碎了。

![](imgs/algorithm1.jpg)

**代码实现：**

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function (l1, l2) {
  if (l1 === null) return l2;
  if (l2 === null) return l1;
  let head = new ListNode(-1);
  let l = head;
  while (l1 != null && l2 != null) {
    if (l1.val > l2.val) {
      l.next = l2;
      l2 = l2.next;
    } else {
      l.next = l1;
      l1 = l1.next;
    }
    l = l.next;
  }
  if (l1 === null) l.next = l2;
  if (l2 === null) l.next = l1;
  return head.next;
};
```

### 剑指 offer 22. 链表第 n 个节点

- 输入一个链表，输出该链表中倒数第 k 个节点。为了符合大多数人的习惯，本题从 1 开始计数，即链表的尾节点是倒数第 1 个节点。

实例：

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

> 思路：首先遍历一遍链表，获取链表长度 n，然后重新遍历 n-k 次得到倒数第 k 个节点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function (head, k) {
  if (!head.next) return head;
  let traverse = head;
  let length = 1;
  while (traverse.next) {
    traverse = traverse.next;
    length++;
  }
  if (k === 1) return traverse;
  if (k > length) return null;
  for (let i = 0; i < length - k; i++) {
    head = head.next;
  }
  return head;
};
```

---

**二刷**

> 思路：上述的做法实际上遍历了 2 次链表，感觉还可以再优化一下。看了一下各路大佬的题解，发现一种快慢指针法只需要遍历一次链表即可。关键是让快指针先移动 k-1 步，这样的话快慢指针就分别指向所求链表的尾、头节点了。

```js
var getKthFromEnd = function (head, k) {
  if (!head) return null;
  let fast = head;
  let slow = head;
  for (let i = 0; i < k - 1; i++) {
    if (!fast) return null; /* 即 k 大于链表的长度 */
    fast = fast.next;
  }
  while (fast.next) {
    slow = slow.next;
    fast = fast.next;
  }
  return slow;
};
```

### leetcode 146. LRU 缓存机制

> 思路：这道题我们首先要想到，新的在前，旧的在后，若超出存贮极限，则把最旧的给抛弃掉。除此之外，当每次插入、更新或者获取节点的时候，都得把节点放在最前面也就是从旧变新了。因此我们可以定义几个方法：`_moveToHead`,`_remove`,`_isFull`。**`get方法`**：若 key 不存在 return -1；若存在则获取 value 并且 moveToHead。**`put`**：key 存在，则更新 value 并且 moveToHead；key 不存在，判断是否满了，若满了则删除尾节点；然后插入新节点。

> 这道题最难的地方，我认为在于时间复杂度需要 O(1)。存储节点我们首先想到的就是数组或者链表，但是当我们插入删除以及获取的时候，它们都不可避免的需要 0(n)。因此，我选择使用链表来执行插入删除，使用哈希表来获取，这样的话时间复杂度就都满足 0(1)了。hashMap 存储的是 key->node，链表存储的是 node。

```js
//第一次：
class Node {
  constructor(key, value) {
    this.key = key;
    this.data = value;
    this.next = null;
    this.prev = null;
  }
}
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.usedSpace = 0;
    this.head = new Node(null, null);
    this.tail = new Node(null, null);
    this.head.next = this.tail;
    this.tail.prev = this.head;
    this.hashmap = {};
  }
  _isFull() {
    return this.usedSpace == this.capacity ? true : false;
  }
  _remove(node) {
    node.prev.next = node.next;
    node.next.prev = node.prev;
    node.next = null;
    node.prev = null;
    return node;
  }
  _moveToHead(node) {
    let current = this.head;
    this.head = node;
    node.next = current;
    current.prev = node;
  }
  get(key) {
    if (key in this.hashmap) {
      let node = this._remove(this.hashmap[key]);
      this._moveToHead(node);
      return node.value;
    } else {
      return -1;
    }
  }
  put(key, value) {
    //
    if (!this.hashmap[key]) {
      let newNode = new Node(key, value);
      if (!this._isFull()) {
        this._moveToHead(newNode);
        this.hashmap[key] = newNode;
        this.usedSpace++;
      } else {
        delete this.hashmap[this.tail.key];
        this.hashmap[key] = newNode;
        this.tail = this.tail.prev;
        this.tail.next = null;
        this._moveToHead(newNode);
      }
    } else {
      this.hashmap[key].value = value;
      let node = this._remove(this.hashmap[key]);
      this._moveToHead(node);
    }
  }
}
```

**那么众所周知第一次往往都会失败，原因有 2 个：**

- 首先节点类的 value 写错成了 data (这个写错的地方花了我好久才发现)
- remove 方法中，如果删除的是尾节点的话则没有 node.next.prev，会报错

因此我采用两个固定的 dummyHead 以及 dummyTail 来记录下虚拟头尾节点

![](imgs/algorithm2.jpg)
代码如下：

```js
class DoubleLinkedListNode {
  constructor(key, value) {
    this.key = key;
    this.value = value;
    this.prev = null;
    this.next = null;
  }
}
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.usedSpace = 0;
    this.hashmap = {};
    this.dummyHead = new DoubleLinkedListNode(null, null);
    this.dummyTail = new DoubleLinkedListNode(null, null);
    this.dummyHead.next = this.dummyTail;
    this.dummyTail.prev = this.dummyHead;
  }
  _isFull() {
    return this.usedSpace === this.capacity;
  }
  _remove(node) {
    node.prev.next = node.next;
    node.next.prev = node.prev;
    node.prev = null;
    node.next = null;
    return node;
  }
  _moveToHead(node) {
    const head = this.dummyHead.next;
    node.next = head;
    head.prev = node;
    node.prev = this.dummyHead;
    this.dummyHead.next = node;
  }
  get(key) {
    if (key in this.hashmap) {
      const node = this.hashmap[key];
      this._moveToHead(this._remove(node));
      return node.value;
      /* let node = this._remove(this.hashmap[key])
            this._moveToHead(node)
            return node.value */
    } else {
      return -1;
    }
  }

  put(key, value) {
    if (key in this.hashmap) {
      const node = this.hashmap[key];
      node.value = value;
      this._moveToHead(this._remove(node));
      /* this.hashmap[key].value = value
            let node = this._remove(this.hashmap[key])
            this._moveToHead(node) */
    } else {
      if (this._isFull()) {
        const node = this.dummyTail.prev;
        delete this.hashmap[node.key];
        this._remove(node);
        this.usedSpace--;
      }
      const newNode = new DoubleLinkedListNode(key, value);
      this.hashmap[key] = newNode;
      this._moveToHead(newNode);
      this.usedSpace++;
    }
  }
}
```

## 字符串

### leetcode125. 验证回文串

**问题**：给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

> 思路：首先我们把字母都转成小写，利用正则来选取字母与数字，再合并成一个新的字符串。然后我们利用双指针，从头尾同时朝着中间遍历，进行比较。

**代码实现：**

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function (s) {
  let arr = s.toLowerCase().match(/[a-z0-9]+/g);
  if (!arr) return true;
  let str = arr.join("");
  let head = 0;
  let tail = str.length - 1;
  while (tail > head) {
    if (str[head] === str[tail]) {
      head++;
      tail--;
    } else {
      return false;
    }
  }
  return true;
};
```

```

```
