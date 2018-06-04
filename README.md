# Udemy-Interview-DA-ALGO-Summary-2
第二部分
为了排版和操作的方便，分成上下两个部分

## String Reverse

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
