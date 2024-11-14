---
{"dg-publish":true,"permalink":"//blog/log//cfar/"}
---

[[实验室/实验室项目/雷达/雷达\|实验室/实验室项目/雷达/雷达]]
# 简介
# CFAR
## CFAR简介
CFAR（Constant False Alarm Rate）算法是一种常用的目标检测和跟踪算法。它的主要作用是在背景噪声中检测出目标信号，同时保证误检概率不变。
CFAR算法的基本思想是，对于每个雷达测量的数据点，以该点为中心，建立一个检测窗口，在该窗口内计算信号功率的平均值和方差，并将该窗口划分为若干个子窗口。然后，根据期望的误检概率和背景噪声的统计特性，计算出每个子窗口的阈值，用于判断该窗口内是否存在目标信号。

---

CFAR算法是目标检测的核心算法，由流程图可以看出，在进行2维fft之后，进行CFAR算法检测
![Pasted image 20240629145021.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240629145021.png)

---

CFAR算法的目标是设置阈值足够高以将虚警限制在可容忍的范围内，但又足够低以允许目标检测。（虚警是噪声被误认为是目标）也就是说CFAR是一个动态的实时调整判断阈值的算法，目的是能够尽可能的提高目标判断的准确度。

---
## CFAR窗
![Pasted image 20240629153928.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240629153928.png)


## 目标检测
根据CFAR窗，可以计算出门限，如果信号功率超过门限，则认为目标存在，反之则不存在，这里用<font color=yellow>xy师兄的测试图来做说明</font>。

## CA-CFAR
示意图如下
![Pasted image 20240629153712.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240629153712.png)

  通过示意图可以看出来，CFAR的思路就是，先根据前置窗和后置窗算出噪声平均功率，然后与门限因子相乘，得到门限值，之后和被测单元对比，被测单元的功率值大于门限则检测目标存在，反之则不存在。
$$
其中Z_{CA}是噪声功率，公式如下：
$$
$$
Z_{CA}  = 0110frac{1}{N_{train} }\sum_{i=1}^{N_{train} }X_{i}  
$$
$$
其中T_{CA}是门限因子，计算公式如下

$$
$$
T_{CA}  = {N_{train} }(P_{fa}^{-\frac{1}{{N_{train} }} } -1)
$$
## GOCA
![Pasted image 20240629160434.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240629160434.png)

示意图表示，分别将前置窗和后置窗的平均功率计算出来，然后比较取大，再与门限因子相乘得到门限。

好处：可以避免杂波边缘的虚警。杂波下 CA-CFAR 反应较晚，因为它采用整个训练单元范围的平均值，而 GOCA-CFAR 则采用最多一半的训练单元。

> [!NOTE] 问题
> 为什么能够避免杂波边缘的虚警？

## 2D CFAR
### 原理


2D CFAR的检测对象通常是距离-多普勒图像（RDM），其仿真代码链接为：https://link.zhihu.com/?target=https%3A//github.com/tooth2/2D-CFAR
示意图如下
![Pasted image 20240629161412.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E6%96%87%E4%BB%B6/Pasted%20image%2020240629161412.png)

matlab参考代码如下[[日常/BLOG/学习log/雷达/雷达Cfar+测距测速测角--maltab\|雷达Cfar+测距测速测角--maltab]]
![[实验室/文件/radar-target-generation-and-detection.m]]

#### 实际仿真log
算出RDM之后，观察RDM的3D图，可以发现，在doppler维，目标大概占6格，range维度大概占20格，由此可得保护单元的大小6 * 20。由于担心把相近的目标列进训练单元。所以训练单元就往外扩一圈，也就是8 * 22。
![Pasted image 20241105202725.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241105202725.png)
![Pasted image 20241105202529.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241105202529.png)
但是仍然不好
train_range = 1;
train_doppler = 1;
guard_range = 10;
guard_doppler = 3;
![Pasted image 20241106104306.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241106104306.png)

train_range = 1;
train_doppler = 2;
guard_range = 6;
guard_doppler = 12;
![Pasted image 20241106104127.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241106104127.png)
扩大训练单元
train_range = 4;
train_doppler = 8;
guard_range = 10;
guard_doppler = 3;
![Pasted image 20241106104443.png](/img/user/%E5%AE%9E%E9%AA%8C%E5%AE%A4/%E5%AE%9E%E9%AA%8C%E5%AE%A4%E9%A1%B9%E7%9B%AE/%E9%9B%B7%E8%BE%BE/%E7%90%86%E8%AE%BA/Pasted%20image%2020241106104443.png)

> [!NOTE] 问题
> 如果用正常2d_cfar，在多目标监测时，会可能把相近的目标纳入训练单元。

# 参考文献
【1】Heijne, Rainier. Comparing Detection Algorithms for Short Range Radar based on the use-case of the Cobotic-65

【2】James J. Jen. A STUDY OF CFAR IMPLEMENTATION COST AND PERFORMANCE TRADEOFFS IN HETEROGENEOUS ENVIRONMENTS

【3】MATTHIAS KRONAUGE. Fast Two-Dimensional CFAR Procedure

【4】[https://la.mathworks.com/help/phased/ug/constant-false-alarm-rate-cfar-detection.html](https://link.zhihu.com/?target=https%3A//la.mathworks.com/help/phased/ug/constant-false-alarm-rate-cfar-detection.html)

【5】Mark Richards,_Fundamentals of Radar Signal Processing_, McGraw Hill, 2005

【6】[https://zhuanlan.zhihu.com/p/652220176](https://zhuanlan.zhihu.com/p/652220176)

【7】https://zhuanlan.zhihu.com/p/508870274