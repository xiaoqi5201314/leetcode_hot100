## LeetCode 19. 删除链表的倒数第 N 个结点

### 题目描述

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**示例 1：**
```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**
```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**
```
输入：head = [1,2], n = 1
输出：[1]
```

**提示：**
- 链表节点数在范围 `[1, 10^5]` 内
- `1 <= Node.val <= 10^5`
- `1 <= n <= 10^5`

### Java 实现解法

#### 方法一：双指针法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head == null || n == 0){
            return head;
        }
        ListNode fast = head;
        ListNode slow = head;
        for(int i = 0;i < n;i++){
            fast = fast.next;
        }
        if(fast == null){
            return head.next;
        }
        while(fast.next != null){
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```

### 解题思路

- **双指针法**：使用两个指针 `fast` 和 `slow`，`fast` 指针先移动 `n` 步，然后两个指针一起移动，直到 `fast.next` 指针到达链表末尾。
  - `fast` 指针先向前移动 `n` 步，这样 `fast` 和 `slow` 之间的距离就是 `n`。
  - 然后同时移动 `fast` 和 `slow` 指针，直到 `fast.next` 指针到达链表末尾。
  - 当 `fast.next` 指针到达末尾时，`slow` 指针就位于倒数第 `n` 个节点，我们可以将其下一个节点从链表中断开，从而删除倒数第 `n` 个节点。

这种方法的时间复杂度是 `O(L)`，其中 `L` 是链表的长度。空间复杂度是 `O(1)`，因为我们只使用了常数个额外的指针。这种方法简单且高效，是解决这类问题的标准方法。
