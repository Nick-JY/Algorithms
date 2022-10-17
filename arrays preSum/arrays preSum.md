## 数组前缀和

### 1.一维数组的前缀和：

- 什么是前缀和？

  - 在一个数组中，前N个元素的和就叫前缀和。

- 有人可能会问，那我直接用一个for循环不就能求前N个元素的和了吗，为什么还要引出前缀和这一概念？

  - 如果第一次求前100个元素的和，我调用一次求和函数，不一会又要求前200个元素的和，又调用一次求和函数。
  - 虽然一个函数的时间复杂度是O(n)，但是多次调用，时间复杂度也上去了。
  - 前缀和就是解决这个问题的，前缀和让求数组前N个元素的时间复杂度变为O(1)。
  - 但是使用前缀和的前提是有前缀和数组，求一个数组的前缀和数组的时间复杂度是O(n)。

- 思维分析图：

  - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/presumarray.jpeg)
    - 该图出自labuladong公众号。
  - 对于上面的例子，我们观察preSum数组：
    - 1.数组在开头多了一个元素，这样可以使preSum[N]对应前N个元素的和。
    - 2.我们如何求preSum呢：
      - preSum数组的第一个元素已经固定为0了，因此我们需要从索引为1的地方开始。
      - ``preSum[i] = nums[i - 1] + preSum[i - 1]``，这个公式的含义是，``前i个元素的和 = 第i个元素 + 前i - 1个元素的和``，由于nums是从0索引开始的，因此nums[i - 1]表示的是第i个元素。
      - 当i等于1的时候由于preSum数组有固定元素0，所以preSum[1]能准确的求出来。

- 子数列问题：

  - **给你一个整数数组 nums 和一个整数 k 。你需要找到 nums 中长度为 k 的``子数列`` ，且这个子数列的和最大 。请你返回任意一个长度为k的整数子数列。**

  - **其中 k >= 1 ， nums.length >= 1。**

  - **事实上，LeetCode上并没有这个题......**

    - 注意区分子数列和子序列的区别，数列要求是数组中连续的一串数据，而序列要求是数组中的元素即可。

    我们如何利用前缀和数组求数组中某一区间的长度呢？

    - 拿上面的思维分析图为例，我想求**nums[1] + nums[2] + nums[3]**，sum = 5 + 2 - 2 = 5，我们可以转换成前四个元素的和:``nums[0] + nums[1] + nums[2] + nums[3]``减去前一个元素的和:``nums[0]``。即``preSum[4] - preSum[1]``。
    - 我们把子数列转换成数组中某一区间的长度，子数列的长度是多少，区间的长度就是多少。
    - 我们假设子数列的长度是K，那么第一个子数列的和一定是preSum[K] - preSum[0]，就是前K个元素的和；第二个子数列的和一定是preSum[K + 1] - preSum[1]......
    - 因此我们可以使用快慢指针，当快指针到达preSum数组的最后一个元素的时候，结束。

  - 代码如下：

    - ```cpp
      vector<int> maxSubsequence(vector<int>& nums, int k)
      {
          int* preSum = new int[nums.size() + 1];
          preSum[0] = 0;
          for (int i = 1 ; i <= nums.size() ; i++)
      		preSum[i] = preSum[i - 1] + nums[i - 1];
          //上述过程求出前缀和数组。
          int slow = 0;
          int fast = k;//创建快慢指针用于按顺序求子数列的和。
          int sum_max = 0;//记录最大的子数列的和。
          int tag_slow = slow;
          int tag_fast = fast;//用于当子数列的和最大的时候的slow和fast，用于返回子数列。
         	while (slow < fast && fast <= nums.size())//nums.size()的值就是preSum数组最后一个元素的索引。
          {
              int sum = preSum[fast] - preSum[slow];
              if (sum > sum_max)
              {
                  sum_max = sum;
                  tag_fast = fast;
                  tag_slow = slow;
              }
              slow++;
              fast++;
          }
          vector<int> res;
          for (int i = tag_slow ; i < tag_fast ; i++)//注意，tag_slow的值是子数列开始的索引，tag_fast - 1是子数列结束的索引。
              res.push_back(nums[i]);
          return res;
      }