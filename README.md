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
