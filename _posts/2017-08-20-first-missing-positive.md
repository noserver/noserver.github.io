---
layout: post
title: first missing positive
---
{{ page.title}}
===============
<p class="meta">{{ page.date }}</p>
一个leetcode题目，想出了一个比较有意思的解答，在这里整理一下思路。
题目是这样的，输入是一个整型数组，要求找出数组中第一个缺失的正整数。并要求空间复杂度为O(1),时间复杂度为O(n)。
想了一段时间没有思路，然后看了一下解答，得分最高的答案是这样的：
``` java
//原解答是c++，这里直接转成Java了
public class Solution{
    public int firstMissingPositive(int[] nums) {
        for(int i=0;i<n;i++){
            while(nums[i]>0&&nums[i]<=nums.length&&nums[nums[i]-1]!=nums[i]){
                swap(nums,i,nums[i]-1);
            }
        }
        for(int i=0;i<n;i++){
            if(nums[i]!=i+1){
                return i+1;
            }
        }
        return n+1;
    }
    public void swap(int[] arr,x,y){
        int temp = arr[x];
        arr[x] = arr[y];
        arr[y] = temp;
    }
}
```
这个解法的思路大概就是，将数组进行一遍遍历，大于0，nums.length,且和需要交换值不是同一个数值时，将nums[i]和nums[nums[i]-1]进行交换。然后进行第二遍遍历，第一个nums[i]!=i+1的值就是返回的结果。
这个解法思路比较清晰，我看了下，其他大部分解答都是按照这个思路来的，使用交换这个思路可以完美的解决空间复杂度O(1)的要求。两个不相嵌套的循环之后，时间复杂度仍然是O(n)。但是这里第一个for循环中嵌套了一个while循环，这样虽然最多还是进行n次遍历，但是实际上时间复杂度变成了O(n^2)。
其实仔细想，完全不需要进行第二个for循环，完全可以按照这个思路来，从位置0开始遍历，然后如果位置0上的值不是1(也就是i+1)的话，就将位置0上的数字进行交换，一直交换到为1为止，如果最终找不到1，则第一个缺少的数字就是1，这个过程完全可以一个循环完成。一开始也是用for里面嵌套while的方法，后来想办法把它给替换掉了。下面是具体的代码：
``` java
public class Solution {
    public int firstMissingPositive(int[] nums) {
        int temp = 0;
        int max = nums.length;//最大值
        
        for(int i=0;i<max;){
            //just check next one
            if(nums[i]==i+1){
                i++;
            }
            else if(nums[i]<=0||nums[i]>max||nums[i]<i+1||nums[i]==nums[nums[i]-1]){
                //四种可能抛弃该值
                //swap nums[i]和nums[max-1]
                temp = nums[i];
                nums[i]=nums[max-1];
                nums[max-1] = temp;
                max--;
            }
            else{
                //交换值 nums[i]和nums[nums[i]-1]
                temp = nums[i];
                nums[i] = nums[temp-1];
                nums[temp-1] = temp;
            }
        }
        //返回结果
        return max+1;
    }
}
```
这里说一下具体思路，设一个temp值用来进行交换值，设一个max量用来保存最大值，初始值为nums.length,这个最大值的含义就是当数组长度为n时，数组中连续的最大正整数是n。然后进行遍历，如果遍历的i时，nums[i]==i+1，说明i位置的数字已经正确了，不需要动了，此时i++，进行下一个遍历下一个数字。而如果i位置的数字为以下四种情况的话：1.不是正整数，2.大于最大值max，3.小于i+1，4.nums[i]==nums[nums[i]-1]。这里1，2种情况说明该值不符合条件，应该抛弃值，而第3种情况说明nums[i]的值和该位置之前的数字重复了，而之前的数字已经是排序好的了，所以此时该值也需要抛弃值，第4种情况下说明nums[i]与该值需要交换的位置的值重复了，同样是多一个值的情况，也需要进行抛弃。抛弃操作是将该值放到max-1位置上，然后将max值减一，因为此时最大值只能是max-1了。然后其他情况下进行值交换，将nums[i]交换到nums[nums[i-1]]位置上去。
这就是完整思路，按照这个思路来可以满足全部条件。