# [栈的压入、弹出序列](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?tpId=13&tqId=11174&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&tab=answerKey)

## 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。

例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

```
示例1
输入
[1,2,3,4,5],[4,3,5,1,2]

返回值
false
```

## 解题思路

- 使用一个数组来模拟栈的入栈和出栈
- 迭代入栈序列
    - 每次入栈一个元素需要判断是否是出栈序列中的第一个元素
        - 如果是则出栈序号加一，否则入栈
        - 然后循环判断已入栈的栈顶元素是否是出栈序列中的第一个元素
            - 如果是则出栈序号加一
            - 否则跳出当前循环

- 最后判断出栈序号是否等于入栈序列长度
    - 如果相等则为真
    - 否则为假
