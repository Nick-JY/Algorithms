## 两数之和：

![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/title.png)

- 解决办法：
  - 问题前提：数组按照递增序列排好序，并且数组中符合条件的``元素对<=1``。
  - 解决两数之和问题的关键就是``左右指针``。
  - ``left``从数组的第一个元素开始，``right``从数组的最后一个元素开始，left和right``相向而行``，依次检测``left和right的和sum``。
  - 如果``sum < target``，我们将left向``大方向``移动。
  - 如果``sum > target``，我们将right向``小方向``移动。
  - 当``sum = target``，返回一个索引数组即可。
  - 当``left = right``，说明没找到，返回一个空索引数组即可。
  - 返回值是索引数组，使用C++写比较方便。

- ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/liangshuzhihe12131.png)

- 代码如下：

  - ```cpp
    vector<int> twoSum(vector<int>& numbers, int target)
    {
        vector<int> index;
     	auto numsSize = numbers.size();
        auto left = 0;
        auto right = numsSize - 1;
        while (left != right)
        {
            int sum = numbers[left] + numbers[right];
            if (sum < target)
                left++;
            else if (sum > target)
                right--;
            else
            {
                index.push_back(left + 1);
                index.push_back(right + 1);//注意，问题中规定数组索引从1开始。
    				return index;
            }
        }
      	return index;
    }
    ```