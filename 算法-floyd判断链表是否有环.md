---
title: 算法-floyd判环(圈)算法
date: 2017-07-03 22:00:16 
categories: 算法,面试
tags: [算法,Algorithm,面试] 
description: 面试算法之----floyd判环算法（摘自数据结构与算法经典问题解析57页问题9）
---
   
floyd（Floyd cycle detection）

问题：如何检测一个链表是否有环，如果有，那么如何确定环的起点？如何确定环的长度？

 - 时间复杂度：O（n） （高效率）
 - 空间复杂度：O（1）

此算法又成为龟兔赛跑算法，基本思想是：

好比两个人在赛跑，A的速度快，B的速度慢，经过一定时间后，A总是会和B相遇，且相遇时A跑过的总距离减去B跑过的总距离一定是圈长的n倍（初中数学题......）。

弗洛伊德（Floyd ）使用了两个指针，一个慢指针（龟）每次前进一步，快指针（兔）指针每次前进两步（两步或多步效果是等价的，只要一个比另一个快就行。但是如果移动步数增加，算法的复杂度可能增加）。如果两者在链表头以外（不包含开始情况）的某一点相遇（即相等）了，那么说明链表有环，否则，如果（快指针）到达了链表的结尾（如果存在结尾，肯定无环），那么说明没环。


环的检测从上面的解释理解起来应该没有问题。接下来我们来看一下如何确定环的起点，这也是Floyd解法的第二部分。方法是将慢指针（或快指针）移到链表起点，然后两者同时移动，每次移动一步，那么两者相遇的地方就是环的起点。

下面给出论证过程：

假设一个链表是下面这样的：

![这里写图片描述](http://img.blog.csdn.net/20170703170559457?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

设环长为n，非环形部分长度为m，当第一次相遇时显然slow指针行走了 m+A*n+k（A表示slow行走了A圈。附：A*n 是因为如果环够大，则他们的相遇需要经过好几环才相遇）。fast行走了 m+B*n+k。

上面我们说了slow每次行走一步，fast每次行走两步，则在同一时间，fast行走的路程是slow的两倍。假设slow行走的路程为S，则fast行走的路程为2S。

用fast减去slow可得：
```
S=（B-A）*n
```
很显然这意味着当slow和fast相遇时他们走过的路程都为圈长的倍数。

接下来，将slow移动到起点位置，如下图：

![这里写图片描述](http://img.blog.csdn.net/20170703172340818?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


然后每次两个指针都只移动一步，当slow移动了m，即到达了环的起点位置，此时fast总共移动了 2S+m。 考虑到S为环长的倍数，可以理解为：fast先从链表起点出发，经过了m到达环的起点，然后绕着环移动了几圈，最终又到达环的起点，值为2S+m。所以fast最终必定处在环的起点位置。即两者相遇点即为环的起点位置。

----------


算法如下（Java）：

```
public ListNode findBeginLoop(ListNode head){

ListNode slowPtr=head;
ListNode fastPtr=head;
boolean loopExists=false;

if(head=null){return 0;}

while(slowPtr.getNext()!=null && fastPtr.getNext().getNext()!=nulll){
 slowPtr=slowPtr.getNext();
 fastPtr=fastPtr.getNext().getNext();
 if(slowPtr==fastPtr){
	loopExists=true;
	break;
	}
  }
  if(loopExists){  //环存在
    slowPtr=head;
	while(slowPtr!=fastPtr){
		slowPtr=slowPtr.getNext();
		fastPtr=fastPtr.getNext();
	}
	return slowPtr;
  } 
  return null; //环不存在
}

```
如果要求环的长度，可以这样做：

当两者相遇之后，固定一个指针，让另一个指针行走一圈，使用count计数，如果两个指针相等（即相遇），则count即为环的长度。


csdn地址：http://blog.csdn.net/u012534831
github地址：https://github.com/qht1003077897
个人博客地址：https://qht1003077897.github.io/
QQ：1003077897



