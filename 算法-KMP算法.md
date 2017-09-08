---
title: 算法-KMP算法
date: 2017-07-21 10:00:16 
categories: 算法
tags: [算法,Algorithm,面试] 
description: 面试算法之----KMP算法（摘自数据结构与算法经典问题解析 338 页问题 15.6）

---
   
KMP算法

字符串匹配是计算机的基本任务之一。

问题：给定一个主字符串（以 S 代替）和模式串（以 P 代替），要求找出 P 在 S 中出现的位置，即串的模式匹配问题。今天来介绍解决这一问题的常用算法之一，Knuth-Morris-Pratt 算法（简称 KMP），这个算法是由Knuth、Morris、Pratt共同提出。

 - 时间复杂度：O（m+n） （n为文本串T的长度）
 - 空间复杂度：O（m） （m为模式串P的长度）

这个问题有多种解法：

- 蛮力法。
- Robin-Karp字符串匹配算法。
- 基于有限自动机的字符串匹配算法。
- Boyce-Moore算法。
- 后缀树。

在继续下面的内容之前，有必要在这里介绍下两个概念：前缀和后缀:
![这里写图片描述](http://img.blog.csdn.net/20170720105330877?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<font color="#ff0000" size = "3px"><b>注意："hin" 并不是"china"前缀或后缀。</b><font>

以下摘自<font color="#ff0000" size = "3px">刘毅</font>[KMP 算法（1）：如何理解 KMP](http://www.61mon.com/index.php/archives/183/ "KMP 算法（1）：如何理解 KMP")。

###算法思想：

1、首先，主串 "BBC ABCDAB ABCDABCDABDE" 的第一个字符与模式串 "ABCDABD" 的第一个字符，进行比较。因为 B 与 A 不匹配，所以模式串后移一位。

![这里写图片描述](http://oi0fekpsr.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%95_2.png?imageView2/2/w/1440/q/75/format/webp)
2、因为 B 与 A 又不匹配，模式串再往后移。
![这里写图片描述](http://oi0fekpsr.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%95_3.png?imageView2/2/w/1440/q/75/format/webp)
3、就这样，直到主串有一个字符，与模式串的第一个字符相同为止。
![这里写图片描述](http://oi0fekpsr.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%95_4.png?imageView2/2/w/1440/q/75/format/webp)
4、接着比较主串和模式串的下一个字符，还是相同。
![这里写图片描述](http://oi0fekpsr.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%95_5.png?imageView2/2/w/1440/q/75/format/webp)
5、直到主串有一个字符，与模式串对应的字符不相同为止。
![这里写图片描述](http://oi0fekpsr.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%95_6.png?imageView2/2/w/1440/q/75/format/webp)
6、这时，最自然的反应是，将模式串整个后移一位，再从头逐个比较。这样做虽然可行，但是效率很差，因为你要把 "搜索位置" 移到已经比较过的位置，重比一遍。
![这里写图片描述](http://oi0fekpsr.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%95_7.png?imageView2/2/w/1440/q/75/format/webp)
7、一个基本事实是，当空格与 D 不匹配时，你其实知道前面六个字符是 "ABCDAB"。KMP 算法的想法是，设法利用这个已知信息，不要把 "搜索位置" 移回已经比较过的位置，而是继续把它向后移，这样就提高了效率。该算法避免了T中部分元素的再次比较，如果这些元素已经与模式串P中的部分元素比较过。
![这里写图片描述](http://oi0fekpsr.bkt.clouddn.com/KMP%E7%AE%97%E6%B3%95_8.png?imageView2/2/w/1440/q/75/format/webp)


###前缀表：

&emsp;&emsp;该算法使用一个表F，F通常称为前缀函数、前缀表或失配函数。下面首先介绍如何构造表F，然后阐述如何使用该表进行模式串匹配。
&emsp;&emsp;前缀表F中存储了模式串资深如何移动的信息。此信息用来避免模式串P不必要的移动。也就是说，此表用以避免在字符串T中的回溯。

####前缀表算法：

```
int[] F;     //全局变量，next数组，也称前缀表
	private void Prefix_Table(int P[],int m){
		int i=1,j=0;F[0]=0;
		while(i<m){
			if(P[i]==P[j]){
					F[i]=j+1;
					i++;
					j++;
			}else if(j>0){
				j=F[j-1];
			}else{
				F[i]=0;
				i++;
			}
		}
	}
```
####前缀表算法解析：
![这里写图片描述](http://img.blog.csdn.net/20170720113843819?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)![这里写图片描述](http://img.blog.csdn.net/20170720113852250?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

懵逼歇菜不可怕，下来跟上思路对上面的算法详细解析：
假设模式串P为 a b a b a c a。依据如下步骤构造前缀表F。初始时候，m=lenth[P]=7，F[0]=0。
![这里写图片描述](http://img.blog.csdn.net/20170720115544166?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
1、i=1；j=0；P[1]！=P[0]，且j=0，则F[1]=0，i++；
解释：因为P[1]！=P[0]，在0-1这个子串上所以没有相等的前后缀，在1这个位置上构造表填0；
![这里写图片描述](http://img.blog.csdn.net/20170720121222563?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
2、i=2；j=0；P[2]==P[0]，则F[2]=1，i++，j++；
解释：因为P[2]==P[0]，在0-2这个子串上出现了相等的前后缀，a==a，且最大长度为1，在2这个位置上构造表填1。
![这里写图片描述](http://img.blog.csdn.net/20170720121443387?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
3、i=3；j=1；P[3]==P[1]，则F[3]=2，i++，j++；
解释：因为P[3]==P[1]，在0-3这个子串上出现了相等的前后缀，ab==ab，且最大长度为2，在3这个位置上构造表填2。
![这里写图片描述](http://img.blog.csdn.net/20170720121622826?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
4、i=4；j=2；P[4]==P[2]，则F[4]=3，i++，j++；
在这儿停一下，先不急着往下看，思考一下，填的这个3是相等（重复）前后缀的长度，其实对前缀来说，它也是下标，是前缀末尾数字的下一个下标位置。这样，当我们去匹配i=5这个位置上的数时，如果没有匹配上，但是很显然在它前面的数P[0-4]肯定是匹配成功的，也就是说很显然它前面的前后缀肯定是匹配成功的，所以下一次我们就没必要回溯到j=0的位置，而是直接从前缀的下一个下标位置i=3开始即可。有没有懂一点？所以这儿就解释了为什么算法中F[i]=j+1。因为F[i]记录了相等前后缀的下一个下标位置哦，也即为相等前后缀的长度。
![这里写图片描述](http://img.blog.csdn.net/20170720121721469?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
5、i=5；j=3；P[5]!=P[3]，且j>0，则j=F[j-1]=1；
6、继续while，i=5；j=1；P[5]!=P[1]，且j>0，则j=F[j-1]=0；
7、继续while，i=5；j=0；P[5]!=P[0]，且j！>0，则F[i]=F[5]=0，i++；

咦，<font color="#ff0000" size = "3px">为什么当P[i]!=P[j]时j=F[j-1]呢？</font> 这才是我们讨论的重点。见下面注：
![这里写图片描述](http://img.blog.csdn.net/20170720122438009?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
8、i=6；j=0；P[6]==P[0]，则F[6]=1，i++，j++；
![这里写图片描述](http://img.blog.csdn.net/20170720122450152?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
至此，前缀表构造完成。

<font color="#ff0000" size = "3px"><b>注：</b></font> 
看了多多少少不下10篇博客，没发现有一篇把else这块是彻彻底底讲清楚这个问题的，可见是多么的难以描述。晚上终于是把j=F[j-1]（有的地方为j=F[j]，原因是我们定义的前缀表初始值不同）搞懂了，历时1天，可见有多笨................
首先，必须明确这样一个结论：

当P[i] == P[j]时，
有<font color="#ff0000" size = "3px">F[j+1] == F[j] + 1，F[j-1] == F[j] - 1</font> 
这个结论不多说，如下图紫色线条，稍微揣摩一下就能看得出来。
![这里写图片描述](http://img.blog.csdn.net/20170721105220168?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<font color="#ff0000" size = "3px">寻找前后缀的过程其实就是个匹配过程，理解成这样很重要，因为我们下面的内容都是基于这个思路分析的！</font> 在i=5和j=3的地方c!=b，匹配不成功。摒弃 i 这个概念，先不管 i。

暴力情况下，当匹配不成功时，我们的j应该回溯到0的位置，但是KMP的思想是如果存在最长前后缀，那我们就没必要再回溯到开头去，而是利用这个已经匹配过的最长前后缀。那我们致力于去寻找这个最长前后缀。在j=3时，j位置的最长前后缀即为F[j]，长度为2，F[j]=k+1，所以K=1。此时如果我们将j向左移动到j=k=1，再去和i=5匹配，是不是j没有回溯到开头，而是利用了匹配过的最长前后缀，仔细体会一下。如下图：
![这里写图片描述](http://img.blog.csdn.net/20170721105227572?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUzNDgzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
那这个时候的j=1用公式怎么求呢？

前面我们知道：
① F[j+1] == F[j] + 1
② F[j-1] == F[j] - 1
③ F[j]=k+1

由 ③ 可得 F[j]-1 = k，记为④。
由 ②+④ 可得k= F[j-1]。

所以F[j-1]应为我们的j往前移动的位置，即：
```
else if(j>0){
				j=F[j-1];
			}
```

终于是分析完这个else了，搞定了前缀表的构造，下来的匹配算法就简单多了，其实在上面我们已经把匹配算法的思想贯彻了。
 
----------
		
算法如下（java）：

```
	private int KMP(String Ts,String Ps){
		int i=0,j=0,m=0,n=0;
		m=Ps.length();
		n=Ts.length();
		char[] T = Ts.toCharArray();
		char[] P = Ps.toCharArray();
		if(n<m){return -1;}
		Prefix_Table(P,m);
		while(i<n && j<m){
			if(j<=0 || T[i]==P[j]){
					i++;
					j++;
			}else{
				j=F[j-1];
		}
	}
		if(j==m){
			return i-j;
		}
		return -1;
	}
```

----------


csdn地址：http://blog.csdn.net/u012534831
github地址：https://github.com/qht1003077897
个人博客地址：https://qht1003077897.github.io/
QQ：1003077897



