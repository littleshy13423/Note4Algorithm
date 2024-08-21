# topK 问题

## 整体思路

### 问题描述：

给定一个未排序的数组，要求找出其中的前k个最大元素。

### 快速选择的核心思想：

快速选择算法类似于快速排序，但它只对数组的一部分进行排序。
通过选择一个“枢轴”（pivot），将数组划分为两部分：一部分元素比枢轴小，另一部分元素比枢轴大。根据k的值决定是否在左部分、右部分继续递归查找，或者直接返回结果。

### 步骤：

选择一个枢轴元素，将数组划分为两部分。

判断枢轴的位置与k的关系：
如果枢轴位置正好是第k大元素的位置，则直接返回前k个元素。

如果枢轴位置小于k，则在右部分递归查找。

如果枢轴位置大于k，则在左部分递归查找。

```C++ 
#include <iostream>
#include <vector>
#include <algorithm>

int partition(std::vector<int>& nums, int low, int high) {
    int pivot = nums[high];
    int i = low;
    for (int j = low; j < high; ++j) {
        if (nums[j] > pivot) {
            std::swap(nums[i], nums[j]);
            ++i;
        }
    }
    std::swap(nums[i], nums[high]);
    return i;
}

int quickSelect(std::vector<int>& nums, int low, int high, int k) {
    if (low == high) {
        return nums[low];
    }
    int pivotIndex = partition(nums, low, high);
    int count = pivotIndex - low + 1;

    if (count == k) {
        return nums[pivotIndex];
    } else if (count > k) {
        return quickSelect(nums, low, pivotIndex - 1, k);
    } else {
        return quickSelect(nums, pivotIndex + 1, high, k - count);
    }
}

std::vector<int> topKElements(std::vector<int>& nums, int k) {
    int n = nums.size();
    quickSelect(nums, 0, n - 1, k);
    std::vector<int> result(nums.begin(), nums.begin() + k);
    return result;
}

int main() {
    std::vector<int> nums = {3, 2, 1, 5, 6, 4};
    int k = 3;
    std::vector<int> result = topKElements(nums, k);

    std::cout << "Top " << k << " elements are: ";
    for (int num : result) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}

```