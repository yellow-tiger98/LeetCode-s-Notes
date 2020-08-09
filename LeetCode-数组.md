# LeetCode-数组

## 1.寻找数组中心索引

给定一个整数类型的数组 nums，请编写一个能够返回数组 “中心索引” 的方法。

我们是这样定义数组 中心索引 的：数组中心索引的左侧所有元素相加的和等于右侧所有元素相加的和。

如果数组不存在中心索引，那么我们应该返回 -1。如果数组有多个中心索引，那么我们应该返回最靠近左边的那一个。

> 示例 1：
>
> 输入：
> nums = [1, 7, 3, 6, 5, 6]
> 输出：3
> 解释：
> 索引 3 (nums[3] = 6) 的左侧数之和 (1 + 7 + 3 = 11)，与右侧数之和 (5 + 6 = 11) 相等。
> 同时, 3 也是第一个符合要求的中心索引。

> 示例 2：
>
> 输入：
> nums = [1, 2, 3]
> 输出：-1
> 解释：
> 数组中不存在满足此条件的中心索引。

```java
class Solution {
    public int pivotIndex(int[] nums) {
        //用于判断有无进行初始值的改变
        boolean change = false;
        int centerNum = 0;
        //数组长度为0，不需要进行后续操作，因为不会有中心索引
        if(nums.length == 0){
            return -1;
        }
      	//数组长度为1，直接返回索引0
       	if(nums.length == 1){
         return 0;
       	}
      	//除以上两种情况，其余进行遍历
        for(int i=0;i<nums.length;i++){
          	//设置左右的元素和
            int rightSum = 0;
            int leftSum = 0;
          	//如果i为0，则左边元素和肯定为0
            if(i==0){
                leftSum = 0;
            }else{
                for(int j=i-1;j>=0;j--){
                leftSum = leftSum+nums[j]; 
            }
            }
          	//如果i为最后一个索引，则右边元素和肯定为0
            if(i==nums.length-1){
                rightSum = 0;
            }else{
                for(int z=i+1;z<=nums.length-1;z++){
                rightSum = rightSum+nums[z];
            }
            }
          	//判断左边和右边的和是否相等，若相等，则已经找出了中心索引，结束循环
            if(rightSum == leftSum){
                centerNum = i;
                change = true;
                break;
            }
        }
      	// 如果便利完没有进行过改变，则直接返回-1
        if(change == false){
            return -1;
        }
      	// 否则返回中心索引值
        return centerNum;
    }
}
```

## 2.搜索数组插入位置

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
      	//初始化索引值
        int index = 0;
      	//遍历数组
        for(int i=0;i<nums.length;i++){
          	//因为假设是有序数组，则当目标值小于或等于当前遍历元素值时，索引就是当前元素索引
            if(target<=nums[i]){
                index = i;
                break;
            }
          	//当目标值大于当前遍历元素值时，证明目标值在当前遍历元素之后，需在当前所在索引的基础上+1
            if(target>nums[i]){
                index = i+1;
            }
        }
      	//返回结果
        return index;
    }
}
```

## 3.合并区间

给出一个区间的集合，请合并所有重叠的区间。

> 示例 1:
>
> 输入: [[1,3],[2,6],[8,10],[15,18]]
> 输出: [[1,6],[8,10],[15,18]]
> 解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

> 示例2:
>
> 输入: [[1,4],[4,5]]
> 输出: [[1,5]]
> 解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。

```java
public class Test02 {
    public static  int[][] merge(int[][] intervals) {
        //如果为空或者长度只有0或1，直接返回
        if(intervals == null || intervals.length<=1)
            return intervals;
        //用于存储区间
        List<int[]> list = new ArrayList<>();
        //自定义排序，先将数组进行排序，每一组的left进行比较，小的排前面
        Arrays.sort(intervals,new Comparator<int[]>(){
            @Override
            public int compare(int[] a,int[] b){
                return a[0]-b[0];
            }
        });
        //用于遍历自增
        int i=0;
        //获取数组长度
        int n = intervals.length;
        while(i<n){
            //当前遍历元素左边值[this,nothis]
            int left = intervals[i][0];
            //当前遍历元素右边值[nothis,this]
            int right = intervals[i][1];
            //如果不是最后一个元素，且下个元素的left<=当前元素的right，则遍历找出最大的合并区间
            while(i<n-1 && right>=intervals[i+1][0]){
                // 当前元素的右边可能是最大的，且有可能不是最大的，比较得出最大右边界
                right = Math.max(right,intervals[i+1][1]);
                i++;
            }
            //加入集合中
            list.add(new int[] {left,right});
            i++;
        }
        //转换为数组
        return list.toArray(new int[list.size()][2]);
    }
}
```

