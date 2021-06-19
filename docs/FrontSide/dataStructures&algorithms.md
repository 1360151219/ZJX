# javascript(ES6) 前端数据结构与算法

---

## Stack 栈结构

---

### 栈常见的操作

- `push()` 添加一个新元素到栈顶位置。
- `pop()` 移除栈顶的元素，同时返回被移除的元素。
- `peek()` 返回栈顶的元素，不对栈做任何修改（该方法不会移除栈顶的元素，仅仅返回它）。
- `isEmpty()` 如果栈里没有任何元素就返回 `true`，否则返回 `false`。
- `size()` 返回栈里的元素个数。这个方法和数组的 `length` 属性类似。
- `toString()` 将栈结构的内容以字符串的形式返回。

### JavaScript 实现栈结构

```js
class Stack {
  constructor() {
    this.items = [];
  }
  push(el) {
    this.items.push(el);
  }
  pop() {
    return this.items.pop();
  }
  peek() {
    if (this.items.length === 0) return null;
    return this.items[this.items.length - 1];
  }
  isEmpty() {
    return this.items.length === 0;
  }
  size() {
    return this.items.length;
  }
  toString() {
    let str = "";
    for (let i = 0; i < this.items.length; i++) {
      str += this.items[i];
    }
    return str;
  }
}
```

### 利用栈结构实现 十进制变成二进制

---

```js
function toBinary(num) {
  let stack = new Stack();
  while (num > 0) {
    stack.push(num % 2);
    num = Math.floor(num / 2);
  }
  let res = "";
  while (!stack.isEmpty()) {
    res += stack.pop();
  }
  return res;
}
```

## Queue 队列

---

### 队列常见的操作

- `enqueue(element)` 向队列尾部添加一个（或多个）新的项。
- `dequeue()` 移除队列的第一（即排在队列最前面的）项，并返回被移除的元素。
- `front()` 返回队列中的第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动（不移除元素，只返回元素信息与 Map 类的 peek 方法非常类似）。
- `isEmpty()` 如果队列中不包含任何元素，返回 true，否则返回 false。
- `size()` 返回队列包含的元素个数，与数组的 length 属性类似。
- `toString()` 将队列中的内容，转成字符串形式。

### JavaScript 实现队列

---

```js
class Queue {
  constructor() {
    this.items = [];
  }
  enqueue(el) {
    this.items.push(el);
  }
  dequeue() {
    return this.items.shift();
  }
  front() {
    if (this.items.length === 0) return null;
    return this.items[0];
  }
  isEmpty() {
    return this.items.length === 0;
  }
  size() {
    return this.items.length;
  }
  toString() {
    let str = "";
    for (let i = 0; i < this.items.length; i++) {
      str += this.items[i];
    }
    return str;
  }
}
```

### 队列的应用

使用队列实现小游戏：**击鼓传花**。

?> 分析：传入一组数据集合和设定的数字 number，循环遍历数组内元素，遍历到的元素为指定数字 number 时将该元素删除，直至数组剩下一个元素。

```js
function passGame(nameList, num) {
  let queue = new Queue();
  /* 先插入元素 */
  for (let i = 0; i < nameList.length; i++) {
    queue.enqueue(nameList[i]);
  }
  while (queue.size() > 1) {
    for (let i = 0; i < num - 1; i++) {
      /* 把队列头部的元素放在尾部，这样就算num大于队列长度也不会出错*/
      queue.enqueue(queue.dequeue());
    }
    queue.dequeue();
  }
  return queue.front();
}
```

### PriorityQueue 优先级队列

---

优先级队列主要考虑的问题：

- 每个元素不再只是一个数据，还包含优先级。
- 在添加元素过程中，根据优先级放入到正确位置。

```js
// 优先队列内部的元素类
class QueueElement {
  constructor(element, priority) {
    this.element = element;
    this.priority = priority;
  }
}

// 优先队列类（继承 Queue 类）
export class PriorityQueue extends Queue {
  constructor() {
    super();
  }

  // enqueue(element, priority) 入队，将元素按优先级加入到队列中
  // 重写 enqueue()
  enqueue(el, priority) {
    // 根据传入的元素，创建 QueueElement 对象
    const queueElement = new QueueElement(el, priority);
    if (this.isEmpty()) this.items.push(queueElement);
    else {
      let added = false;
      /* 优先级越小越高 */
      for (let i = 0; i < this.size(); i++) {
        if (this.items[i].priority > queueElement.priority) {
          this.items.splice(i, 0, queueElement);
          added = true;
          break;
        }
      }
      if (!added) {
        this.items.push(queueElement);
      }
    }
  }

  // dequeue() 出队，从队列中删除前端元素，返回删除的元素
  // 继承 Queue 类的 dequeue()
  dequeue() {
    return super.dequeue();
  }

  // front() 查看队列的前端元素
  // 继承 Queue 类的 front()
  front() {
    return super.front();
  }

  // isEmpty() 查看队列是否为空
  // 继承 Queue 类的 isEmpty()
  isEmpty() {
    return super.isEmpty();
  }

  // size() 查看队列中元素的个数
  // 继承 Queue 类的 size()
  size() {
    return super.size();
  }

  // toString() 将队列中元素以字符串形式返回
  // 重写 toString()
  toString() {
    let result = "";
    for (let item of this.items) {
      result += item.element + "-" + item.priority + " ";
    }
    return result;
  }
}
```

![数组、栈和队列图解](https://cdn.jsdelivr.net/gh/XPoet/image-hosting@master/JavaScript-数据结构与算法/image.64kg5ej56vk0.png)

## 链表

---

### 单向链表

> 单向链表是由一个一个节点链接而成的，链表只可以从 **head** 开始遍历。链表元素在内存中不必是连续的空间，因此可以实现内存动态管理；它的长度可以无限延申；插入以及删除元素的效率高

> **节点的封装**：

```js
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}
```

**单向链表的封装**:

```js
class LinkedList {
  constructor() {
    this.length = 0;
    this.head = null;
  }
  /* 方法 */
  append(data) {
    let newNode = new Node(data);
    if (!this.head) {
      this.head = newNode;
    } else {
      let current = this.head;
      while (current.next) {
        current = current.next;
      }
      current.next = newNode;
    }
    this.length++;
  }
  insert(position, data) {
    let newNode = new Node(data);
    if (position < 0 || position > this.length) return false;
    if (position === 0) {
      newNode.next = this.head;
      this.head = newNode;
    } else {
      let index = 0;
      let current = this.head;
      let prev = null;
      while (index++ < position) {
        prev = current;
        current = current.next;
      }
      newNode.next = current;
      prev.next = newNode;
    }
    this.length++;
    return true;
  }
  get(position) {
    let index = 0;
    let current = this.head;
    if (position < 0 || position > this.length - 1) return null;
    while (index++ < position) {
      current = current.next;
    }
    return current;
  }
  indexOf(data) {
    if (!this.head) return -1;
    let current = this.head;
    let index = 0;
    while (current) {
      if (current.data === data) {
        return index;
      }
      current = current.next;
      index++;
    }
    return -1;
  }
  removeAt(position) {
    if (position < 0 || position > this.length - 1) return null;
    let index = 0;
    let current = this.head;
    let prev = null;
    if (position === 0) {
      this.head = current.next;
    } else {
      while (index++ < position) {
        prev = current;
        current = current.next;
      }
      prev.next = current.next;
      current.next = null;
    }
    this.length--;
    return current.data;
  }
  update(position, data) {
    let res = this.removeAt(position);
    this.insert(position, data);
    return res;
  }
  remove(data) {
    const index = this.indexOf(data);
    if (index === -1) return;
    this.removeAt(index);
  }
  isEmpty() {
    return this.length === 0 ? true : false;
  }
  size() {
    return this.length;
  }
}
```

- `append()`: 在链表尾部插入节点。
- `insert()`：找到位置索引，引入当前节点和上一位节点，先将新节点指向当前节点，再断开上一位的链接，并指向新节点。
- `removeAt()`：找到位置索引，引入当前节点和上一位节点，先将上一位节点指向当前节点的后一位节点，再将当前节点指向 NULL，此时当前节点就断开了与链表的链接，会被自动清除内存空间。

### 双向链表

> 双向链表与单向链表最大的不同，就是每个节点还有一个 prev 指针，指向前一个节点。首节点的 prev 以及未节点的 next 为 null。链表除了 head 以外，还有一个 tail 指向尾节点

我们可以使用继承来实现双向链表的封装：

```js
class DoubleNode extends Node {
  constructor(data) {
    super(data);
    this.prev = null;
  }
}
```

```js
export class DoublyLinkedList extends LinkedList {
  constructor() {
    super();
    this.tail = null;
  }
}
```

然后让我们用 JavaScript 来实现双向链表的各种方法吧！

- `append` 添加元素

!> 这里要注意节点的 next 和 prev 的指向

```js
append(data) {
        const newNode = new DoubleNode(data)
        if (!this.head) {
            this.head = newNode
            this.tail = newNode
        } else {
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
        }
        this.length++
    }
```

- `insert` 插入元素

!> 这里要注意插入节点的时候，前后两个节点的 next 和 prev 的指向

```js
insert(position, data) {
        if (position < 0 || position > this.length) return false
        const newNode = new DoubleNode(data)
        /* 特殊情况 */
        if (position === 0) {
          if(!this.head){
            this.head = newNode
            this.tail = newNode
          }else{
            this.head.prev = newNode
            newNode.next = this.head
            this.head = newNode
          }
        } else if (position === this.length) {
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
        } else {
            let current = this.head
            let prevnode = null
            let index = 0
            while (index++ < position) {
                prevnode = current
                current = current.next
            }
            /* 这里注意有4个指针需要断开重连 */
            current.prev = newNode
            newNode.next = current
            newNode.prev = prevnode
            prevnode.next = newNode
        }
        this.length++
        return true
}
```

- `removeAt` 删除元素

```js
removeAt(position) {
        if (position < 0 || position > this.length - 1) return false
        if (position === 0) {
            if (this.length === 1) {
                this.head = null
                this.tail = null
            } else {
                this.head.next.prev = null
                this.head = this.head.next
            }
        } else if (position === this.length - 1) {
            this.tail = this.tail.prev
            this.tail.next = null
        } else {
            let index = 0
            let current = this.head
            let prevnode = null
            if (index++ < position) {
                prevnode = current
                current = current.next
            }
            prevnode.next = current.next
            current.next.prev = prevnode
            /* current.next = null
            current.prev = null */
        }
        this.length--
        return true
    }

```

- 顺序倒序输出字符串

```js
/*  链表数据从后往前以字符串形式返回 */
    backwardString() {
        let current = this.tail
        let str = ''
        while (current) {
            str += current.data + '---'
            current = current.prev
        }
        return str
    }
    /*  链表数据从前往后以字符串形式返回 */
    forwardToString() {
        let current = this.head
        let str = ''
        while (current) {
            str += current.data + '---'
            current = current.next
        }
        return str
    }
```
