# 带精英策略的非支配排序遗传算法2

# 一、多目标进化算法

进化算法是一类模拟生物自然选择与自然进化的随机搜索算法，因其适用于求解高度复杂的非线性问题而得到了广泛的应用，同时他又具有较好的通用性。在解决只有单个目标的复杂系统优化问题时，进化算法的优势得到了充分展现。然而，现实世界中的优化问题通常是多属性的，一般是对多个目标的同时优化。由此，针对多个目标的优化问题，出现了多目标进化算法MOEA。

 

## 1.1 MOEA概述

1967年，Rosenberg建议采用基于进化的搜索来处理多目标优化问题，但他没有具体实现。1985年，David Schaffer首次在机器学习中实现了向量评估遗传算法。1989年，David Goldberg提出来用进化算法实现多目标的优化技术，在对多目标进化算法的研究具有重要的方向性指导意义。

1994-2001年的8年，国际上所出版的论文是过去10年的3倍多。最近15年的发展速度比过去8年又有很大提高，一方面，在IEEE Transacrions on Evolutionary Computation、Evolutionary Computation和Genetic Programming and Evolvable Machines等国际重要学术期刊，以及各类国际进化计算学术会议上发表的有关多目标进化的论文比过去八年增长的幅度大很多，另一方面有关进化算法的期刊或会议的影响力越来越大，如IEEE Transacrions on Evolutionary Computation、Evolutionary Computation按JCR期刊影响因子均已进入SCI一区。第三方面，应用成果越来越多，涉及的应用范围越来越广。

 

## 1.2 MOEA的分类

MOEA种类较多，在此我们只讨论按不同的进化机制和不同的决策方式对MOEA进行分类:

- 基于分解的MOEA
- 基于支配关系的MOEA
- 基于指标的MOEA

 

NSGA-Ⅱ属于支配关系的MOEA



 # 二、NSGA-Ⅱ算法介绍

## 2.1 NSGA算法

NSGA通过基于非支配排序的方法保留了种群中的优良个体,并且利用适应度共享函数保持了群体的多样性，取得了非常良好的效果。但实际工程领域中发现NSGA算法存在明显不足，这主要体现在如下3个方面：

- 非支配排序的高计算复杂性。非支配排序算法一般要进行mN^3次搜索，搜索的次数随着目标函数数量和种群大小的增加而增多。（m为目标数，N为群体大小） 
- 缺少精英策略。研究结果表明，引用精英策略可以加快遗传算法的执行，并且还助于防止优秀的个体丢失。
- 需要指定共享参数share，在NSGA算法中保持种群和解的多样性方法都是依赖于共享的概念，共享的主要问题之一就是需要人为指定一个共享参数share。

 

## 2.2 NSGA-Ⅱ算法

为了克服非支配排序遗传算法(NSGA)的上述不足，印度科学家Deb于2002年在NSGA算法的基础上进行了改进，提出了带精英策略的非支配排序遗传算法(Elitist Non-Dominated Sorting Genetic Algorithm，NSGA-II)，NSGA-II 算法针对NSGA的缺陷通过以下三个方面进行了改进：

- 提出了快速非支配的排序算法，降低了计算非支配序的复杂度。
- 引入了精英策略，扩大了采样空间，提高优化结果的准确度。
- 引入拥挤度和拥挤度比较算子，这不但克服了NSGA算法中需要人为指定共享参数的缺陷，而且将拥挤度作为种群中个体之间的比较准则，使得准Pareto域中的种群个体能均匀扩展到整个Pareto域，从而保证了种群的多样性。

 

**什么是非支配排序？**

答：将一组解分成n个集合：rank1,rank2…rankn,每个集合中所有的解都互不支配，但ranki中的任意解支配rankj中的任意解（i<j）。

 

**什么是精英策略？**

答：精英策略即保留父代（上代），然后让父代和经过选择、交叉、变异后产生的子代共同组成一个群体，其目的就是为了防止父代中可能存在的最优解被遗落，最后经过再次选择操作，获得与初始种群同样规模的群落。 

## 2.3 NSGA-Ⅱ算法流程图

![image-20230306210745452](assets/image-20230306210745452.png)

首先，随机产生规模为N的初始种群,非支配排序后通过遗传算法的选择、交叉、变异三个基本操作得到第一代子代种群；

其次，从第二代开始,将父代种群与子代种群合并,进行快速非支配排序,同时对每个非支配层中的个体进行拥挤度计算,根据非支配关系以及个体的拥挤度选取合适的个体组成新的父代种群;

最后，通过遗传算法的基本操作产生新的子代种群:依此类推,直到满足程序结束的条件。

## 选择、交叉、变异

遗传算法中个体的编码方式主要有实数编码、二进制编码、格雷编码等，根据系统容量一般较大的特点，本文选择实数编码方式。在优化过程中，个体的好坏依赖于个体的适应度，适应度高的个体有更大的可能被保留进入下一代。在实际操作当中，适应度一般为个体的目标函数。

### （1）选择

选择操作模仿自然界中的“优胜劣汰”法则，若个体的适应度高则其有更大概率被遗传到下一代，反之则概率较小。进行选择操作的方法有许多，比如轮盘赌选择、排序选择、最优个体保存、随机联赛选择等。

- 轮盘赌选择：将种群中所有个体的适应度值加和，并把每个个体的适应度值与和的比值作为该个体选择的选择概率，从而个体适应度越高被选中概率越高。
- 排序选择：按照适应度值大小对所有个体进行排序，并根据排序确定个体被选中的概率。
- 最优个体保存：会将父代群体中的最优的个体直接保存入子代个体中，保证了优秀个体能够遗传到下一代。
- 随机联赛选择：设置固定值k，每次随机取k个个体，将其中适应度最高的个体遗传入下一代。

![image-20230306210840086](assets/image-20230306210840086.png)

### （2）交叉

交叉操作模拟自然界中染色体的交叉换位现象，用于生成新个体，决定了算法的全局搜索能力。标准的NSGA-II算法使用模拟二进制交叉算子(SBX)，第k+1代个体的计算公式如下：

![image-20230306210856082](assets/image-20230306210856082.png)

上式中，p1,k+1和p2,k+1是交叉后生成的第k+1代个体；p1,k和p2,k是被选中的第k代个体；βqi是均匀分布因子,其计算方式如下：

![image-20230306210909708](assets/image-20230306210909708.png)

式中，ui是属于[0,1)的随机数；η是交叉分布指数，一般的定义为20~30，η的大小会影响产生的个体距离父代个体的远近。

### （3）变异

变异操作是模拟生物的基因变异，同交叉操作一样，都用于产生新个体。标准NSGA-II算法的变异算子为多项式变异算子，第k+1代个体的计算公式如下：

![image-20230306210929746](assets/image-20230306210929746.png)

上式中，pk是被选中的第k代个体；pk+1是pk经变异操作得到的第k+1代个体；pmax k和pmin k分别为决策变量的上界和下界；δk计算公式如下：

![image-20230306210943394](assets/image-20230306210943394.png)

式中，rk为[0,1]中的均匀分布随机数；ηm为变异分布指数。



## 快速非支配排序

种群中的每个个体都有两个参数ni和Si，ni为种群中支配个体i 的个体数量，Si是被个体i支配的个体的集合,快速非支配排序的步骤如下：

- 通过循环比较找到种群中所有ni = 0的个体，赋予其非支配等级为1，并将这些个体存入非支配集合rank1中。
- 集合rank1中的每一个个体，将其所支配的个体集合中的每个个体的nj都减去1，若nj-1=0则将个体j存入集合rank2中，并赋予其中的个体非支配等级2。
- 之后对rank2中的个体重复上述操作，直至所有个体都被赋予了非支配等级。

 

非支配等级也称作Pareto等级，其中Pareto等级为1的个体由于不受其他个体的支配，叫做非支配解，也叫 Pareto最优解，而解集所形成的曲线叫做Pareto前沿。以有两个目标函数f1和f2为例，假设经过快速非支配排序之后共分成了三个Pareto等级，如图4.1所示。图4.1中用圆圈表示的个体即Pareto等级为1的个体组成的集合为本例的Pareto最优解，这些个体形成的曲线即为本例的Pareto前沿。

![image-20230306211005814](assets/image-20230306211005814.png)

### 快速非支配排序伪代码

假设种群大小为P，该算法需要计算每个个体p的被支配个数n_p和该个体支配的解的集合S_p这两个参数。遍历整个种群，该参数的计算复杂度为O（mM^2）。该算法的伪代码如下：

1. 计算出种群中每个个体的两个参数n_p和S_p。
2. 将种群中n_p=0的个体放入集合F1中。
3. ![image-20230306211350746](assets/image-20230306211350746.png)
4. 上面得到pareto等级2的个体的集合F2，对集合F2中的个体继续重复步骤3，以此类推到种群等级被全部划分。

![image-20230306211410864](assets/image-20230306211410864.png)

## 拥挤度计算

在NSGA算法中，需要指定共享半径σshare，这对经验要求较高，为了克服这一缺点，NSGA-II引用了拥挤度的概念。拥挤度表示空间中个体的密度值，直观上可以用个体 周围不包括其他个体的长方形表示，如图所示。

![image-20230306211439032](assets/image-20230306211439032.png)

**目的**：同一层非支配个体集合中，为了保证解的个体能均匀分配在Pareto前沿，就需要使同一层中的非支配个体具有多样性，否则，个体都在某一处“扎堆”，将无法得到Pareto最优解集。NSGA—II采用了拥挤度策略，即计算同一非支配层级中某给定个体周围其他个体的密度。
每个个体的拥挤距离是通过计算与其相邻的两个个体在每个子目标函数上的距离差之和来求取。



### 拥挤度计算伪代码

![image-20230306211622976](assets/image-20230306211622976.png)

从二目标优化问题来看，就像该个体在目标空间所能生成的最大矩形（该矩形不能触碰目标空间其他的点）的边长之和。





## 精英策略

NSGA-II算法引入了精英策略，达到保留优秀个体淘汰劣等个体的目的。精英策略通过将父代与子代个体混合形成新的群体，扩大了产生下一代个体时的筛选范围。以图所示的例子进行分析，图中P表示父代种群，设其中的个体数量为n，Q表示子代种群，具体步骤如下：

- 将父代种群和子代种群合并形成新的种群。之后对新种群进行非支配排序，本例中将种群分成了6个Pareto等级。
- 进行新的父代的生成工作，先将Pareto等级为1的非支配个体放入新的父代集合当中，之后将Pareto等级为2的个体放入新的父代种群中，以此类推。
- 若等级为k的个体全部放入新的父代集合中后，集合中个体的数量小于n，而等级为k+1的个体全部放入新的父代集合中后，集合中的个体数量大于n，则对第k+1等级的全部个体计算拥挤度并将所有个体按拥挤度进行降序排列,之后将等级大于k+1的个体全部淘汰。本例中可以看出k为2，所以对Pareto等级为3的个体计算拥挤度并按其进行降序排序，等级为4~6的个体全部淘汰。
- 将等级k+1中的个体按步骤2中排好的顺序逐个放入新的父代集合中，直到父代集合中的个体数量等于n，剩余的个体被淘汰。

![image-20230306211731863](assets/image-20230306211731863.png)

## 主程序伪代码

![image-20230306211805910](assets/image-20230306211805910.png)

1. 首先将父代Pt和子代Qt的个体放到一个数组为Rt

2. 对Rt进行非支配排序

3. 初始化

4. 直到父种群被填充为止（F1表示第一梯队，即Pareto等级为1的种群）

5. 1. 计算F1的拥挤度（拥挤度计算）
   2. 将最优的梯队拿出来组成新的父代P

1. （精英策略）如果群数量有限，所以到了再加一个梯队就超出种群数量的时候，就需要对这个边缘梯队做一个淘汰，这时候拥挤距离这个参数就用上了，它就是用来区分同梯队个体优劣的。
2. 新的父代生成完毕，执行遗传算法形成新的子代（make-new-pop：选择、交叉、变异）
3. 继续循环