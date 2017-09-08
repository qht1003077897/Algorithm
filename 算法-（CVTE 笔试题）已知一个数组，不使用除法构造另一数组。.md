---
title: 算法-不使用除法构造数组
date: 2017-09-08 11:00:16 
categories: 算法,面试
tags: [算法,Algorithm,面试] 
description: 面试算法之----
title: 算法-不使用除法构造数组（CVTE 笔试题）
---
   

####题目：给定一数组a[N]，我们希望构造数组b [N]，其中b[j]=a[0]*a[1]…a[N-1] / a[j]，在构造过程中，不允许使用除法；要求O（1）空间复杂度和O（n）的时间复杂度。

样例输入：[2,5,6,8]

计算过程：   
 - 2*5*6*8/2=240
 - 2*5*6*8/5=96
 - 2*5*6*8/6=80
 - 2*5*6*8/8=60
 
样例输出：[240,96,80,60]

b[j]=[5*6*8，2*6*8，2*5*8，2*5*6]

观察b[j]的构造公式，发现它实际上是由两部分相乘得到。即b[j]是a[j]左边的数和a[j]右边的数相乘，中间的a[j]被除掉了。接下来就是怎么实现的问题了。先从左向右扫描a[N]数组，将坐标左边的元素累乘起来存入b[N]；然后可用一个变量保存右边数组元素乘积，从右向左扫描一遍，依次将b[N]和乘积变量相乘即可。

####代码实现：

```
public static void main(String[] arg) {
        int[] A=new int[]{2,5,6,8};
        int[] B=new int[A.length];
        B[0]=1;
        for(int j=1;j<A.length;j++){
            B[j]=A[j-1]*B[j-1];
        }
        for(int k=A.length-1;k>0;k--){
            B[k]=B[k]*B[0];
            B[0]=A[k]*B[0];
        }
        for (int l:B) {
            System.out.print(l+"");
        }
    }
```
![这里写图片描述](http://img.blog.csdn.net/20170908114046763?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)