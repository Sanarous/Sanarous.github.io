## 链表中倒数第k个节点

题目描述：输入一个链表，输出该链表中倒数第k个节点。

虽然最直观的思路往往不是最优解，但是我们在做算法题的时候还是需要从最直观的解法出发，不断优化现有思路或者参考他人解法来获取新的idea来解决问题，这样才能达到思维层面上的进步。

就这题而言，最直观的解法显然是先遍历链表，找到链表的长度`length`,然后再找到第`length-k+1`位置处的节点，就是需要的倒数第`k`个节点。那么可以很快的写出下面代码：

```java
public ListNode FindKthToTail(ListNode head,int k) {
    /**
     * 解题思路一：找到链表的长度n，再从头开始找n-k+1的链表的位置就是第k个节点
     */
    //边界条件
    if(head == null || k < 1){
        return null;
    }
    int index = 1;
    ListNode pNext = head;
    while(pNext != null){
        pNext = pNext.next;
        index ++;
    }
    int len = index;
    //如果倒数第k个节点不存在的边界条件
    if(k > len){
       return null;
    }
    //找到第n-k+1个节点的值返回
    int count = 1;
    ListNode pn = head;
     while(count <= len-k+1){
        count++;
        pn = pn.next;
        if(count == len-k+1){
           return pn;
        }
     }
    return null;
}
```

做完之后，结果当然是可以通过的，但是需要优化的地方也很明显，那就是上述解法遍历了两次链表，这显然是造成了冗余，那么优化的方向就是只遍历一次就可以获取我们需要的结果，根据这个思路，我们实际上可以想到双指针思路，这个在链表题目中经常遇到的思路显然也适合这里。

> 于是我们可以使用双指针，第一个指针先遍历链表到第`k-1`个节点位置，然后当第一个指针到第`k`个节点位置时，第二个指针开始从第一个节点处遍历，这样当第一个指针遍历到最后一个节点时，第二个指针的位置刚好是第`length-k+1`的位置，也就是倒数第`k`个节点的位置。这样一次遍历就可以完成我们需要的结果。

然后就不假思索的写出以下代码：

```java
public ListNode FindKthToTail1(ListNode head,int k){
  if(head == null || k < 1){
      return null;
  }
  ListNode pFast = head;
  ListNode pLow = head;
  int index = 1;
  while(pFast.next != null){
     pFast = pFast.next;
     if(index >= k){
         pLow = pLow.next;
      }
     index++;
   }
    return pLow;
 }
```

然后就发现只能通过测试用例的50%，为什么呢，呵呵......其实稍微注意一点应该能看到，在最开始的思路中我们是判断过边界条件`k < length`的，而下面这种做法显然是没有判断这个边界条件的。。。这就导致如果`k`是大于链表的长度的，那么就会出现`NullPointerException`，然而上面的方法是不会抛出空指针异常的，但是跟预期的`null`结果也是不一样的。这就需要我们处理一下这个问题。

那么怎么处理呢？反正不能再遍历链表了，不然跟第一种方法相比就没啥区别，于是我们可以想到一种巧妙的解法：

```java
 public ListNode FindKthToTail1(ListNode head,int k) {
   if(head == null || k < 1){
        return null;
   }
   ListNode p1 = head;
   //两个指针移动的代码不放在一起，先让第一个指针走k步
   while(p1 != null && k-- > 0){
        p1 = p1.next;
   }
   //然后在这里就可以加上判空条件了
   //如果这时候k不等于0，那么只能说明k有问题了，直接返回null
    if(k > 0) return null;
    ListNode p2 = head;
    while(p1 != null){
        p1 = p1.next;
        p2 = p2.next;
    }
      return p2;
}
```

## 链表中环的入口节点

题目描述：给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。 

![](https://blogimage-1258928558.cos.ap-guangzhou.myqcloud.com/offer/circle-list.png)

这个题没什么直观性思路，主要是看知不知道方法，我们需要获取以下几个思路并证明：

1. 设置快慢指针，假如有环，他们最后一定相遇。
2. 两个指针分别从链表头和相遇点继续出发，每次走一步，最后一定相遇于环入口。

先证明一下：设置快慢指针`fast`和`low`，`fast`每次走两步，`low`每次走一步。假如有环，两者一定会相遇（因为`low`一旦进环，可看作`fast`在后面追赶`low`的过程，每次两者都接近一步，最后一定能追上）。 

设：

链表头到环入口长度为--**a**

环入口到相遇点长度为--**b**

相遇点到环入口长度为--**c**

则：相遇时 

`快指针路程=a+(b+c)k+b` ，k>=1  其中b+c为环的长度，k为绕环的圈数（k>=1,即最少一圈，不能是0圈，不然和慢指针走的一样长，矛盾）。 

`慢指针路程=a+b  `

快指针走的路程是慢指针的两倍，所以： 

`（a+b）\*2=a+(b+c)k+b`

 化简可得： 

 `a=(k-1)(b+c)+c`  这个式子的意思是：  **链表头到环入口的距离=相遇点到环入口的距离+（k-1）圈环长度**。其中`k>=1`,所以`k-1>=0`圈。所以两个指针分别从链表头和相遇点出发，最后一定相遇于环入口。

```java
public class Solution {
    public ListNode EntryNodeOfLoop(ListNode pHead)
    {
        if(pHead == null || pHead.next == null){
            return null;
        }
        ListNode slow = pHead;
        ListNode fast = pHead;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            //到相遇点
            if(slow == fast){
                fast = pHead;
                while(slow != fast){
                    slow = slow.next;
                    fast = fast.next;
                }
                if(slow == fast){
                    return slow;
                }
            }
        }
        return null;
    }
}
```

## 从尾到头打印链表

题目描述：输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

这个题目的话思路就比较简单，借助一个栈来解决问题的话就只需要遍历一次链表。

```java
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
    ArrayList<Integer> list = new ArrayList<>();
    if(listNode == null){
         return list;
    }
    Stack<Integer> stack = new Stack<>();
    while(listNode != null){
       stack.push(listNode.val);
       listNode = listNode.next;
    }
    while(!stack.isEmpty()){
       list.add(stack.pop());
    }
    return list;
}
```

## 删除链表中的重复节点

题目描述：在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

这个题的话，我们需要处理两个问题：

1. 判断多节点重复问题
2. 将重复节点的前一个不重复节点的`next`指向下一个不重复的节点

如果上面两个问题能解决的话，那么这个问题就迎刃而解了。

为了解决第一个问题，比较好的思路应该是递归查找，即如果有两个节点重复了，那么我们在这两个节点重复的基础上继续往下查找看是否重复，这样找到所有重复的节点后，我们就可以解决第二个问题了，那么整个问题就可以解决了。

基于上述思路，我们可以写出如下代码：

```java
public ListNode deleteDuplication(ListNode pHead){
   //边界条件
   if(pHead == null || pHead.next == null){
       return pHead;
   }
   ListNode next = pHead.next;
   if(pHead.val == next.val){
       //保证重复的节点的前一个节点与后面比重复节点的值要大的节点相连
       //判断多节点重复
       while(next != null && pHead.val == next.val){
           //跳过值与当前节点相同的全部节点，找到第一个值不同的节点
           next = next.next;
       }
       //从第一个与当前不同的节点开始递归
       return deleteDuplication(next);
    }else{
        //当前节点不是重复节点
        pHead.next = deleteDuplication(pHead.next); //保留当前节点，从下一个节点开始递归
        return pHead;
    }
}
```