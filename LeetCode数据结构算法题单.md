# LeetCode

## 数组

### 1.两数之和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

~~~c
// 定义函数 twoSum，接受四个参数：一个整数数组 nums，数组的大小 numsSize，目标值 target，以及一个指向整数的指针 returnSize
int* twoSum(int* nums, int numsSize, int target, int* returnSize) {
    // 外层循环，遍历数组中的每一个元素
    for (int i = 0; i < numsSize; ++i) {
        // 内层循环，从外层循环的下一个元素开始，避免重复计算
        for (int j = i + 1; j < numsSize; ++j) {
            // 检查当前数对 (nums[i], nums[j]) 的和是否等于目标值
            if (nums[i] + nums[j] == target) {
                // 如果找到了满足条件的数对
                // 动态分配内存，用于存储两个索引
                int* ret = malloc(sizeof(int) * 2);
                // 将索引 i 和 j 存储到动态分配的数组中
                ret[0] = i;
                ret[1] = j;
                // 设置 returnSize 为 2，表示返回的数组包含两个元素
                *returnSize = 2;
                // 返回存储索引的数组
                return ret;
            }
        }
    }
    // 如果没有找到满足条件的数对
    // 设置 returnSize 为 0，表示没有找到任何数对
    *returnSize = 0;
    // 返回 NULL，表示没有找到任何数对
    return NULL;
}
/*
函数定义：
    int* twoSum(int* nums, int numsSize, int target, int* returnSize)：定义了一个函数 twoSum，接受四个参数：
    int* nums：指向整数数组的指针。
    int numsSize：数组的大小。
    int target：目标值。
    int* returnSize：一个指向整数的指针，用于返回结果数组的大小。
外层循环：
    for (int i = 0; i < numsSize; ++i)：遍历数组中的每一个元素。
内层循环：
    for (int j = i + 1; j < numsSize; ++j)：从外层循环的下一个元素开始，避免重复计算和自身相加的情况。
条件判断：
    if (nums[i] + nums[j] == target)：检查当前数对的和是否等于目标值。
找到数对：
    int* ret = malloc(sizeof(int) * 2)：动态分配内存，用于存储两个索引。
    ret[0] = i; ret[1] = j;：将索引 i 和 j 存储到动态分配的数组中。
*/
~~~

### 4.寻找两个正序数组的中位数

给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。

算法的时间复杂度应该为 O(log (m+n)) 。

~~~c
double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) {
    // 计算两个数组合并后的总长度
    int totalLength = nums1Size + nums2Size;
    // 计算合并后数组的中间位置
    int halfLength = totalLength / 2;

    // 初始化指向两个数组的指针
    int *nums1Ptr = nums1, *nums2Ptr = nums2;
    // 用于存储当前和前一个元素
    int last, secondLast;
    // 用于追踪当前遍历的位置
    int index = -1;

    // 循环直到遍历到中位数的位置
    while (index < halfLength) {
        // 保存当前元素为前一个元素
        secondLast = last;

        // 比较两个数组的当前元素，选择较小的元素
        if (nums1Ptr != nums1 + nums1Size && (nums2Ptr == nums2 + nums2Size || *nums1Ptr <= *nums2Ptr)) 
        {
            // 如果 nums1 还有未处理的元素，并且 nums2 已经处理完或当前元素小于等于 nums2 的当前元素
            last = *nums1Ptr++;
        } 
        else 
        {
            // 否则，选择 nums2 的当前元素
            last = *nums2Ptr++;
        }

        // 增加索引
        index++;
    }

    // 根据合并后数组的总长度是奇数还是偶数，返回相应的中位数值
    if (totalLength % 2 == 0) 
    {
        // 如果总长度是偶数，中位数是中间两个数的平均值
        return (last + secondLast) / 2.0;
    } 
    else 
    {
        // 如果总长度是奇数，中位数是中间的那个数
        return last;
    }
}
/*
初始化和计算：
    totalLength：计算两个数组合并后的总长度。
    halfLength：计算合并后数组的中间位置。
    nums1Ptr 和 nums2Ptr：初始化指向两个数组的指针。
    last 和 secondLast：用于存储当前和前一个元素。
    index：用于追踪当前遍历的位置，初始值为 -1。
遍历和合并：
    使用 while 循环遍历直到 index 达到 halfLength。
    在每次迭代中，更新 secondLast 为 last，即前一个元素。
    比较 nums1 和 nums2 的当前元素，选择较小的元素并移动相应的指针。
    增加 index，表示已经处理了一个元素。
计算中位数：
    如果 totalLength 是偶数，中位数是中间两个数的平均值，即 (last + secondLast) / 2.0。
    如果 totalLength 是奇数，中位数是中间的那个数，即 last。
*/
~~~
