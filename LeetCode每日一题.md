## 2024.10.12

### 3158.求出出现两次数字的 XOR 值

给你一个数组 nums ，数组中的数字 要么 出现一次，要么 出现两次。

请你返回数组中所有出现两次数字的按位 XOR 值，如果没有数字出现过两次，返回 0 。

~~~c
int duplicateNumbersXOR(int* nums, int numsSize) 
{
    int j=0;
    int res=0;
    int buffer[51]={0};
    int repeat[51]={0};

    for(int i=0;i<numsSize;i++)
    {
        if(buffer[nums[i]]==0)
        {
            buffer[nums[i]]=1;
        }
        else
        {
            repeat[j]=nums[i];
            j++;
        }
    }

    for(int k=0;k<j;k++)
    {
        printf("%d\n",repeat[k]);
        res^=repeat[k];
    }
    return res;
    
}
~~~

1. 初始化变量：
- j 用于记录找到的重复数字的数量。
- res 用于存储最终的异或结果。
- buffer 数组用于标记哪些数字已经出现过。这里假设输入的数字范围在 [0, 50] 之间。
- repeat 数组用于存储所有的重复数字。
2. 遍历输入数组：
- 对于每个元素 nums[i]，如果它在 buffer 中的对应位置为 0，则将其标记为已出现（设置 buffer[nums[i]] 为 1）。
- 如果 buffer[nums[i]] 已经是 1，说明这个数字之前已经出现过，因此将它添加到 repeat 数组中，并增加 j 的值。
3. 处理重复数字：
- 遍历 repeat 数组，打印每个重复数字，并使用异或运算更新 res 的值。异或运算的一个重要特性是，对于任何整数 x，有 x ^ x = 0 和 x ^ 0 = x。这意味着，当两个相同的数字进行异或时，结果为 0；而任何数字与 0 进行异或时，结果仍然是该数字本身。因此，如果有偶数个相同的数字，它们会相互抵消，最终结果将是所有出现奇数次的数字的异或值。
4. 返回结果：
- 函数最后返回 res，即所有重复数字的异或结果。