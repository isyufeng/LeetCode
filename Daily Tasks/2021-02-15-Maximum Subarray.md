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


### Approach 2 Naive Method

TO run two loops. The outer loop picks the beginning element, the inner loop finds the maximum possible sum with first element picked by outer loop and compares this maximum with the overall maximum. Finally return the overall maximum. The time complexity of the Naive method is O(n^2).

### Approach 3 Divid and Conque

**Thoughts**

分治法的思想将所给序列逐步分解，分解到不能分解为止（即递归的基准情形），然后再逐步回退，分别求各个分解的子序列的最大子序列和。

1. 找出左侧子数组中的最大子序数和
2. 找出右侧子数组中的最大子序数和
3. 找出横跨左右两侧子数组的子序数和：这个值由包含左侧的最后一个元素的最大子序列和以及包含右侧第一个元素的最大子序列和二者求和所得到。

> Using Divide and Conquer approach, we can find the maximum subarray sum in O(nLogn) time. Following is the Divide and Conquer algorithm:
   
   > 1.Divide the given array in two halves. 
   
   > 2.Return the maximum of following three:
  
     Maximum subarray sum in left half (Make a recursive call) 

     Maximum subarray sum in right half (Make a recursive call) 

     Maximum subarray sum such that the subarray crosses the midpoint
      
> The lines 2.a and 2.b are simple recursive calls. How to find maximum subarray sum such that the subarray crosses the midpoint? We can easily find the crossing sum in linear time. The idea is simple, find the maximum sum starting from mid point and ending at some point on left of mid, then find the maximum sum starting from mid + 1 and ending with sum point on right of mid + 1. Finally, combine the two and return. 
> 

```
class Solution {
    public int maxSubArray(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        return maxSubSum(nums, left, right);
    }

    public int maxSubSum(int[] nums, int left, int right) {
        if (left == right){
            return nums[left];
        }

        int middle = (left + right)/2;
        // 计算左侧和右侧的最大子序数和 -- 递归思想
        int maxLeftSubSum = maxSubSum(nums, left, middle);
        int maxRightSubSum = maxSubSum(nums, middle + 1, right);
        
        // 计算左侧包含最后一个元素的最大子序数和
        int maxLeftSum = nums[middle];
        int leftSum = nums[middle];
        for(int i = middle-1; i>=left; i--){
            leftSum += nums[i];
            if (leftSum > maxLeftSum){
                maxLeftSum = leftSum;
            }
        }

        // 计算左侧包含最后第一个元素的最大子序数和
        int maxRightSum = nums[middle+1];
        int rightSum = nums[middle+1];
        for(int i = middle+2; i<=right; i++){
            rightSum += nums[i];
            if(rightSum > maxRightSum){
                maxRightSum = rightSum;
            }
        }

        return Math.max(Math.max(maxLeftSubSum, maxRightSubSum), maxLeftSum + maxRightSum);
    }

}
```
执行用时: 1 ms, 内存消耗: 38.6 MB

Reference：

<https://www.geeksforgeeks.org/maximum-subarray-sum-using-divide-and-conquer-algorithm/>
<https://blog.csdn.net/abnerwang2014/article/details/36027747>

