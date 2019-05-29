## 数组中重复的数字

<div class="note info">题目描述：在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。 </div>

### 解题思路

数组类题目解题往往是存在直观解法的，因为不管是一维数组还是二维数组，都可以直接通过双重`for`循环之类方法进行直接求解，但是这种解法的时间复杂度都是很高的，为此，在算法类题目中涉及到数组，往往不会使用多重循环求解。

### 代码实现

但是在一步步求解过程中，还是需要思路循序渐进，因此还是从最直观的解法入手一步步进行优化。

上述题目最直观的解法如下：

```java
public static boolean duplicate(int[] numbers, int length, int[] duplication) {
    for (int i = 0; i < length; i++) {
        for (int j = i + 1; j < length; j++) {
           if(numbers[i] == numbers[j]){
               duplication[0] = numbers[i];
               return true;
            }
         }
    }
  return false;
}
```

一个双重`for`循环可以直接搞定，但是问题来了，上面解法时间复杂度显然为`O(n^2)`，这种复杂度对于海量数据求解来说，是很不可取的。

那么我们可以再次进行优化一下，我们可以对数组进行排序，这样算法复杂度会降低，于是可以以下面代码实现：

```java
public static boolean duplicate(int[] numbers, int length, int[] duplication) {
    ArrayList<Integer> list = new ArrayList<>();
    for(int i = 0 ; i < length ; i++){
        list.add(numbers[i]);
    }
    Collections.sort(list); //时间复杂度为O(NlogN)
    for(int i = 0 ; i < list.size() ; i++){
        if(i+1 < list.size()){
            if(list.get(i) == list.get(i+1)){
               duplication[0] = list.get(i);
               return true;
             }
         }
     }
    return false;
}
```

但是上述还不是最优解，假如我们要求`时间复杂度为O(n)，空间复杂度为O(1)`，在这个前提下，我们需要再寻求一种最优解。

那么我们需要注意到题目中有一个条件是<font color="red">长度为n的数组里面所有数字都在0~n-1范围内</font>，这个条件肯定不是白给的，上面两种解题思路都没有用到这个条件，那么这个条件肯定就是解题的关键。

根据上面思路，如果这个数组中没有重复的数字，那么当数组排序后，数字`i`将出现在下标为`i`的位置，但是由于数组中一定有重复的数字，那么有些位置可能存在多个数字，同时有些位置可能没有数字。

比如对于数组`[2,3,1,0,2,5,3]`

1. 首先从头到尾扫描这个数组，当扫描到下标为`i` 的数字时，比较这个数字（比如为`m`）是不是等于`i`。
2. 如果是，那么接着扫描下一个数字
3. 如果不是，那么拿这个数字跟第`m`个数字进行比较。
4. 如果这个数字跟第`m`个数字相等，那么这个数字就是其中一个重复数字，结束寻找
5. 如果不等，将把第`i`个数字和第`m`个数字交换，即把`m`放到属于它的位置
6. 重复上述过程，直到发现一个重复的数字。

根据上述思路，我们可以写出最终优化的代码如下：

```java
 	public static boolean duplicate(int[] numbers,int length,int[] duplication) {
        //考虑边界条件
        if(numbers == null || length <= 0){
            return false;
        }
        for (int i = 0; i < length-1; i++){
            while (numbers[i] != i){
                if(numbers[i] == numbers[numbers[i]]){
                    duplication[0] = numbers[i];
                    return true;
                }
                swap(numbers,i,numbers[i]);
            }
        }
        return false;
    }

    public static void swap(int[] nums,int i,int j){
        int temp = nums[i];
        nums[j] = nums[i];
        nums[i] = temp;
    }
```

## 二维数组的查找

<div class="note info">题目描述：在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。 </div>

### 解题思路

那么显然同上一题，这个题还是可以通过双重`for`循环直接解决，并且代码十分简洁。但是还是那个问题，最直观的解法往往不是最优解，题目给了那么多条件，仅仅通过一个双重`for`循环完成，时间复杂度是非常高的。

### 代码实现

根据题目条件，实际上不难想出另外一种解法：

比如对于如下二维数组，我们要查找数字`7`：

```java
1  2  8  9
2  4  9  12
4  7  10 13
6  8  11 15
```

由于数组从左到右递增，从上到下递增，我们可以试图先选一个边角的值，比如右上角的`9`，这个值比它左边的值都要大，比它下面的值都要小。

那么这个值比要找的`7`要大，说明`7`不可能在`9`这一列了，于是我们把这一列去掉。

```java
1  2  8
2  4  9
4  7  10
6  8  11
```

继续选取右上角的值，我们发现`8`还是比`7`要大，于是`8`这一列的值也可以删掉了。

```java
1  2
2  4
4  7
6  8
```

然后选取右上角的值`2`，由于`2`比`7`小，那么说明`2`所在的一行不可能包括`7`，那么把这一行删掉。

```java
2  4
4  7
6  8
```

由于右上角的`4`比`7`要小，那么同样可以删掉这一行。

```java
4  7
6  8
```

最终右上角的值就是我们要找的值，那么这种办法实际上避免了双重循环的冗余度，可以很快的找到是否包括需要查找的值。并且在数据量越大的时候，越能体会到这种算法的时间优越性。这就是算法的实用性所在。

基于上述思路，代码实现可以如下：

```java
    public boolean Find(int target, int [][] array) {
        boolean flag = false;
        if(array != null && array.length > 0 && array[0].length > 0){
            int row = 0 ;
            int columns = array[0].length;
            int column =  columns - 1;
            while(row < array.length && column >= 0){
                if(array[row][column] == target){
                    flag = true;
                    break;
                }else if(array[row][column] > target){
                    //如果右上角元素的值比目标值要大，就删除最后一列
                    column--;
                }else{
                    //如果右上角元素的值比目标值要小，就删除最上面一行
                    row++;
                }
            }
        }
        return flag;
    }
```

## 构建乘积数组

<div class="note info">题目描述：给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。 </div>

### 解题思路

如果没有不能使用除法的限制，那么这个题目可以直接使用公式解出来，即将`(A[0]*A[1]*...*A[n-1])/A[i]`即可得到结果。

既然不能用除法，那么只能寻找其他思路。

1. 直接按题目要求连乘，显然时间复杂度为O(n^2)
2. 有吗？

当然有，我们基于题目算法可以构建矩阵如下：

![](https://blogimage-1258928558.cos.ap-guangzhou.myqcloud.com/offer/array-multi.png)

**其中B[i]的值可以看作下图的矩阵中每行的乘积。** 

下三角用连乘可以很容求得，上三角，从下向上也是连乘。 

 因此我们的思路就很清晰了，先算下三角中的连乘，即我们先算出B[i]中的一部分，然后倒过来按上三角中的分布规律，把另一部分也乘进去。

### 代码实现

基于上述思路，代码实现如下：

```java
public class Solution {
    public int[] multiply(int[] A) {
        int length = A.length;
        int[] B = new int[length];
        if(length != 0 ){
            B[0] = 1;
            //计算下三角连乘
            for(int i = 1; i < length; i++){
                B[i] = B[i-1] * A[i-1];
            }
            int temp = 1;
            //计算上三角
            for(int j = length-2; j >= 0; j--){
                temp *= A[j+1];
                B[j] *= temp;
            }
        }
        return B;
    }
}
```
