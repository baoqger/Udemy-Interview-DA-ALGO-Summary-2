# Udemy-Interview-DA-ALGO-Summary-2
第二部分
为了排版和操作的方便，分成上下两个部分

## Circular List

Circular Lists:
判断一个linked list是否是circled，这个解决方案也很巧妙，如下：
```
function circular(list) {
  let slow = list.getFirst();
  let fast = list.getFirst();

  while (fast.next && fast.next.next) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow === fast) {
      return true;
    }
  }

  return false;
}
```
最开始想了一个问题，就是fast的步距是slow的两倍，那么就一定能保证fast和slow相遇吗？这个问题有点儿意思了。

## Tree

这个树结构的实现，我觉得有点儿奇怪。本身tree类没有什么方法，都靠Node类的add和remove, 但是靠Node类，也无法控制给具体哪个node添加children的，看来还是要看看别的实现方法。

另外，就是深度优先和广度优先两种遍历的实现：维护一个array，从array的头部不断的shift元素出来，然后把这个元素的children不断的往array里面添加，不同的是，广度优先需要不断的push到array的end，而深度优先就需要不断的unshift到array的head上。
```
class Node {
  constructor(data) {
    this.data = data;
    this.children = [];
  }

  add(data) {
    this.children.push(new Node(data));
  }

  remove(data) {
    this.children = this.children.filter(node => {
      return node.data !== data;
    });
  }
}

class Tree {
  constructor() {
    this.root = null;
  }
  // 广度优先
  traverseBF(fn) {
    // 初始化一个数组
    const arr = [this.root];
    // 循环判断
    while (arr.length) {
      // 取得头部的元素
      const node = arr.shift();
      // 把头部元素的children push到array里
      arr.push(...node.children);
      fn(node);
    }
  }
  // 深度优先
  traverseDF(fn) {
    const arr = [this.root];
    while (arr.length) {
      const node = arr.shift();

      arr.unshift(...node.children);
      fn(node);
    }
  }
}

module.exports = { Tree, Node };
```

## Tree level width

计算tree结构每一层的宽度，比如下面这样：
```
Given the root node of a tree, return
an array where each element is the width
of the tree at each level.
--- Example
Given:
    0
  / |  \
1   2   3
|       |
4       5
Answer: [1, 3, 2]
```
解决方案如下：
```
function levelWidth(root) {
  const arr = [root, 's'];
  const counters = [0];

  while (arr.length > 1) {
    const node = arr.shift();

    if (node === 's') {
      counters.push(0);
      arr.push('s');
    } else {
      arr.push(...node.children);
      counters[counters.length - 1]++;
    }
  }

  return counters;
}
```
其实本质上是利用广度优先的搜索，让计算width明显就是广度优先了，关键是如何区分不同的level。这里是在array加上一个's'标识符，用来分隔不同的level数据。
这个trick还是很巧妙的。

## Events

在这个课程项目中，莫名其妙的加入了这个section，讲JS的事件系统，自己构建一个events类来实现类似eventEmitter的效果，代码如下：
```
// --- Directions
// Create an 'eventing' library out of the
// Events class.  The Events class should
// have methods 'on', 'trigger', and 'off'.

class Events {
  constructor() {
    this.events = {};
  }
  
  // Register an event handler
  on(eventName, callback) {
    if (this.events[eventName]) {
      this.events[eventName].push(callback);
    } else {
      this.events[eventName] = [callback];
    }
  }

  // Trigger all callbacks associated
  // with a given eventName
  trigger(eventName) {
    if (this.events[eventName]) {
      for (let cb of this.events[eventName]) {
        cb();
      }
    }
  }

  // Remove all event handlers associated
  // with the given eventName
  off(eventName) {
    delete this.events[eventName];
  }
}
```
感觉上跟Observer观察者模式类似，可以想一想他们的区别了，对比出真知。

## Binary Search Tree

二叉树是树的一个类型，二叉搜索树又是二叉树的一个具体特例。Binary Search Tree的定义是：每个节点最多有两个children（所谓二叉树），而且left子节点比本节点小，right子节点比本节点大。因为这种大小关系的特性，在搜索的时候效率会特别高。

```
// 1) Implement the Node class to create
// a binary search tree.  The constructor
// should initialize values 'data', 'left',
// and 'right'.
// 2) Implement the 'insert' method for the
// Node class.  Insert should accept an argument
// 'data', then create an insert a new node
// at the appropriate location in the tree.
// 3) Implement the 'contains' method for the Node
// class.  Contains should accept a 'data' argument
// and return the Node in the tree with the same value.
// If the value isn't in the tree return null.

class Node {
  constructor(data) {
    // 每个node三个属性 
    this.data = data;
    this.left = null;
    this.right = null;
  }

  insert(data) {
    if (data < this.data && this.left) {
      //对left子节点的递归insert
      this.left.insert(data); 
    } else if (data < this.data) {
      // 赋值给left子节点
      this.left = new Node(data);
    } else if (data > this.data && this.right) {
      //对right子节点的递归insert
      this.right.insert(data);
    } else if (data > this.data) {
      // 赋值给right子节点
      this.right = new Node(data);
    }
  }

  contains(data) {
    if (this.data === data) {
      return this;
    }

    if (this.data < data && this.right) {
      // right子节点的contains操作
      return this.right.contains(data);
    } else if (this.data > data && this.left) {
      // left子节点的contains操作
      return this.left.contains(data);
    }

    return null;
  }
}
```
毫无疑问依靠递归来说实现的。
