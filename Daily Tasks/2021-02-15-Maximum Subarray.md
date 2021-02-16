# 53. Maximum Subarray 最大子序和

*Array, Divid and Conque分治算法, Dynamic Programming, stack*

## Description

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

## Solution

### Aproach 1: Dynamic Programming

**Thoughts**

<https://www.baeldung.com/java-maximum-subarray>

Dynamic programming solves a problem by dividing it into smaller subproblems. This is very similar to the divide-and-conquer algorithm solving technique. The major difference, however, is that dynamic programming solves a subproblem only once.

It then stores the result of this subproblem and later reuses this result to solve other related subproblems. This process is known as memoization.

The most important challenge in solving a dynamic programming problem is to find **the optimal subproblems**.

1. 假设最大子序和中子数组中最后一个元素的下标为n-1，那么：

   这个最大子序和 = 前n-1个元素中的最大子序和 + 下标为n-1的元素值
   
   前n-1个元素中的最大子序和又是由下标为n-2的元素结尾的子数组中各元素的和。
   
   
2. 依次往前类推：

   前n-1个元素中的最大子序和 = 前n-2个元素的最大子序和 + 下标为n-2的元素值
   
因此，我们提出假设：

maximumSubArraySum = max_so_far + arr[n-1]

max_so_far是以下标为n-2的元素结尾的子数组的最大子序和。

该假设可以应用到计算以任何下标结尾子数组的最大子序和。

如：maximumSubArraySum[n-2] = max_so_far[n-3] + arr[n-2]

所以，我们可以将该问题总结抽象为：maximumSubArraySum[i] = maximumSubArraySum[i-1] + arr[i]

又因为，每一个单独的元素都是数组中的长度为1的子数组，所以还需要将最大子序和与其中数组包含的单个元素作比较：

maximumSubArraySum[i] = Max(arr[i], maximumSubArraySum[i-1] + arr[i])

**Codes**

```
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0;
        int max = nums[0];
        for(int num: nums){
            pre = Math.max(num, pre + num);
            max = Math.max(max,pre);
        }
        return max;
    }
}
```
执行用时: 1 ms, 内存消耗: 38.7 MB

时间复杂度：O(n), 空间复杂度：O(1)





