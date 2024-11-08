# **LeetCode LRU缓存 (LRU Cache)**

## **题目描述**
设计和实现一个 **LRU (Least Recently Used)** 缓存机制。它应该支持以下操作：
- `get(key)`：如果缓存中存在 `key`，则返回 `value`，否则返回 `-1`。
- `put(key, value)`：如果缓存已满，移除最久未使用的项，然后插入新的 `key-value` 对。如果 `key` 已存在，则更新其 `value`。

**注意**：缓存的容量是一个正整数。

---

## **解法：使用哈希表和双向链表**
### **思路**
1. 使用一个**哈希表**存储键值对。
2. 使用**双向链表**保持元素的使用顺序，头部表示最近使用，尾部表示最久未使用。
3. 当访问元素时，将其移动到链表的头部；当缓存满时，移除链表尾部元素。

### **代码实现**

```java
import java.util.HashMap;

class LRUCache {
    private class Node {
        int key, value;
        Node prev, next;
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private HashMap<Integer, Node> cache;
    private Node head, tail;
    private int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        cache = new HashMap<>();
        head = new Node(0, 0); // 虚拟头节点
        tail = new Node(0, 0); // 虚拟尾节点
        head.next = tail;
        tail.prev = head;
    }

    private void addNode(Node node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(Node node) {
        Node prev = node.prev;
        Node next = node.next;
        prev.next = next;
        next.prev = prev;
    }

    private Node popTail() {
        Node res = tail.prev;
        removeNode(res);
        return res;
    }

    public int get(int key) {
        if (!cache.containsKey(key)) return -1;
        Node node = cache.get(key);
        removeNode(node);
        addNode(node);
        return node.value;
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            removeNode(node);
            node.value = value;
            addNode(node);
        } else {
            Node newNode = new Node(key, value);
            if (cache.size() >= capacity) {
                Node tail = popTail();
                cache.remove(tail.key);
            }
            addNode(newNode);
            cache.put(key, newNode);
        }
    }
}
