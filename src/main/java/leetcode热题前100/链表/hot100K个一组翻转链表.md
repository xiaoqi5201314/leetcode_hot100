## LeetCode 25. K 个一组翻转链表

### 题目描述

给你链表的头节点 `head`，每 `k` 个节点一组进行翻转，请你返回修改后的链表。`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么最后剩余的节点保持原有顺序。

**示例 1：**
```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**
```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

**提示：**
- 链表中的节点数目为 `n`
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

### Java 实现解法

#### 方法一：迭代

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
     public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null || k == 1){
            return head;
        }
        // 创建虚节点
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        // 每一组的前缀节点
        ListNode preGroup = dummy;
        while(true){
            // 每一组的开始节点
            ListNode start = preGroup.next;
            // 获取每一组的结束节点
            ListNode end = start;
            for(int i = 0; i < k - 1 && end != null; i++){
                end = end.next;
            }
            // 如果不足k个节点，则停止
            if(end == null) break;
            // 记录下一组的开始节点
            ListNode nextStart = end.next;
            // 断裂与下一组的连接
            end.next = null;
            // 反转当前组
            ListNode currGroup =  reverse(start);
            // 将当前组与前缀节点连接
            preGroup.next = currGroup;
            // 连接下一组，start已经变为end
            start.next = nextStart;
            // 更新下一组的前缀节点
            preGroup = start;
        }
        return dummy.next;
    }

    private ListNode reverse(ListNode head) {
        ListNode curr = head;
        ListNode pre = null;
        while(curr != null){
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        return pre;
    }
}
```

这个代码实现了 LeetCode 第 25 题“K 个一组翻转链表”的功能。下面是解题思路和时间复杂度与空间复杂度的分析：

### 解题思路

1. **初始化**：创建一个虚拟头节点 `dummy`，它的 `next` 指向原始链表的头节点 `head`。使用 `preGroup` 指针追踪每组翻转前的最后一个节点。
2. **循环处理每组**：使用一个无限循环来处理链表，直到所有节点都被翻转或剩余节点不足以组成一组。
   - 找到当前组的起始节点 `start` 和结束节点 `end`。如果剩余节点不足以组成一组，则退出循环。
   - 记录下一组的起始节点 `nextStart`，并将当前组的结束节点 `end` 的 `next` 设置为 `null`，从而将当前组从链表中断开。
   - 翻转当前组的节点，使用 `reverse` 函数实现。
   - 将翻转后的组连接到 `preGroup` 之后，并将下一组的起始节点连接到翻转后的组的末尾。
   - 更新 `preGroup` 为下一个未翻转的组的起始节点。
3. **翻转函数**：`reverse` 函数用于翻转链表。它使用两个指针 `pre` 和 `curr` 遍历链表，并将每个节点的 `next` 指针指向前一个节点，从而实现翻转。
### 时间复杂度`O(n)` 空间复杂度是 `O(1)`。

