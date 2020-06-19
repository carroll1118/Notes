#  二分查找框架
```java
int binarySearch(int[] nums, int target) {
    int left = 0, right = ...;

    while(...) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }
    }
    return ...;
}
```

> 分析二分查找的一个技巧是：不要出现 else，而是把所有情况用 else if 写清楚，这样可以清楚地展现所有细节

> 计算 mid 时需要防止溢出，代码中 left + (right - left) / 2 就和 (left + right) / 2 的结果相同，但是有效防止了 left 和 right 太大直接相加导致溢出。

```java
int binary_search(int[] nums, int target) {
    int left = 0, right = nums.length - 1; 
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}

int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定左侧边界
            right = mid - 1;
        }
    }
    // 最后要检查 left 越界的情况
    if (left >= nums.length || nums[left] != target)
        return -1;
    return left;
}


int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    // 最后要检查 right 越界的情况
    if (right < 0 || nums[right] != target)
        return -1;
    return right;
}
```
#  704. 二分查找
* [原题地址](https://leetcode-cn.com/problems/binary-search/)

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]<target){
                left = mid + 1;
            }else if(nums[mid]>target){
                right = mid - 1;
            }else if(nums[mid]==target){
                return mid;
            }
        }
        return -1;
    }
}
```
#  33. 搜索旋转排序数组
* [原题地址](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            int mid = left+(right-left)/2;
            if(nums[mid]==target){
                return mid;
            }

            if(nums[mid]>=nums[left]){
                if(nums[mid]>=target&&nums[left]<=target){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }else{
                if(nums[right]>=target&&nums[mid]<=target){
                    left = mid + 1;
                }else{
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```
#  81. 搜索旋转排序数组 II
* [原题地址](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]==target){
                return true;
            }
            if(nums[mid]>nums[left]){
                if(nums[mid]>=target&&nums[left]<=target){
                    right = mid -1;
                }else{
                    left = mid + 1;
                }
            }else if(nums[mid]<nums[left]){
                if(nums[mid]<=target&&nums[right]>=target){
                    left = mid + 1;
                }else{
                    right = mid - 1;
                }
            }else{
                left++;
            }
        }
        return false;
    }
}
```
#  153. 寻找旋转排序数组中的最小值
* [原题地址](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/submissions/)

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]>nums[right]){
                left = mid + 1;
            }else if(nums[mid]<nums[right]){
                right = mid;
            }else{
                right = mid - 1;
            }
        }
        return nums[left];
    }
}
```
#  154. 寻找旋转排序数组中的最小值 II
* [原题地址](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]>nums[right]){
                left = mid + 1;
            }else if(nums[mid]<nums[right]){
                right = mid;
            }else{
                right--;
            }
        }
        return nums[left];
    }
}
```
#  300. 最长上升子序列
* [原题地址](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int res = 0;
        for(int num : nums) {
            int left = 0, right = res;
            while(left < right) {
                int mid = (left + right) / 2;
                if(tails[mid] < num) left = mid + 1;
                else right = mid;
            }
            tails[left] = num;
            if(res == right) res++;
        }
        return res;
    }
}
```
#  275. H指数 II
* [原题地址](https://leetcode-cn.com/problems/h-index-ii/)

```java
class Solution {
    public int hIndex(int[] citations) {
        int len = citations.length;
        if(len==0||citations[len-1]==0){
            return 0;
        }

        int left = 0;
        int right = len-1;
        while(left<right){
            int mid = left + (right-left)/2;
            //区间 [mid, len - 1] 的长度，即 len - 1 - mid + 1 = len - mid
            if(citations[mid]<len - mid){
                left = mid + 1;
            }else{
                right = mid;
            }
        }
        return len-left;
    }
}
```
#  34. 在排序数组中查找元素的第一个和最后一个位置
* [原题地址](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)


```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int FirstPosition = findFirstPosition(nums,target);
        if(FirstPosition==-1){
            return new int[]{-1,-1};
        }
        int LastPosition = findLastPosition(nums,target);
        return new int[]{FirstPosition,LastPosition};
    }

    private int findFirstPosition(int[] nums,int target){
        int left = 0;
        int right = nums.length - 1;
        while(left<=right){
            int mid = left + (right-left)/2;
            //寻找开始位置
            if(nums[mid]>target){
                right = mid - 1;
            }else if(nums[mid]<target){
                left = mid + 1;
            }else if(nums[mid]==target){
                right = mid - 1;
            }
        }
        // 最后要检查 left 越界的情况
        if (left >= nums.length || nums[left] != target)
            return -1;
        return left;
    }

    private int findLastPosition(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else if (nums[mid] == target) {
                // 别返回，锁定右侧边界
                left = mid + 1;
            }
        }
        // 最后要检查 right 越界的情况
        if (right < 0 || nums[right] != target)
            return -1;
        return right;
    }
}
```
#  1095. 山脉数组中查找目标值
* [原题地址](https://leetcode-cn.com/problems/find-in-mountain-array/)

```java
class Solution {
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int size = mountainArr.length();
        int Mountaintop = findMountaintop(mountainArr,0,size-1);
        int res = findFromSortedArr(mountainArr,target,0,Mountaintop);
        if(res!=-1){
            return res;
        }
        res = findFromInversedArr(mountainArr,target,Mountaintop+1,size-1);
        return res;
    }

    private int findMountaintop(MountainArray mountainArr,int l,int r){
        while(l<r){
            int mid = l + (r - l)/2;
            if(mountainArr.get(mid)<mountainArr.get(mid+1)){
                l = mid+1;
            }else{
                r = mid;
            }
        }
        return l;
    }

    private int findFromSortedArr(MountainArray mountainArr,int target,int l,int r){
        while(l<r){
            int mid = l + (r-l)/2;
            if(target==mountainArr.get(mid)){
                return mid;
            }else if(mountainArr.get(mid)<target){
                l = mid + 1;
            }else{
                r = mid;
            }
        }
        if (mountainArr.get(l) == target) {
            return l;
        }
        return -1;
    }
    private int findFromInversedArr(MountainArray mountainArr,int target,int l,int r){
        while(l<r){
            int mid = l + (r-l)/2;
            if(target==mountainArr.get(mid)){
                return mid;
            }else if(mountainArr.get(mid)>target){
                l = mid + 1;
            }else{
                r = mid;
            }
        }
        if (mountainArr.get(l) == target) {
            return l;
        }
        return -1;
    }
}
```
#  4. 寻找两个有序数组的中位数
* [原题地址](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)
#  69. x 的平方根
* [原题地址](https://leetcode-cn.com/problems/sqrtx/)
```java
class Solution {
    public int mySqrt(int x) {
        long left = 0;
        // # 为了照顾到 1 把右边界设置为 x // 2 + 1
        long right = x / 2 + 1;
        while (left < right) {
            // 注意：这里一定取右中位数，如果取左中位数，代码会进入死循环
            long mid = left + (right - left + 1) / 2;
            if (mid * mid > x) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        return (int) left;
    }
}
```
#  374. 猜数字大小
* [原题地址](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

```java
class Solution extends GuessGame {
    public int guessNumber(int n) {
        int low = 1;
        int high = n;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int res = guess(mid);
            if (res == 0)
                return mid;
            else if (res < 0)
                high = mid - 1;
            else
                low = mid + 1;
        }
        return -1;
    }
}
```
**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
