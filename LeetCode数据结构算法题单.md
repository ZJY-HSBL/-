# LeetCode

## 数组

### 1.两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

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
~~~

- **函数签名**:

  - `int* nums`：指向整数数组的指针。
  - `int numsSize`：数组的大小（元素个数）。
  - `int target`：要寻找的目标值。
  - `int* returnSize`：一个指向整数的指针，用于返回找到的索引对的数量（0 或 2）。

- **外层循环**:
  这个循环遍历数组中的每一个元素，变量 `i` 代表当前遍历到的元素的索引。

- **内层循环**:
  内层循环从 `i+1` 开始，这样可以确保不会重复使用同一个元素两次，并且避免了将同一对元素检查两次的情况（例如 `(nums[0], nums[1])` 和 `(nums[1], nums[0])` 是相同的数对）。

- **条件判断**:
  如果当前的两个数 `nums[i]` 和 `nums[j]` 的和等于目标值 `target`，则找到了满足条件的数对。

- **内存分配与结果返回**:
  - 使用 `malloc` 动态分配足够的内存来存储两个索引。
  - 将找到的两个索引 `i` 和 `j` 存储在新分配的数组中。
  - 设置 `*returnSize` 为 2，表示返回的结果包含两个元素。
  - 返回存储索引的数组 `ret`。

- **未找到匹配时的处理**:

  - 如果遍历完整个数组后没有找到任何满足条件的数对，则设置 `*returnSize` 为 0。
  - 函数返回 `NULL` 表示没有找到合适的数对。

### 4.寻找两个正序数组的中位数

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

~~~c
double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) 
{
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
    while (index < halfLength) 
    {
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
~~~

1. **初始化和计算**：
   - `totalLength`：计算两个数组合并后的总长度。
   - `halfLength`：计算合并后数组的中间位置。
   - `nums1Ptr` 和 `nums2Ptr`：初始化指向两个数组的指针。
   - `last` 和 `secondLast`：用于存储当前和前一个元素。
   - `index`：用于追踪当前遍历的位置，初始值为 -1。
2. **遍历和合并**：
   - 使用 `while` 循环遍历直到 `index` 达到 `halfLength`。
   - 在每次迭代中，更新 `secondLast` 为 `last`，即前一个元素。
   - 比较 `nums1` 和 `nums2` 的当前元素，选择较小的元素并移动相应的指针。
   - 增加 `index`，表示已经处理了一个元素。
3. **计算中位数**：
   - 如果 `totalLength` 是偶数，中位数是中间两个数的平均值，即 `(last + secondLast) / 2.0`。
   - 如果 `totalLength` 是奇数，中位数是中间的那个数，即 `last`。

### 11. 盛最多水的容器

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

~~~c
// 定义一个宏MIN，它接受两个参数a和b，并返回较小的那个值。
#define MIN(a, b) ((b) < (a) ? (b) : (a))

// 定义一个宏MAX，它接受两个参数a和b，并返回较大的那个值。
#define MAX(a, b) ((b) > (a) ? (b) : (a))

// 函数maxArea用于计算可以容纳的最大水量。
// 参数:
//  int* height - 指向整数数组的指针，表示一系列垂直线的高度。
//  int heightSize - 整数数组的大小。
// 返回值:
//  int - 可以容纳的最大水量（即最大矩形面积）。
int maxArea(int* height, int heightSize) {
    // 初始化变量
    // ans: 存储目前找到的最大面积
    // left: 左边界的索引，初始化为数组的第一个元素
    // right: 右边界的索引，初始化为数组的最后一个元素
    int ans = 0, left = 0, right = heightSize - 1;

    // 当左边索引小于右边索引时循环
    while (left < right) {
        // 计算当前左右边界之间的面积，宽度是right-left，高度取两边较短的那条
        int area = (right - left) * MIN(height[left], height[right]);
        
        // 更新最大面积
        ans = MAX(ans, area);

        // 根据左右边界的高度决定移动哪个指针
        // 如果左边高度小于右边高度，则左指针右移，否则右指针左移
        // 目的是寻找可能形成更大面积的新边界
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    // 返回计算出的最大面积
    return ans;
}
~~~

1. `#define MIN(a, b) ((b) < (a) ? (b) : (a))` - 定义一个宏MIN，它接受两个参数a和b，返回较小的那个值。
2. `#define MAX(a, b) ((b) > (a) ? (b) : (a))` - 定义一个宏MAX，它接受两个参数a和b，返回较大的那个值。
3. `int maxArea(int* height, int heightSize)` - 函数声明，接受一个整数数组`height`和它的大小`heightSize`作为输入参数，函数返回类型为整型，表示可以容纳的最大水量。
4. `int ans = 0, left = 0, right = heightSize - 1;` - 初始化变量：
   - `ans`用来存储目前找到的最大面积。
   - `left`指针初始化为数组的第一个元素索引（即0）。
   - `right`指针初始化为数组的最后一个元素索引（即`heightSize-1`）。
5. `while (left < right)` - 当左指针小于右指针时，执行循环体。这是双指针技术的应用。
6. `int area = (right - left) * MIN(height[left], height[right]);` - 计算当前左右指针之间形成的矩形面积。宽度是`right - left`，高度是左右两边较短的那个线段的高度。
7. `ans = MAX(ans, area);` - 更新最大面积`ans`，如果新计算出的面积`area`大于当前的最大面积`ans`，则更新`ans`。
8. `height[left] < height[right] ? left++ : right--;` - 根据左右两边的高度决定移动哪个指针：
   - 如果左边的高度小于右边的高度，则移动左指针`left`向右，试图找到更高的左边界。
   - 否则，移动右指针`right`向左，试图找到更高的右边界。
   - 这样做的原因是：移动较高的一边不会得到更大的面积，因为新的面积将受到较低边界的限制。
9. `return ans;` - 循环结束后，返回最大面积`ans`。

~~~c
int maxArea(int* height, int heightSize) 
{
    // 初始化变量来存储最大面积，初始值为0
    int max = 0;
    
    // 初始化左指针，指向数组的第一个元素
    int left = 0;
    
    // 初始化右指针，指向数组的最后一个元素
    int right = heightSize - 1;

    // 当左指针小于右指针时，继续循环
    while (left < right) 
    {
        // 计算当前左右边界的面积
        // 如果左边的高度小于右边的高度，那么容器的高度取决于左边的高度
        if (height[left] < height[right]) 
        {
            // 计算当前容器的面积：宽度 * 较短的一边（这里是左边）
            int currentArea = (right - left) * height[left];
            // 如果当前面积大于已知的最大面积，则更新最大面积
            if (currentArea > max) 
            {
                max = currentArea;
            }
            // 因为左边较矮，移动左指针以尝试找到更高的边界
            left++; // 移动左边界
        } 
        else 
        {  // 如果右边的高度小于等于左边的高度，那么容器的高度取决于右边的高度
            // 计算当前容器的面积：宽度 * 较短的一边（这里是右边）
            int currentArea = (right - left) * height[right];
            // 如果当前面积大于已知的最大面积，则更新最大面积
            if (currentArea > max) 
            {
                max = currentArea;
            }
            // 因为右边较矮或相等，移动右指针以尝试找到更高的边界
            right--; // 移动右边界
        }
    }

    // 循环结束，返回找到的最大面积
    return max;
}
~~~

算法核心思想

- 使用双指针技术，分别从数组的两端向中间逼近。
- 每次计算由左右指针所指向的两条线段形成的容器面积。
- 根据较短的那条线段来决定容器的高度，因为水的高度不会超过两边中较低的那个。
- 为了找到可能的最大面积，总是移动较短的那一边，这样有机会在下一次迭代中遇到更高的线段，从而可能得到更大的面积。
- 当左右指针相遇时，算法结束，此时的 `max` 变量就保存了最大的面积。
