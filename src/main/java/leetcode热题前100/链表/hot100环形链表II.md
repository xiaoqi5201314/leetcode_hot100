## LeetCode 142. 环形链表 II
### 题目描述

给定一个链表的头节点 `head`，返回链表开始入环的第一个节点。如果链表无环，则返回 `null`。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 `pos` 是 `-1`，则在该链表中没有环。

**说明：** 不允许修改给定的链表。

**示例 1：**
```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**
```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**
```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```

**进阶：** 你是否可以不用额外空间解决此题？

### Java 实现解法

#### 方法：快慢指针法

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }
        ListNode fast = head;
        ListNode slow = head;
        
        // 首先使用快慢指针确定是否有环
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (fast == slow) {
                // 有环，重置一个指针到头节点，然后两个指针一起移动直到相遇，相遇点即为环的起始节点
                ListNode ptr = head;
                while (ptr != slow) {
                    ptr = ptr.next;
                    slow = slow.next;
                }
                return slow;
            }
        }
        
        // 如果没有环，则返回null
        return null;
    }
}
```
### 解题思路

- **快慢指针法**：使用两个指针 `fast` 和 `slow`，`fast` 每次移动两步，`slow` 每次移动一步。如果链表中有环，`fast` 和 `slow` 最终会在环中相遇。
- **确定环的起始节点**：一旦 `fast` 和 `slow` 相遇，将 `fast` 重置为头节点，然后两个指针以相同速度移动，当它们再次相遇时，相遇点即为环的起始节点。

这种方法的时间复杂度是 `O(n)`，空间复杂度是 `O(1)`，符合题目的进阶要求。通过原地操作，避免了使用额外的空间来存储链表的节点。这种方法简单且高效，是解决链表环问题的经典方法。
