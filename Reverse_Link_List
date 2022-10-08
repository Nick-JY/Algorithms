## 逆置链表：

- 目前逆置链表类问题有两种类型：
  - 将链表进行整体逆置：
    - 逆置整个链表是基础问题，LeetCode上第206题。
  - 将链表进行局部逆置：
    - 局部逆置链表属于中等问题，有一定难度，但也需要掌握，LeetCode上第92题。
- 无论是整体逆置链表还是局部逆置链表，基本思想都有两个：
  - 迭代：对于迭代算法来讲，时间复杂度是O(n)，空间复杂度是O(1)。
  - 递归：对于递归算法来讲，时间复杂度是O(n)，空间复杂度是O(n)。
- 看上去好像递归没有优势(dog)，实际上确实是这样的，递归不适合大逆置大规模的链表（因为可能会爆栈），但是递归的代码简洁、优美。

### 1.整体逆置链表：

- 首先讨论递归思想：

  - 先上代码：

    - ```cpp
      ListNode* reverseAll(ListNode* head)
      {
          if (head == nullptr || head->next == nullptr)
          {
              return head;
          }
          ListNode* last = reverseAll(head->next);
          head->next->next = head;
          head->next = nullptr;
          return last;
      }
      ```

  - 分析过程：

    - 一般我们用递归算法不太好画思维过程图，我们现在来具体的分析一下。
      - 整体的思想史，我递归到链表的最尾端的节点，然后利用递归逐级返回的特点进行逆置(利用递归的这一特点在处理链表问题的时候我们是不需要单独设置prev节点的)
      - 首先需要一个base case，这个base case能够告诉我们已经递归到最后一个节点了：``if (head == nullptr || head->next == nullptr)``，注意，这里多加了一条判断语句，目的是如果链表为空，我就直接不需要逆置了。
      - 找到之后把这个节点作为逆置链表的头结点： ``ListNode* last = reverseAll(head->next);``**last**代表的就是最终逆置链表的头结点，在每一级递归中，我们都创建一个last用来承接上一级递归逆置链表的头结点，到函数结束之后，我们就成功返回了这个last。
      - 在每一级递归中，当前级的head以及head->next的连接还没有改变，但是在新表中，head->next是最后一个节点，因此交换head和head->next的顺序。执行完这一次递归之后，head应该在新表的最后，head->next应该为``nullptr``。

  - 下面给出了最高两级递归的思维分析图：

    - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/逆置俩表all递归1.jpg)
    - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/逆置链表all递归2.jpg)

- 迭代思想：

  - 迭代思想其实很简单，直接用思维分析图分析即可：

    - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/义务来电说唱.jpg)
    - 上面给出了迭代的前两轮：
      - 首先，我们创建了一个``dummy``节点，这个节点的目的是用来指向逆置部分的头结点，有了dummy，我们就**不需要苦苦寻找逆置部分的头结点**了。
      - 在一开始，current指向第一个节点1，``nextNode = current->next``，然后执行：``current->next = nextNode->next``，这样就将1节点和3节点连接起来了；对于2节点，是要作为新的逆置部分的头结点，因此执行：``nextNode->next = dummy->next``；随后我们更新``dummy->next = nextNode``。
      - 第二轮和第一轮相同，但是我们要注意一点，current指向的节点是不变的：都是1，但是1的下一个节点是每一轮都在变的，这也是使用dummy的好处之一：减少改变量。

  - 上代码：

    - ```cpp
      ListNode* reverseAll(ListNode* head)
      {
          ListNode* dummy = new ListNode;
          dummy->next = head;
          ListNode* current = head;
          ListNode* nextNode = nullptr;
          
          while (current != nullptr && current->next != nullptr)//重点：当你使用了current->next，不论是在判断条件还是在哪，你都要检测current是否有为空的情况。
      //因为这一点，debug了一个小时。。。。。。
          {
              nextNode = current->next;
              current->next = nextNode->next;
              nextNode->next = dummy->next;
              dummy->next = nextNode;
          }
          return dummy->next;
      }
      ```

    - 其实循环次数就是节点数，所以可以改用for循环。

### 2.局部逆置单链表：

- 递归思想：

  - 我们首先考虑逆置单链表的前N个节点，前N个节点的区间相当于[1,N]。

  - 对于逆置[a,b]这个区间，我们可以先让头指针移动a个单位，即把第a个节点当成首节点，然后就转换成了逆置区间[1,b-a]。这样就使问题得到了化简。

  - 首先分析逆置单链表的前N个节点：

    - 首先我们要清楚，将前N个节点逆置，后面没有逆置的节点我们要取出来，因此需要设置一个``nextNode``存储没有逆置的节点。

    - 先上代码：

      - ```cpp
        ListNode* nextNode = nullptr;
        ListNode* reversePreN(ListNode* head , int n)
        {
            if (n == 1)
            {
                nextNode = head->next;
                return head;
            }
            ListNode* last = reversePreN(head->next , n - 1);
            head->next->next = head;
            head->next = nextNode;
            return last;
        }
        ```

      - 我们先来分析这段代码的base case：

        - 我们把第一个节点的索引设置成1，第二个设置成2......
        - 鉴于逆置整个链表，我们是从最后一个开始，因此逆置前N个节点，我们应该从第n个节点开始，于是先将head更新n - 1次。我们使用base case直接返回第n个节点，并且把其后面的不需要逆置的节点取出来。
        - 接着就类似于逆置整个链表的过程，注意，当前级的head不再指向NULL，而是指向``nextNode``，并且``nextNode是固定值``。

  - 接下来我们分析区间转换：

    - 上代码：

      - ```cpp
        ListNode* reverseBetween(ListNode* head , int left , int right)
        {
            if (left == 1)
                return reversePreN(head , right);
            head->next = reverseBetween(head->next , left - 1 , right - 1);
            return head;
        }
        ```

      - 我们先分析递归函数``reverseBetween(head->next , left - 1 , right - 1)``，这就是在做区间转换，把``head``这个指针定位到待逆置的区间的开始处，此时``left == 1``，很显然我们转换成了逆置前N个节点。

      - 接下来就是执行``reversePreN``这个函数，没啥好说的，注意``N = right``。

      - 重点：为什么要把返回值赋值给head->next呢？

        - 因为我们在最后一次递归的时候，head的值是``逆置区间的开端``，因此上一级递归中的head是逆置区间的前一个节点。
        - 当这个区间被逆置完了之后,正好返回给逆置区间前一个节点，为了通过递归能把每一级的``返回值串联``起来，就必须使用``head->next``。

        - 注意：逆置整个链表的时候，还有逆置前N个节点的时候，我们其实都不需要将值串联起来，因为链表是从头开始逆置的。**但是逆置一部分的时候，最开始的几个值是不会被逆置的，因此我们需要这种形式的逐级返回**。

- 迭代思想：

  - 借鉴逆置整个链表的思想，实现逆置区间的思维分析图：

    - 对于下图，我们的逆置区间为：[3,6]。
    - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/28290044-7497-4047-b3fd-8cb79794e386_1658375922.2664697.png)
    - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/786cd5ed-9358-44aa-8f7a-2165f3161765_1658373106.107245.png)
    - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/cf33f371-6877-4eee-a9ed-21f7345ce4d2_1658373524.8555613.png)
    - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/944a0961-1ae6-427d-9eff-973f95864860_1658373816.7264853.png)
    - 很明显，这些操作和我们在逆置整个单链表的时候如出一辙。
    - 和逆置整个单链表不同的是，我们需要设置一个``prevNode``来指向那个不需要逆置的部分的最后一个节点。
    - 注意，逆置整个链表是逆置区间链表的子集，因此我们还是要设置一个``dummy``节点。
    - 其实你会发现，``prevNode``和逆置整个链表中的``dummy``执行的功能是相同的。``prevNode``只不过是``移动版的dummy``。

  - 代码如下：

    - ```cpp
      ListNode* reverseBetween(ListNode* head , int left , int right)
      {
          ListNode* dummy = new ListNode;
          dummy->next = head;
          ListNode* current = nullptr;
          ListNode* nextNode = nullptr;//和forw一个意思。
          ListNode* prevNode = dummy;
          
          for (int i = 0 ; i < left - 1 ; i++)
              prevNode = prevNode->next;//目的是让prevNode指向逆置区间前一个节点。
          current = prevNode->next;
          for (int i = 0 ; i < right - left ; i++)//有right-left个节点的区间要进行逆置。
          {
              nextNode = current->next;
              current->next = nextNode->next;
              nextNode->next = prevNode->next;
              prevNode->next = nextNode;
          }  
          return dummy->next;
      }
      ```

    - 这代码和逆置整个链表如出一辙......

