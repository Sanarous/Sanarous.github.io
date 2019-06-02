## 连续子数组的最大和

### 题目描述

HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1) 

### 解题思路

第一次看到这个题的时候，最直观的思路应该是想办法枚举所有子数组并求其和，但是一个长度为`n`的数组，枚举的子数组共有`n(n+1)/2`个，因此最快也需要`O(n^2)`时间。在算法题目中，凡是面对这种复杂度的解法，往往是不考虑这种解法的。

以下解法均以数组`aray = {1，-2，3，10，-4，7，2，-5}`为例，该数组的最大和数组为`{3,10,-4,7,2}`，最大和为`18`。

### 解法一：分析数组的规律

可以先试着从头到尾累加数组中的每个数字。初始化和为0，第一次加上1，此时和为1。第二步加上-2，和变成了-1，如果再加上第三个数字3，我们注意到此前累加的和是-1，加上3后和为2，累加和的结果比3本身还要小。也就是说，从第一个数字开始的子数组的和比从第三个数字开始的子数组的和要小。因此，我们完全没有必要考虑从第一个数字开始的子数组，并且之前累加的和也可以抛弃。

然后我们从第三个数字开始累加，此时的和为3，第四步加10后和为13。第五步加上-4后和为9，这时我们发现由于-4是一个负数，那么累加之后的结果比原来的和还要小，因此我们需要把之前得到的累加和13保存下来，因为它有可能是最大子数组的和。然后第六步加上数字7得到和为16比保存的13要大，因此把最大子数组的和从13更新为16，然后再加2得到和为18，那么继续更新最大子数组的和为18。依此类推，最终结果为18。

那么我们用表总结一下上述过程：

| 步骤 | 操作                | 累加的子数组和 | 最大的子数组和 |
| ---- | ------------------- | -------------- | -------------- |
| 1    | 加1                 | 1              | 1              |
| 2    | 加-2                | -1             | 1              |
| 3    | 抛弃前面的和-1，加3 | 3              | 3              |
| 4    | 加10                | 13             | 13             |
| 5    | 加-4                | 9              | 13             |
| 6    | 加7                 | 16             | 16             |
| 7    | 加2                 | 18             | 18             |
| 8    | 加-5                | 13             | 18             |

### 代码实现

```java
    public int FindGreatestSumOfSubArray(int[] array) {
        if (array.length == 0 || array == null) {
            return 0;
        }
        int curSum = 0;
        //设置一个32位int型整数的最小值(-2)^32，保证第一次一定可以更新最大值
        int greatestSum = 0x80000000;
        for (int i = 0; i < array.length; i++) {
            //如果加的值小于0，保持之前的最大值
            if (curSum <= 0) {
                curSum = array[i]; //记录当前最大值
            } else {
                 //当array[i]为正数时，加上之前的最大值并更新最大值。
                curSum += array[i];
            }
            //更新最大值
            if (curSum > greatestSum) {
                greatestSum = curSum;
            }
        }
        return greatestSum;
    }
```

### 解法二：动态规划

这个题显然还可以使用DP来做，但是DP的状态转移方程比较难想到。

`f(i)`：以`array[i]`为末尾元素的子数组的和的最大值，子数组的元素的相对位置不变 。

那么可以列出转移方程：`f(i) = max( f(i-1)+array[i] ，array[i] ) `

`res`：所有子数组的和的最大值 ，那么`res=max(res, f(i)) `。

比如对于`aray = {1，-2，3，10，-4，7，2，-5}`：

1. 初始状态：`f(0)=1`，`res=1`。
2. 当`i=1`时，`f(1)=max(f(0)-2,-2)=-1`，那么`res=max(1,-2)=1`。
3. 当`i=2`时，`f(2)=max(f(1)+3,3)=3`，那么`res=max(1,3)=3`。
4. 当`i=3`时，`f(3)=max(f(2)+10,10)=13`，那么`res=max(3,13)=13`。
5. 当`i=4`时，`f(4)=max(f(3)-4,-4)=9`，那么`res=max(13,9)=13`。
6. 当`i=5`时，`f(5)=max(f(4)+7,7)=16`，那么`res=max(16,13)=16`。
7. 当`i=6`时，`f(6)=max(f(5)+2,2)=18`，那么`res=max(18,16)=18`。
8. 当`i=7`时，`f(7)=max(f(6)-5,-5)=13`，那么`res=max(13,18)=18`。

因此最终结果为18。

### 代码实现

```java
    public int FindGreatestSumOfSubArray1(int[] array) {
        int res = array[0]; //记录当前所有子数组的和的最大值
        int max = array[0];   //包含array[i]的连续数组最大值
        for (int i = 1; i < array.length; i++) {
            max = Math.max(max + array[i], array[i]);
            res = Math.max(max, res);
        }
        return res;
    }
```

上述两种思路的时间复杂度均为`O(n)`。

## 最小的k个数

题目描述：输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

### 解题思路

这个题最直观的思路，也是能够直接解决问题的解法就是直接对数组进行排序，然后取最小的前k个数就可以了。这种思路的平均时间复杂度为O(nlong)，并且不适合用于海量数据，面试的时候会提示有更快的解法。

还有一种思路就是用最大堆保存这k个数，每次只和堆顶比，如果比堆顶小，删除堆顶，然后新数入堆。这种思路下时间复杂度为O(nlogk)，并且可以处理海量数据。 

### 代码实现

#### 解法一：对数组进行排序实现

```java
public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
    if(input == null || input.length <= 0 || k <= 0 || k > input.length){
        return null;
    }
    ArrayList<Integer> array = new ArrayList<>();
    for(int i = 0 ; i < input.length ; i++){
        array.add(input[i]);
    }
    Collections.sort(array);
    ArrayList<Integer> result = new ArrayList<>();
    for(int i = 0 ; i < k ; i++){
        result.add(array.get(i));
    }
    return result;
}
```

#### 解法二：基于最大堆实现

```java
public ArrayList<Integer> GetLeastNumbers_Solution1(int[] input, int k) {
    ArrayList<Integer> result = new ArrayList<>();
    int length = input.length;
    if(k > length || k < 1){
        return result;
    }
    //基于最大堆实现
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(k, new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return o2.compareTo(o1);
        }
    });
    for (int i = 0; i < length; i++) {
        if (maxHeap.size() != k) {
            maxHeap.offer(input[i]);
        } else if (maxHeap.peek() > input[i]) {
            Integer temp = maxHeap.poll();
            temp = null;
            maxHeap.offer(input[i]);
        }
    }
    for (Integer integer : maxHeap) {
        result.add(integer);
    }
    return result;
}
```

## 数组中出现次数超过一半的数字

题目描述：数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

### 解题思路

刚看到这个题的时候，最直观的思路应该还是使用排序，这样时间复杂度为O(nlogn)，但是还是那句话，最直观的思路往往不是面试官满意的算法。

首先在思路上可以想到，能够使用`HashMap`，由于`HashMap`不能存储相同的键，那么把数组中的数字作为键，如果相同，则其值加1，这样最后直接判断`HashMap`中的键对应的值是否超过了数组的一半即可，并且这种解法时间复杂度为O(n)。

除去`HashMap`之外，针对数组本身，我们还可以想到一种O(n)的解法，那就是只遍历一次数组，在这次遍历中找到其中重复次数最多的数字，然后跟数组长度的一半进行比较。这种解法的时间复杂度也为O(n)。

### 代码实现

#### 解法一：对数组进行排序

```java
public int MoreThanHalfNum_Solution(int [] array) {
    Arrays.sort(array);
    int count=0;

    for(int i=0;i<array.length;i++){
        if(array[i]==array[array.length/2]){
            count++;
        }
    }
    if(count>array.length/2){
        return array[array.length/2];
    }else{
        return 0;
    }
}
```

#### 解法二：使用HashMap

```java
public int MoreThanHalfNum_Solution(int[] array) {
    HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();

    for(int i=0;i<array.length;i++){

        if(!map.containsKey(array[i])){
            map.put(array[i],1);
        }else{
            int count = map.get(array[i]);
            map.put(array[i],++count);
            if(map.get(array[i]) > array.length/2){
                return array[i];
            }
        }
    }
    return 0;
}
```

#### 解法三： 基于数组本身的特点

```java
public int MoreThanHalfNum_Solution(int [] array) {
    if(array == null || array.length <= 0)
        return 0;
    int len = array.length;
    int times = 1;
    int res = array[0];
    // 遍历每个元素，并记录次数；若与前一个元素相同，则次数加1，否则次数减1
    for(int i = 1; i < len; i++){
        if(times == 0){
            // 更新result的值为当前元素，并置次数为1
            res = array[i];
            times = 1;
        }else if(array[i] == res)
            times++;
        else
            times--;
    }
    times = 0;
    for(int num : array)
        if(num == res)
            times++;
    return (times > len / 2)? res : 0;
}
```

## 整数中1出现的次数

题目描述：输入一个整数n，求1~n中这n个整数的十进制表示中1出现的次数。例如：输入12，则1~12的整数中1出现的数字有1、10、11和12，1一共出现了5次。

## 解题思路

