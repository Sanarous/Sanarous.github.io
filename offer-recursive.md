## 斐波拉契数列

题目描述：大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39

斐波拉契数列应该是很多学生大一学习C语言时解决的一个问题，我记得那时候好像是用递归算法去解决的，然鹅稍微懂一些数据结构的小伙伴应该都知道，斐波拉契数列是最好不使用递归算法的，为什么呢，因为递归计算斐波拉契数列的时候重复计算项很多，而且越到后面计算量越来越爆炸，最后会导致运行超时，比如栈溢出等exception，如果我们使用递归，代码确实简洁了，比如下面：

```java
public int Fibonacci(int n){
    if(n < 1)  return 0;
    
    if(n == 1) return 1;
    
    return Fibonacci(n-1) + Fibonacci(n-2);
}
```

但是比如我们想求解第10项数据的时候，我们以`f(10)`表示第10项斐波拉契结果，那么用递归运行时会出现下面结构：

![](https://blogimage-1258928558.cos.ap-guangzhou.myqcloud.com/offer/fib.png)

不难看出，重复的节点会随着`n`的增大而急剧增大，事实上也是，用递归方法计算的时间复杂度是以`n`的指数方式递增的，不妨可以使用递归计算一下`Fibonacci`的第`100`项，看看会慢到什么程度。

所以实用解法肯定不是这种啦。

而改进方法，实际上目的就是为了避免上面的重复计算，那么我们实际上用一个数列保存已经计算过的数。于是我们可以写出如下代码：

```java
public int Fibonacci(int n) {
   if(n < 1){
       return 0;
   }
   if(n == 1 || n == 2){
       return 1;
   }
   if(n == 3){
       return 2;
   }
   int fib1 = 1;
   int fib2 = 2;
   int result = 0;
   for(int i = 3 ; i < n ; i++){
       result = fib1 + fib2;
       fib1 = fib2;
       fib2 = result;
   }
   return result;
}
```

上面代码就可以简约的搞定问题了！

## 青蛙跳台阶问题

题目描述：一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

首先我们可以从最简单的情况开始考虑，如果只有一级台阶，那显然只能跳一次。如果有两级台阶，那么就有两种跳法：一种是每次跳一级；另一种是一次跳两级。

然后我们将这种情况一般化，假如有`n`级台阶，可以把跳`n`级台阶的跳法看作是`n`的函数`f(n)`。当`n>2`时，第一次跳的时候就有两种选择：

- 第一次跳一级，那么跳法数目等于后面的`n-1`级台阶的跳法数目，即为`f(n-1)`。
- 第一次跳两级，那么跳法数目等于后面的`n-2`级台阶的跳法数目，即为`f(n-2)`。

因此，`n`级台阶的不同跳法数目的总数为`f(n)=f(n-1)+f(n-2)`，到这里应该可以看出，说到底这个问题就是一个包装的斐波拉契数列问题。

那么代码其实跟上面的没啥区别：

```java
 public int JumpFloor(int target) {
    if(target <= 2){
       return target;
    }
    int pre1 = 1;
    int pre2 = 2;
    int result = 0;
    for(int i = 2;i < target; i++){
        result = pre1 + pre2;
        pre1 = pre2;
        pre2 = result;
    }
    return result;
}
```

## 变态青蛙跳台阶

题目描述：这次这只青蛙很变态，一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

这个其实问题更不大，这本质上就是一个简单的数学问题，我们可以根据上面那个问题归纳这个问题：

跳`n`级台阶时，这时候跳法就很多了：

- 第一次跳一级，那么剩下的就是`f(n-1)`
- 第一次跳两级，那么剩下的就是`f(n-2)`
- 第一次跳三级，那么剩下的就是`f(n-3)`
- ....
- 第一次跳n-1级，那么剩下就是`f(1)`.

那么实际上就是`f(n)=f(n-1)+f(n-2)+f(n-3)+...+f(1)`，又因为`f(n-1)=f(n-2)+f(n-3)+...+f(1)`

，所以`f(n)=2f(n-1)`。

到这里应该可以看出来了吧？这不就是一个等比数列吗，并且`f(1)=1`。

那么实际上`f(n)=2^(n-1)`，完事！

```java
public int JumpFloorII1(int target){
   return (int)java.util.Math.pow(2,target-1);
}
```

## 矩形覆盖

<div class="note info">题目描述：我们可以用2x1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2x1的小矩形无重叠地覆盖一个2xn的大矩形，总共有多少种方法？ </div>

这个就借用`牛客网答题者csdong`的回答，很简单易懂：

![](https://blogimage-1258928558.cos.ap-guangzhou.myqcloud.com/offer/juxing.png)

所以跟上面的还是没区别。。。依然是老瓶装新酒。

```java
public int RectCover(int target) {
   if(target <= 2){
       return target;
   }
   int pre1 = 1,pre2 = 2;
   int result = 0;
   for(int i = 3; i <= target;i++){
        result = pre1 + pre2;
        pre1 = pre2;
        pre2 = result;
   }
   return result;
}
```