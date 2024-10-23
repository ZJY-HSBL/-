## 2024.10.12

### 3158.求出出现两次数字的 XOR 值

给你一个数组 `nums` ，数组中的数字 **要么** 出现一次，**要么** 出现两次。

请你返回数组中所有出现两次数字的按位 `XOR` 值，如果没有数字出现过两次，返回 0 。

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

## 2024.10.15
### 3200.三角形的最大高度
给你两个整数 `red` 和 `blue`，分别表示红色球和蓝色球的数量。你需要使用这些球来组成一个三角形，满足第 1 行有 1 个球，第 2 行有 2 个球，第 3 行有 3 个球，依此类推。

每一行的球必须是 **相同** 颜色，且相邻行的颜色必须 **不同**。

返回可以实现的三角形的 **最大** 高度。

~~~c
int maxHeightOfTriangle(int red, int blue) {
    ///若是第一行为红球，那么奇数行都是红色，偶数行是蓝色

    //第一行为红球
    int height1=0;
    int r1=red,b1=blue;
    for(int i=1;;i++)
    {
        if(i%2==1)
        {
            if(r1<i)
            {
                break;
            }
            r1-=i;
        }
        else
        {
            if(b1<i)
            {
                break;
            }
            b1-=i;
        }
        height1++;
    }

    //第一行为篮球
    int height2=0;
    int r2=red,b2=blue;
    for(int i=1;;i++)
    {
        if(i%2==1)
        {
            if(b2<i)
            {
                break;
            }
            b2-=i;
        }
        else
        {
            if(r2<i)
            {
                break;
            }
            r2-=i;
        }
        height2++;
    }
    return height1>height2 ? height1:height2;
}
~~~

- **变量初始化**：
  - height1和 height2分别存储当第一行为红色球和蓝色球时的最大高度。
  - r1, b1, r2, b2 是红色球和蓝色球剩余的数量，它们在每次循环中都会根据当前行的需求减少相应的数量。
- **循环结构**：
  - 每个循环都从第1行开始，逐步增加行号`i`。
  - 根据行号`i`的奇偶性决定当前行使用哪种颜色的球。
  - 如果当前行所需球的数量超过了剩余可用球的数量，则停止循环，因为无法继续构建更高的一行。
  - 每成功构建一行，对应的高度计数器（`height1`或`height2`）就增加1。
- **返回值**：
  - 函数最后比较两种情况下的高度，返回较大的那个值作为结果。

## 2024.10.21

### 910. 最小差值 II

给你一个整数数组 `nums`，和一个整数 `k` 。

对于每个下标 `i`（`0 <= i < nums.length`），将 `nums[i]` 变成 `nums[i] + k` 或 `nums[i] - k` 。

`nums` 的 **分数** 是 `nums` 中最大元素和最小元素的差值。

在更改每个下标对应的值之后，返回 `nums` 的最小 **分数** 。

~~~c
// 比较函数，用于qsort排序
int compare(const void *a, const void *b) {
     // 解引用并转换指针类型为int，然后返回两者的差值
    return (*(int *)a - *(int *)b);
}
// 计算最小范围的函数
int smallestRangeII(int* nums, int numsSize, int k) {
    // 使用qsort对数组进行升序排序
    qsort(nums, numsSize, sizeof(int), compare);

    // 初始化最小分数为排序后的最大值与最小值之差
    int minScore = nums[numsSize - 1] - nums[0];

    // 遍历数组，寻找可能的最小范围
    for (int i = 0; i < numsSize - 1; ++i) {
        // 计算当前分割点下的最大值
        int maxVal = (nums[i] + k > nums[numsSize - 1] - k) ? nums[i] + k : nums[numsSize - 1] - k;
        // 计算当前分割点下的最小值
        int minVal = (nums[0] + k < nums[i + 1] - k) ? nums[0] + k : nums[i + 1] - k;

        // 计算当前分割点下的分数
        int currentScore = maxVal - minVal;
        // 如果当前分数小于已知的最小分数，则更新最小分数
        if (currentScore < minScore) {
            minScore = currentScore;
        }
    }
    // 返回计算得到的最小分数
    return minScore;
}
~~~

## 2024.10.22

### 3184.构成整天的下标对数目I

给你一个整数数组 `hours`，表示以 **小时** 为单位的时间，返回一个整数，表示满足 `i < j` 且 `hours[i] + hours[j]` 构成 **整天** 的下标对 `i`, `j` 的数目。

**整天** 定义为时间持续时间是 24 小时的 **整数倍** 。

例如，1 天是 24 小时，2 天是 48 小时，3 天是 72 小时，以此类推。

~~~c
int countCompleteDayPairs(int* hours, int hoursSize) 
{
    int res=0;
    // 外层循环，遍历数组中的每一个元素
    for(int i=0;i<hoursSize;i++)
    {
        // 内层循环，从当前元素的下一个元素开始遍历，确保 i < j
        for(int j=i+1;j<hoursSize;++j)
        {
            // 检查当前两个元素的和是否是 24 的整数倍
            if(!((hours[i]+hours[j])%24))
            {
                // 如果是，则计数器加 1
                res++;
            }
        }
    }
    return res;
}
~~~

## 2024.10.23

### 3185.构成整天的下标数目II

给你一个整数数组 `hours`，表示以 **小时** 为单位的时间，返回一个整数，表示满足 `i < j` 且 `hours[i] + hours[j]` 构成 **整天** 的下标对 `i`, `j` 的数目。

**整天** 定义为时间持续时间是 24 小时的 **整数倍** 。

例如，1 天是 24 小时，2 天是 48 小时，3 天是 72 小时，以此类推。

~~~C
long long countCompleteDayPairs(int* hours, int hoursSize) {
    // 创建一个大小为 24 的数组来记录每个余数出现的次数
    int remainder_count[24] = {0};
    long long count = 0;

    for (int i = 0; i < hoursSize; ++i) {
        // 计算当前时间点对 24 取模后的余数
        int current_remainder = hours[i] % 24;
        // 计算需要的余数
        int needed_remainder = (24 - current_remainder) % 24;
        // 累加满足条件的对数
        count += remainder_count[needed_remainder];
        // 更新当前余数的出现次数
        remainder_count[current_remainder]++;
    }

    return count;
}

/*
超时：
int countCompleteDayPairs(int* hours, int hoursSize) 
{
    int res=0;
    // 外层循环，遍历数组中的每一个元素
    for(int i=0;i<hoursSize;i++)
    {
        // 内层循环，从当前元素的下一个元素开始遍历，确保 i < j
        for(int j=i+1;j<hoursSize;++j)
        {
            // 检查当前两个元素的和是否是 24 的整数倍
            if(!((hours[i]+hours[j])%24))
            {
                // 如果是，则计数器加 1
                res++;
            }
        }
    }
    return res;
}
*/
~~~

1. **准备一个计数数组**：

   - 准备了一个大小为24的数组 `remainder_count`，用来记录每种余数出现的次数。因为一天有24小时，所以余数的范围是0到23。

2. **初始化计数器**：

   - 用一个变量 `count` 来记录满足条件的数对的数量，初始值为0。

3. **遍历数组中的每一个数**：

   - 对于数组hours中的每一个数，做以下几件事情：

     - **计算当前数的余数**：用当前数对24取余，得到 `current_remainder`。
     - **计算需要的余数**：为了使两个数加起来能被24整除，需要找到一个数，使得这两个数的余数之和为24（或者0）。因此，需要的余数是 `(24 - current_remainder) % 24`，记为 `needed_remainder`。
     - **累加满足条件的数对**：在 `remainder_count` 中查找 `needed_remainder` 出现的次数，并将其加到 `count` 中。这表示找到了与当前数配对的所有可能的数。
     - **更新当前余数的计数**：将 `current_remainder` 在 `remainder_count` 中的计数加1，表示又遇到了一个新的具有该余数的数。

4. **返回结果**：

   - 最后，返回 `count`，即满足条件的数对的数量。