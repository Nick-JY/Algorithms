## 数组的原地修改类问题：

- 处理数组一般有两种方式：
  - 一种是new一个新的数组，然后把修改后的数据拷贝到新的数组中，这种方式比较简单，但空间复杂度为O(n)。
  - 一种是在原数组上进行修改，这就是我们要讨论的原地修改类问题，这种方式需要一定的算法，但空间复杂度是O(1)。
- 原地修改类问题汇总：
  - 第一种：删除数组中的重复元素。
    - LeetCode第26题。
  - 第二种：删除数组中的某一个特定元素。
    - LeetCode第27题。
  - 第三种："移动零"问题。
    - LeetCode第283题。

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

#### 2.删除数组中的特定元素：

- **重点理解一下后面的思维分析图**。

- 思想和上面的数组去重大同小异，我们同样采用快慢指针的方法。

- 在进行数组元素去重的时候，我们在一开始针对数组大小为0或1进行了单独的讨论，原因就是``只需要一个if语句``，会增加速度；但是对于删除特定元素来讲，0和1要分开进行讨论，这样就比较麻烦，因此我们不将这两个单独拎出来分析，``直接放在循环里``也是可以的。

  - 既然这样，我们在一开始就要设定``slow = fast = 0；``，随后我们针对fast所指向的值是否是val进行判断：
    - 如果是的话，很显然我们需要将fast++，继续判断。
    - 如果不是的话，说明此时fast的元素是不需要删除的：
      - 注意，这里我们``先把fast的值赋给slow，然后再对slow++、fast++``。
      - 这样做的目的是，如果数组中的第一个元素需要删除，那么这样做能把第一个元素删除。
  - 我们不要怕slow指针会移动到fast前面，因为如果数组中的元素都不需要删除的话，slow永远和fast指向同一个位置；只要有一个是需要删除的，fast就会在slow前面。
  - **重点**：由于先对slow赋值，在更新slow，因此最终的数组是[0,slow - 1]这个区间。
    - 我们可以根据：如果数组中的元素都不需要删除的话，``slow永远和fast指向同一个位置``，到最后slow和fast等于``numsSize``，因此区间必须是[0,slow - 1]。

- 思维分析图实例：假设待删除元素是2。

  - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/原地算法之原地删除数组指定元素123.png)
  - 经过一系列的删除操作后，[0,slow - 1]这个区间就是我们最终需要的新数组。

- ``C语言代码如下``：

  - ```cpp
    int removeElement(int* nums, int numsSize, int val)
    {
        int slow = 0;
        int fast = 0;
        while (fast != numsSize)
        {
            if (nums[fast] != val)
            {
                nums[slow] = nums[fast];
                slow++;
            }
            fast++;
        }
        return slow;//由于新数组的区间是[0,slow - 1]，因此长度是slow。
    }
    ```

#### 3.移动零问题：

- 什么是移动零问题：
  - 移动零问题指的是，对于一个数组来讲，我们把所有的零全都移动到数组的最后。
  
  - 具体的思路：我们可以利用``原地删除数组特定元素`` + ``修改之后的数组的最后都赋值为0``。
  
  - 代码如下：
  
    - ```c
      void moveZeroes(int* nums, int numsSize)
      {
          int slow = 0;
          int fast = 0;
          while (fast != numsSize)
          {
              if (nums[fast] != 0)
              {
                  nums[slow] = nums[fast];
                  slow++;
              }
              fast++;
          }
          while (slow != fast)
              nums[slow++] = 0;
          return;
      }
      ```
  
      
