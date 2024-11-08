## LeetCode 23. 合并K个升序链表

### 题目描述

合并 `k` 个升序链表，返回合并后的链表。链表的节点数值按升序排列。

**示例：**
```
输入：lists = [[1,4,5], [1,3,4], [2,6]]
输出：[1,1,2,3,4,4,5,6]
```

**提示：**
- `k == lists.length`
- `0 <= k <= 10^4`
- `0 <= lists[i].length <= 500`
- `-10^4 <= lists[i][j] <= 10^4`

### Java 实现解法

#### 方法一：优先队列（最小堆）

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0)
            return null;
        PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);
        for (ListNode node : lists) {
            if (node != null) {
                minHeap.add(node);
            }
        }
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        while (!minHeap.isEmpty()) {
            ListNode minNode = minHeap.poll();
            curr.next = minNode;
            curr = curr.next;
            if(minNode.next != null){
                minHeap.add(minNode.next);
            }
        }
        return dummy.next;
    }
}
```

### 解题思路

- **优先队列（最小堆）**：使用一个最小堆来存储所有链表的头节点，按照节点的值来排序。
  - 初始化最小堆，将所有链表的头节点加入堆中。
  - 每次从堆中取出最小的节点，将其添加到结果链表中，然后将该节点的下一个节点加入堆中。
  - 重复这个过程，直到所有节点都被取出并合并。

这种方法的时间复杂度是 `O(N log k)`，其中 `N` 是所有链表中节点的总数，`k` 是链表的数量。空间复杂度是 `O(k)`，因为我们需要存储所有链表的头节点在堆中。这种方法利用了优先队列来高效地找到最小的节点并合并，是解决多个有序链表合并问题的高效方法。
