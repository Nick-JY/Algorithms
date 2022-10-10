## 数组的原地修改类问题：

- 处理数组一般有两种方式：
  - 一种是new一个新的数组，然后把修改后的数据拷贝到新的数组中，这种方式比较简单，但空间复杂度为O(n)。
  - 一种是在原数组上进行修改，这就是我们要讨论的原地修改类问题，这种方式需要一定的算法，但空间复杂度是O(1)。
- 原地修改类问题汇总：
  - 第一种：删除数组中的重复元素。
    - LeetCode第26题。

#### 1.删除数组中的重复元素：

- 该类问题使用的算法是双指针中的快慢指针。

  - 创建一个快指针和慢指针(对于数组来讲，指针不需要是真正的指针类型，整数索引即可)。
  - 快指针在慢指针前面进行查重操作，如果快指针指向的值和慢指针的相同，则继续移动快指针；否则将慢指针前进一个位置，将快指针的值赋值给慢指针。
  - 经过上面的操作，就可以跳过数组中的重复元素。
  - 注意：该函数的返回值是新的数组的大小K。
  - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/拉不拉东的数组删除重复元素快慢指针.gif)
    - 上图来源labuladong的公众号。

- ``C语言代码如下``：

  - ```c
    int removeDuplicates(int* nums , int numsSize)
    {
        if (numsSize == 0 || numsSize == 1)
            return numsSize;
        //上面的判断条件其实很重要，因为我们打算让一开始fast指针就在slow指针的前面，所以这两种情况需要避免，一种是数组为空；另外一种是数组大小为1，当数组大小为1的时候，fast无法在slow前面。因此这两种情况我们要在一开始避免。
        int slow = 0;
        int fast = 1;
        while (fast != numsSize)//由于我们需要保证fast一直在slow前面，因此fast到达数组末尾即可。
        {
            if (nums[slow] == nums[fast])
                fast++;//跳过重复元素(注意，由于fast天生大于slow，因此nums[slow] == nums[fast]是没问题的)
            else
                nums[++slow] = nums[fast++]; //否则，将slow前进一个位置，然后把fast的值赋值给slow的位置，然后更新fast。
            //上述的if——else语句，无论执行哪一条语句，fast都会更新一次，因此fast恒在slow前面。
        }
        return slow + 1;//区间[0,slow]就是新的数组，但是数组大小要加1。
    }
    ```

- ``C++代码如下``：

  - ```cpp
    int removeDuplicates(vector<int>& nums)
    {
        auto numsSize = nums.size();
        if (numsSize == 0 || numsSize == 1)
            return numsSize;
        
        auto iter_slow = nums.begin();
        auto iter_fast = nums.begin() + 1;
        while (iter_fast != nums.end())
        {
            if (*iter_slow == *iter_fast)
                iter_fast++;
            else
            {
                iter_slow++;
                *iter_slow = *iter_fast;
                iter_fast++;
            }
        }
        return (iter_slow - nums.begin() + 1);
    }
    ```
    
  - 上述``C++``代码的思想和``C语言``一模一样，唯一不同点在于C++使用了``vector模板类``以及``迭代器``这两个特性。