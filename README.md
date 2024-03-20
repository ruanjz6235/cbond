# 剩余时间因子研究

剩余时间对可转债有显著的博弈价值，很明显，剩余时间越短，博弈价值越高，通过对剩余时间进行截面排序，并做相应标准化处理，可得到评分的截面回测：

![image](https://github.com/ruanjz6235/cbond/assets/51026474/4784f500-52db-4ec8-8aeb-473a6bc64722)
![image](https://github.com/ruanjz6235/cbond/assets/51026474/fa05b931-dd57-4e7e-bd06-60f9f25355a6)

因为有新可转债发行才会改变剩余时间的截面排名，这本质上导致评分在短时间内不会有太大的变化，用5日和21日进行分组，组内资产也不会有很大变化，尤其是对快要到期的分组来看更是如此。

故二者回测结果差别不大，后续如无特殊说明，回测以21日为准，给出的因子周期大约也就是在1个月左右。



**但是，很明显有改进方案。**

## Level1

五年和四年半的评分差距要小于一年和半年的评分差距。很明显，可转债初期，剩余时间对于评分的影响没有那么明显，而越到末期，对博弈评分的影响越明显。
通过对最终评分进行调整：

$$new = log_2(score + 1)$$

注意这里以2为底的理由：要保持最小值仍为0，最大值仍为1，只是修正评分的分布，若原始评分是均匀分布，新分布则向评分1聚集，这是符合金融学理解的，毕竟越是临近到期，评分的二次加速度越大。
这样的回测结果为：

![image](https://github.com/ruanjz6235/cbond/assets/51026474/ac315841-f43a-4d3e-84c4-56d1810e65ab)

可见，最高分组和次高分组都有所改进。


## Level2
但是我们仍不满足于此因子的回测效果，虽然我们知道它是凸的（加速度为正），但是并不知道加速度的变化情况。

从经济学含义上看：

1.凸性在合理范围内（不至于因为某一分组数量过少而产生扰动值，从而影响整体收益），因子随凸性的变大而表现更好；

2.凸性在同一水平附近，靠近1的加速度变化更快，理应因子表现更好。

我们想测试两种情况，一是因子与凸性强弱是否相关；二是因子的凸性向0聚集或向1聚集是否会影响到因子的表现。
![image](https://github.com/ruanjz6235/cbond/assets/51026474/bdf964a7-ba67-480d-abb9-431d3ec4d0bb)
![image](https://github.com/ruanjz6235/cbond/assets/51026474/4f52c27a-b03e-4585-86e3-a6857ff517eb)

上第一张图是$log_2(x+1)$和$log_10(9x+1)$，绿线是$log_2(x+1)$
上第二张图是$2x-x^2$和$log_10(9x+1)$，绿线是$2x-x^2$

**结果佐证了这一点。**

## 行为金融学1
在原始值排名的基础上，采用该式对评分进行转换，结果有所改进。

![image](https://github.com/ruanjz6235/cbond/assets/51026474/5db2ea9c-d10b-4e54-95d1-98c11562f9d6)
![image](https://github.com/ruanjz6235/cbond/assets/51026474/3f5d88fd-66a1-43d9-808e-6fcaa221d411)
![image](https://github.com/ruanjz6235/cbond/assets/51026474/415efa71-4683-4097-8e2f-eb76534eeebe)
![image](https://github.com/ruanjz6235/cbond/assets/51026474/8aee3cd7-23b1-41c0-a6a6-a1eaad8fdca4)

可见，在同样的聚集性情况下，自log4开始，因子整体表现逐步下滑。log2到log4因子表现持续上升。

## 行为金融学2

如果换一种分布聚集性，即分布向1聚集，结果改变较为明显。
当分布改为（注意，与该分布同等凸性的分布为log10）

![image](https://github.com/ruanjz6235/cbond/assets/51026474/c64c7585-daaa-4087-95d2-51209bb838ad)

可以获得分层十分明显的因子，这比log系列因子表现都更好，这证实了评分修正应向1聚集。

如果继续增加凸性，即可得圆锥曲线。

![image](https://github.com/ruanjz6235/cbond/assets/51026474/fcd52855-714d-4602-878f-9b6b401135c1)

本图的亮点：1. 第一次做出来收益最好的分组近年来收益持续向上，且收益达到了7；2. 非常罕见地出现，评分最高的分组收益反而最低，是否意味着最临近到期的可转债博弈价值反而会急剧下降呢？


也就是说，可转债的博弈评分机制需要进一步修正为：常规的博弈正向价值（随时间的减少博弈价值增加）+临近到期的博弈负向价值（临近到期博弈价值随时间的减少急剧降低），目前该部分尚未分析清楚。


**关于博弈价值随剩余时间的变化的研究，其实反映了在不知道真实分布的情况下，如何一步步逼近真实分布的数值分析过程。**
