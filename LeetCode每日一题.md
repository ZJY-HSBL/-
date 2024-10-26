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

## 2024.10.26

### 3181. 执行操作可获得的最大总奖励 II

给你一个整数数组 `rewardValues`，长度为 `n`，代表奖励的值。

最初，你的总奖励 `x` 为 0，所有下标都是 **未标记** 的。你可以执行以下操作 **任意次** ：

- 从区间 `[0, n - 1]` 中选择一个 **未标记** 的下标 `i`。
- 如果 `rewardValues[i]` **大于** 你当前的总奖励 `x`，则将 `rewardValues[i]` 加到 `x` 上（即 `x = x + rewardValues[i]`），并 **标记** 下标 `i`。

以整数形式返回执行最优操作能够获得的 **最大** 总奖励。

~~~c
int cmp(const void *a, const void *b)
{
    return *(int*)a > *(int*)b;
}

int maxTotalReward(int* rewardValues, int rewardValuesSize)
{
    qsort(rewardValues, rewardValuesSize, sizeof(int), cmp);
    // 计算dp数组大小并初始化
    int size = rewardValues[rewardValuesSize - 1] / 32 + 2;
    unsigned long long dp[size], temp, mask;
    memset(dp, 0, sizeof(unsigned long long) * size);
    int index, digit;
    dp[0] = 1; // 初始状态

    for (int i = 0; i < rewardValuesSize; ++i) 
    {
        // 根据当前奖励值确定在dp数组中的位置
        index = rewardValues[i] / 64;
        digit = rewardValues[i] % 64;
        // 创建掩码以处理边界情况
        mask = digit ? (1ULL << (64 - digit)) - 1 : 0;

        // 更新dp数组
        for (int j = 0; j < index; ++j)
        {
            if (digit) 
            {
                dp[j + index] |= (dp[j] & mask) << digit; // 左移并合并到新的位置
                dp[j + index + 1] |= dp[j] >> (64 - digit); // 右移并合并到下一个位置
            } 
            else 
            {
                dp[j + index] |= dp[j]; // 直接合并
            }
        }

        // 处理特殊情况
        if (digit) 
        {
            temp = dp[index] & ((1ULL << digit) - 1); // 获取当前块的低digit位
            dp[2 * index] |= (temp & mask) << digit; // 将其左移到新的块
            dp[2 * index + 1] |= temp >> (64 - digit); // 将剩余的部分右移到下一个块
        }
    }

    // 查找最大总奖励
    for (int i = size - 1; i >= 0; --i) 
    {
        if (dp[i])
            return 64 * i + 63 - __builtin_clzll(dp[i]); // 计算总奖励
    }
    return 0; // 如果没有找到有效的状态，返回0
}
~~~

**比较函数 `cmp`**

- 这个比较函数用于 `qsort` 函数来对整数数组进行排序。
- 它将指针 `a` 和 `b` 转换成 `int*` 类型，并返回一个布尔值（在 C 中用整数表示），指示第一个元素是否大于第二个元素。
- 注意这里的比较函数实际上会导致降序排序，因为当 `*(int*)a` 大于 `*(int*)b` 时，它返回一个正值。通常我们希望是升序排序，所以正确的实现应该是 `return *(int*)a - *(int*)b;` 或者 `return (*(int*)a > *(int*)b) - (*(int*)a < *(int*)b);`。

**主要函数 `maxTotalReward`**

**初始化**

- 使用 `qsort` 对 `rewardValues` 数组进行排序。
- `size` 是根据 `rewardValues` 中的最大值来决定 `dp` 数组的大小。这里假设每个 `unsigned long long` 可以表示 64 位的状态。
- `dp` 数组被初始化为全 0，除了 `dp[0]` 被设置为 1，表示初始状态下总奖励为 0 是可行的。

**动态规划更新**

- 对于 `rewardValues` 中的每一个值，根据其大小来确定它在 `dp` 数组中的影响范围。
- `index` 表示当前奖励值对应的 `dp` 数组的起始索引，`digit` 表示该值在 `dp` 数组中对应的具体位数。
- `mask` 用于确保在位操作时不会超出 64 位限制。
- 然后通过一系列的位操作更新 `dp` 数组，这包括左移、右移以及与操作。

**结果查找**

- 最后遍历 `dp` 数组，从高到低寻找第一个非零的 `dp` 值。
- 使用 `__builtin_clzll` 来计算该值的第一个非零位的位置，从而得到最大可能的总奖励。