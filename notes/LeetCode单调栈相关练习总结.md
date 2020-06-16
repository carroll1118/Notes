> 题目和解析持续补充中...

#  [496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Stack<Integer> stack = new Stack<>();
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i<nums2.length;i++){
            while(!stack.isEmpty() && stack.peek()<nums2[i]){
                map.put(stack.pop(),nums2[i]);
            }
            stack.push(nums2[i]);
        }
        int[] res = new int[nums1.length];
        for(int i = 0;i<nums1.length;i++){
            res[i] = map.getOrDefault(nums1[i],-1);
        }
        return res;
    }
}
```
#  [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int left = nums.length;
        int right = 0;
        Stack<Integer> stack = new Stack<>();
        for(int i = 0;i<nums.length;i++){
            while(!stack.isEmpty()&&nums[stack.peek()]>nums[i]){
                left = Math.min(left,stack.pop());
            }
            stack.push(i);
        }
        stack.clear();
        for(int i = nums.length-1;i>=0;i--){
            while(!stack.isEmpty()&&nums[stack.peek()]<nums[i]){
                right = Math.max(right,stack.pop());
            }
            stack.push(i);
        }
        return right-left>0?right-left+1:0;
    }
}
```
#  [402. 移掉K位数字](https://leetcode-cn.com/problems/remove-k-digits/)

```java
class Solution {
    public String removeKdigits(String num, int k) {
        StringBuilder res = new StringBuilder();
        int n = num.length(), m = n - k;
        for (char c : num.toCharArray()) {
            while (k != 0 && res.length() != 0 && res.charAt(res.length() - 1) > c) {
                res = res.deleteCharAt(res.length() - 1);
                --k;
            }
            res.append(c);
        }
        res = res.delete(m, res.length());
        // 去除前导0， 如10200，k = 1
        while (res.length() != 0 && res.charAt(0) == '0') {
            res = res.deleteCharAt(0);
        }
        return res.length() == 0 ? "0" : res.toString();
    }
}
```

#  [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        Stack<Integer> stack = new Stack<>();
        for(int i = 0;i<T.length;i++){
            while(!stack.isEmpty()&&T[stack.peek()]<T[i]){
                int curr = stack.pop();
                res[curr] = i - curr;
            }
            stack.push(i);
        }
        return res;
    }
}
```
#  [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int len = heights.length;
        Stack<Integer> s = new Stack<>();
        int maxArea = 0;
        for (int i = 0; i <= len; i++){
            int h = (i == len ? 0 : heights[i]);
            if (s.isEmpty() || h >= heights[s.peek()]) {
                s.push(i);
            } else {
                int tp = s.pop();
                maxArea = Math.max(maxArea, heights[tp] * (s.isEmpty() ? i : i - 1 - s.peek()));
                i--;
            }
        }
        return maxArea;
    }
}
```
#  [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

```java
class Solution {
    public int trap(int[] height) {
        Stack<Integer> stack = new Stack<>();
        int sum = 0;
        for(int i = 0;i<height.length;i++){
            while(!stack.isEmpty()&&height[stack.peek()]<height[i]){
                int currHeight = stack.pop();
                while(!stack.isEmpty()&&height[stack.peek()]==height[currHeight]){
                    stack.pop();
                }
                if(!stack.isEmpty()){
                    sum += (Math.min(height[stack.peek()],height[i]) - height[currHeight]) * (i-stack.peek()-1);
                }
            }
            stack.push(i);
        }
        return sum;
    }
}
```
**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
