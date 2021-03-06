什么是P问题？什么是NP问题？什么是NPC问题？

###### 时间复杂度

时间复杂度并不是指一个程序运行完需要多少时间，而是随着数量级的增长程序所需要的时间长度增长的有多快。

多项式级别复杂度：O(1)  O(log(n)))  O(n^a) 

非多项式级复杂度：O(a^n)  O(n!)

###### 1、P类问题

一个问题可以找到用多项式级的复杂度解决它的算法

###### 2、NP问题

可以在多项式的时间里验证（猜出）一个解的问题

###### 关联：所有P类问题都是NP问题

###### 3、NPC问题

约化的概念：问题A约化为问题B---可以用B的解法解决A

B的时间复杂度高于或等于A

两个条件：一必须是一个NP问题；二是所有的NP问题都可以约化到它

如何证明？一证明至少是一个NP问题，二证明其中一个已知的NPC问题能够约化到它

NPC问题没有多项式的有效算法，只能用指数级甚至阶乘级计算

###### 4、NP-Hard问题

满足NPC问题定义的第一条，但是不一定满足第二条

NP-Hard  >  NPC

![1649321475877](.assets\1649321475877.png)



