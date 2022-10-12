## 链表元素去重：

- LeetCode第83题。

- 我们之前讨论过数组原地去重的算法：快慢指针。

- 对于上述问题，快慢指针的优点：
  - 快慢指针在进行元素去重的时候是不需要在数组中插入或删除元素的，它把重复元素的删除转换成了元素的移动，这正好符合数组大小固定的特点。
  - 其次，快慢指针的实现需要知道数组的大小。

- 上述两个优点，正好可以完美移植到链表上面：
  - 把删除的过程转换成元素的移动，减小开销。
  - 通过NULL定位链表的结尾。

- 这样做有什么优势呢？
  - 第一点：相较于普通的去重，不需要生成一个哈希数组(检测哪些元素重复出现)，减小空间开销。
  - 第二点：在两个节点之间删除节点的时候，需要进行连接操作,使用快慢指针可以避免这些操作(因为在去重的时候不需要删除节点)

- 这里需要注意一下，移动完成之后，slow指向的是新链表的最后一个节点，我们**可以选择**把这个节点以后的其他节点**删除**(因为这些都是没用的节点)，删除方法：

  - ``delete_head = slow->next``，由于节点都是使用``动态分配``创建的，因此循环将以``delete_head``为头的链表释放即可。

- 注意，最后执行``slow->next = nullptr``。

  - 对于Java这类语言，执行完这句话之后，上面要清除的节点会自动被清除，而对于C++来讲，还是需要执行上面的手动清除步骤。

- 代码如下：

  - ```cpp
    ListNode* deleteDuplicates(ListNode* head)
    {
        if (head == nullptr || head->next == nullptr)
            return head;
        ListNode* slow = head;
        ListNode* fast = head->next;
        while (fast != nullptr)
        {
            if (slow->val == fast->val)
                fast = fast->next;
            else
            {
                slow = slow->next;
                slow->val = fast->val;
                fast = fast->next;
            }
        }
        slow->next = nullptr;
        return head;
    }

