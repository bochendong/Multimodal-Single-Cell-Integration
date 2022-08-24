# Meta data
- [Meta data](#meta-data)
- [Dataset Info](#dataset-info)
- [Data Prepareation](#data-prepareation)
- [Overview](#overview)
- [Analyze by day](#analyze-by-day)
- [Analyze by Donor](#analyze-by-donor)
- [Details](#details)
- [Summary](#summary)
  - [Multiome](#multiome)

# Dataset Info
从四个人身上采集8种不同的细胞，在第2，3，4，7，10天采集，并应用两种不同的技术来分析

* 四人的编号: 13176, 27678, 31800, 32606
* 八种细胞分别为
    * HSC: 造血干细胞
    * EryP: 巨核細胞（红细胞的祖细胞）
    * BP: B淋巴球（白血球中一種淋巴細胞的亞型）
    * NeuP: 造血干/祖细胞
    * MkP: 巨核-红系祖细胞（分化为巨核祖细胞与红系祖细胞）
    * MasP: 肥大细胞（机体侵入门户的哨兵）
    * MoP: 单核细胞前体
    * Hidden: 不可见数据
* 两种技术
    * CITEseq: measures gene expression (RNA) and surface protein levels.
    * Multiome: measures gene expression (RNA) and chromatin accessibility
    <div align=center>
        <img src ="./image/whole_process.jpg" width="800" height ="400"/>  
    </div>
* 数据采集
    * 训练数据（Training Set）
        * 训练数据只包含从13176， 31800和32606号实验体上采集的数据
        * 对于使用Multiome技术的数据，只从第2，3，4，7天采集
        * 对于使用CITEseq技术的数据，只从第2，3，4天采集
    * 可见测试数据（public test set）
        * 只包含从276786号实验体上采集的数据
        * 对于使用Multiome技术的数据，只从第2，3，7天采集
        * 对于使用CITEseq技术的数据，只从第2，3，4天采集
    * 不可见测试数据（private test set）
        * 包含从13176， 276786，31800和32606号实验体上采集的数据
        * 对于使用Multiome技术的数据，只从第10天采集
        * 对于使用CITEseq技术的数据，只从第7天采集
    * Note: There are no day 10 CITEseq samples in any split.
        
* 任务

    * Your task is to predict the labels corresponding to the inputs in the test set. To facilitate submission scoring, we only require predictions on a subset of the Multiome data. This subset was created by sampling 30% of the Multiome rows, and for each row, 15% of the columns. The sample of columns varies from row-to-row. All of the CITEseq labels are scored.

# Data Prepareation
数据的预处理阶段, 我们将原本数据的 **donor** 和 **technology** 进行了如下改变:

* donor :
    * 13176 -> 1
    * 27678 -> 2
    * 31800 -> 3
    * 32606 -> 4

* technology:
    * citeseq -> 0
    * multiome -> 1

# Overview
基于图表，可以得出：
<div align=center>
    <img src ="./image/1.1.png"/>  
</div>

* 细胞种类
    * Hidden 细胞全部使用multiome
    * 除去hidden， BP细胞（B淋巴球）使用multiome的比例最大，约为65%
    * EryP（巨核細胞）使用使用multiome的比例最小，约为43%
* 天数
    * 第十天全部使用multiome
    * 其他天数使用multiome与CITEseq的比例基本相同
* 实验体
    * 不同实验体使用multiome与CITEseq的比例基本相同
    
* 推测
    * 有可能第十天全为Hidden cell
    * 第三天multiome使用的BP细胞较多，CITEseq使用的BP细胞较少


# Analyze by day 

* Numerical plot
<div align=center>
    <img src ="./image/1.2.png"/>  
</div>

* Cell taken from diff cell type in diff day
  * 大部分细胞种类基本对称
  * BP细胞并不对称，也许是因为BP细胞较少导致的样本误差
<div align=center>
    <img src ="./image/1.4.png"/>  
</div>

# Analyze by Donor

- Percentage plot
  * 使用Citeseq的波峰明显高于Multiome
  * Technology = 0 (Citeseq): 不同实验体，在不同天数，被提取的细胞比例基本相同
  * Technology = 1 (Multiome): 第二号实验体在第四天未被提取细胞，其他均基本相同
<div align=center>
    <img src ="./image/1.3.png"/>  
</div>

- Cell taken from diff cell type in diff donor
  * Donor 2 所有用于Multiome的细胞种类均被隐藏
  * 除去donor 2， 只有BP细胞并不呈现对称
  * 也许可以通过比例推算hidden细胞的实际种类

<div align=center>
    <img src ="./image/1.5.png"/>  
</div>

# Details
Number of cells taken from each **donor** in each **day** 
* 对于每一个实验体， **CITEseq**: 
    * 数据都分布于5000-10000之间
    * day 4提取的细胞相对较多
    * day 7与day 3相对较少
* 对于每一个实验体， **Multiome**：
    * 数据分布于5000-15000之间
    * day 7相对较少
<div align=center>
        <img src ="./image/2.1.1.png"/>  
</div>

<div align=center>
    <img src ="./image/2.1.2.png"/>  
</div>

Number of cells taken from each **cell type** in each **day** 
* 对于每一种细胞种类，Citeseq
  * day 4提取的细胞数量较高
  * BP细胞最少，HSC细胞最多
  * 除去 MKP， MOP的day 7，day 7提取的其他细胞种类处于较低水平
* 对于每一种细胞种类，Multiome
  * day 4提取的细胞数量较高
  * BP细胞最少，HSC细胞最多
  * 除去HSC的day 3与 MasP， MKP， MOP的day 7，其他细胞的day 3 与 day 7 均处于较低水平
<div align=center>
        <img src ="./image/2.2.1.png"/>  
</div>

<div align=center>
    <img src ="./image/2.2.2.png"/>  
</div>

Number of cells taken from each **cell type** by each **donor** 
* CITEseq
<div align=center>
    <img src ="./image/2.3.1.png"/>  
</div>

* Multiome
<div align=center>
    <img src ="./image/2.3.2.png"/>  
</div>

# Summary

## Multiome
* 样本数据来自 Day 2，3，4，7，10
  * 来自 Day 10 的样本最多，细胞种类全部为 hidden
  * 来自 Day 4， 7 的细胞样本相对其他天数较少
  * Donor 2 在 Day 4 没有样本用于 Multiome，相比于其他天数，Donor 1，3， 4 在 Day 4 提取的细胞相对较多
* 样本数据来自 Donor 1，2，3，4
  * 来自 Donor 1 的样本最多
  * 来自 Donor 2 的样本最少 （因为Donor 2 在 Day 4 没有样本）
* 用于 Multiome 的细胞种类分布与总分布类似
  * HSC 最多
  * BP 最少
* Hidden 细胞只被用于 Multiome
  * Hidden 细胞于2，3，7，10天被提取
  * 来自 Donor 2 的 Hidden 细胞最多
* Donor 2，Hidden细胞， day 4 与 day 10 为数据集干扰项，考虑数据清洗与重构的可能性

