## 反转数组：

- 翻转数组指的是将数组进行逆置：
- ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/fanzhuanshuzu.png)

- 该问题的解法是``左右指针``。

- 对于处理这个问题，我们不宜使用``while循环``，循环次数是能计算的：``size / 2``。

- 代码：

  - ```cpp
    void reverseString(vector<char>& s)
    {
        auto left = 0;
        auto right = s.size() - 1;
        for (auto i = 0 ; i < s.size() / 2 ; i++)
        {
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
     		left++;
            right--;
        }
        return;
    }