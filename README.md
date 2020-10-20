# 2020.10.20————四数之和
## 题目
给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：

答案中不可以包含重复的四元组。

示例：
```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
````
## 解题
### 语言
java
### 思路
本题与15题：三数之和  很相似。
此方法官方解析很繁琐，我在这里长话短说。
- 两重循环加双指针，两重循环枚举前两个数，双指针指向后两个数

- 每一种循环枚举到的下标必须大于上一重循环枚举到的下标；

- 同一重循环中，如果当前元素与上一个元素相同，则跳过当前元素。

- 假设两重循环枚举到的前两个数分别位于下标i和j，其中 i<j。初始时，左右指针分别指向下标j+1和下标n-1。

> 如果和等于target，则将枚举到的四个数加到答案中，然后将左指针右移直到遇到不同的数，将右指针左移直到遇到不同的数；

> 如果和小于 target，则将左指针右移一位；

> 如果和大于 target，则将右指针左移一位。

- 剪枝：

> 1.在确定第一个数之后，如果`nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target`，说明此时剩下的三个数无论取什么值，四数之和一定大于`target`，因此退出第一重循环；

> 2.在确定第一个数之后，如果 `nums[i]+nums[length-3]+nums[length-2]+nums[length-1]<target`，说明此时剩下的三个数无论取什么值，四数之和一定小于`target`，因此第一重循环直接进入下一轮，枚举nums[i+1]；

> 3.在确定前两个数之后，如果`nums[i]+nums[j]+nums[j+1]+nums[j+2]>target`，说明此时剩下的两个数无论取什么值，四数之和一定大于`target`，因此退出第二重循环；

> 4.在确定前两个数之后，如果`nums[i]+nums[j]+nums[length-2]+nums[length-1]<target`，说明此时剩下的两个数无论取什么值，四数之和一定小于`target`，因此第二重循环直接进入下一轮，枚举 nums[j+1]。

### 代码
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> quadruplets = new ArrayList<List<Integer>>();//创建列表的列表，保存结果信息
        if(nums==null||nums.length<4){
            return quadruplets;//如果输入的值不大于四个就直接返回空值
        }
        Arrays.sort(nums);//给输入的列表排序
        int length = nums.length;
        for(int i=0;i<length-3;i++){//设置第一重循环，length-3是因为后面要保留第三个和第四个数字的位置
            if(i>0&&nums[i]==nums[i-1]){
                continue;//如果有重复数字就进行下个数字的遍历
            }
            if(nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target){
                break;//解析中的第一个剪枝，直接跳出第一个循环
            }
            if(nums[i]+nums[length-3]+nums[length-2]+nums[length-1]<target){
                continue;//解析中的第二个剪枝，直接进行下次循环
            }
            for(int j = i+1;j<length-2;j++){
                if(j>i+1&&nums[j]==nums[j-1]){
                    continue;//如果有重复字母，就执行下次循环
                }
                if(nums[i]+nums[j]+nums[j+1]+nums[j+2]>target){
                    break;//解析中的第三个剪枝，直接跳出循环
                }
                if(nums[i]+nums[j]+nums[length-2]+nums[length-1]<target){
                    continue;//解析中的第四个剪枝，进行下一次循环  
                }
                int left = j+1,right = length-1;
                while(left<right){
                    int sum = nums[i]+nums[j]+nums[left]+nums[right];
                    if(sum==target){
                        quadruplets.add(Arrays.asList(nums[i],nums[j],nums[left],nums[right]));
                        //将正确的结果填入结果集中
                        while(left<right&&nums[left]==nums[left+1]){
                            left++;
                        }
                        left++;//左指针右移到不重复的数字
                        while(left<right&&nums[right]==nums[right-1]){
                            right--;
                        }
                        right--;//右指针左移到不重复的数字
                    }else if(sum<target){
                        left++;//遍历寻找
                    }else{
                        right--;
                    }
                }
            }
        }
        return quadruplets;
    }
}
```
