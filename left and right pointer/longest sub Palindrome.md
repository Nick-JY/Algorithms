## 最长回文子串

![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/zuichanghuiwenzichuantitle.png)

- 解决思路：

  - 在这里我们使用双指针的思路：
    - 和其他问题不一样的是，这道问题的双指针不是从两端向中间逼近，而是从中间向两端逼近。
    - 这样的思想我们称之为"双指针——中心扩散"。
  - 对于``回文字符串``来讲，既可以从两端向中间比较，也可以从中间向两边比较，两者的比较结果都能说明这是一个回文字符串。
  - 但是如果我们要在一个大字符串中寻找其回文子串，从两边向中间比较就无法清晰的知道``left``和``right``指针具体的移动方式。因此我们选择从中间向两端进行检查(本质上来讲，这是从**小规模到大规模**的比较方式)。
    - 但这又有一个问题，对于奇数个字符的回文串来讲，中间只有一个元素；而对于偶数个字符的回文串来讲，中间有两个元素。
    - 因此我们在比较的时候，这两种情况都要考虑到。

- 思维分析图：

  - ![](https://nickaljy-pictures.oss-cn-hangzhou.aliyuncs.com/img/zuichanghuiwenzichuanpanduan.png)
  - 我们最终以每个元素都作为中心节点进行判断，比较之后就能得到最长回文子串。

- 代码如下：

  - ```cpp
    string longestPalindrome(string s)
    {
        string res = "";
        for (int i = 0 ; i < s.size() ; i++)//对字符串中的每一个字符进行遍历
        {
            string s1 = searchPalindrome(s , i , i);//以i作为中心节点，回文串是奇数
            string s2 = searchPalindrome(s , i , i + 1);//以i和i+1作为中心节点，回文串是偶数
            res = longestString(res , s1 , s2);//在每一次循环中，选出一个最长的回文串
        }
        return res;
    }
    string searchPalindrome(string s , int left , int right)
    {
        while (left >= 0 && right < s.size() && s[left] == s[right])
        {//这个判断条件至关重要，left>=0和right<s.size()限制了子串不会越界。
            left--;
            right++;
        } 
        return s.substr(left+1 , right - left - 1);
        //使用string中的子串返回函数，判断结束时候，left+1是子串的开始位置，right - 1是子串的结束位置。因此子串的长度是right - left - 1。
    }
    string longestString(string s1 , string s2 , string s3)//选择出三个字符串中的最长串，并返回。
    {
        string longest = s1.size() > s2.size() ? s1 : s2;
        longest = longest.size() > s3.size() ? longest : s3;
        
        return longest;
    }
    ```