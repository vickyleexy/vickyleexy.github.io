---
title: '''Basics of Bioinformatics'''
date: 2017-07-13 17:27:09
tags:

---

## 生物信息学基础


### lncRNA:

　　长链非编码RNA（Long Noncoding RNA，LncRNA）指的是长度在200nt以上、不编码蛋白、但参与细胞内多种生物学过程的RNA分子。人类基因组计划研究发现只有3%的基因组序列是编码蛋白质的基因，而占人基因组62%的序列转录为lncRNA，这一结论提示非编码区域可能通过表达lncRNA，活跃地参与到生物学功能的调控中。在过去的十几年中，科学家们已经相继发现，lncRNA参与了X染色体沉默，染色质修饰、转录激活、转录干扰、核内运输等多种重要的调控过程，截至目前，在NONCODE中已经收录了73370个lncRNA，它们分别来自1239个物种，仅有不到200个进行了功能注释，人类对lncRNA的研究还知之甚少。随着对lncRNA在哺乳动物进化及人类疾病发生发展中作用的日益关注，lncRNA调控机制已成为当前生命科学研究的新热点。

**LncRNA在生物体内的功能主要分为三大类：**

**生物学功能：**LncRNA与表观遗传调控、转录调控、转录后调控、 miRNA 调控、细胞分化及发育等密切相关；

**应急功能：**LncRNA可作为细胞内各种信号招募蛋白形成复合物参与免疫反应和宿主防御。

**LncRNA与疾病：**LncRNA与人类的许多疾病，尤其是与衰老相关的疾病有密切关系，例如心血管疾病、阿尔兹海默症、糖尿病、癌症等。

因此，lncRNA未来能否作为分子靶标成功应用于临床诊断和癌症治疗，将是其日后发展的难点与热点。


### lncRNA-mRNA 整合分析
1. **LncRNA简要：**
　　LncRNA是一类转录本长度超过200nt的RNA，它们本身并不编码蛋白，而是以RNA的形式在多种层面上（表观遗传调控、转录调控以及转录后调控等）调控基因的表达水平。生物体内含量相相当丰富，约占RNA的4-9%（mRNA约占1-2%）。LncRNA的组织特异性及特定的细胞定位，显示lncRNA受到高度严谨的调控，目前已知其与发育、干细胞维持、癌症及一些疾病相关。虽然近年来随着基因芯片及第二代高通量测序技术的广泛运用，lncRNA不断被发现，但此类转录本的确切功能还未知。目前市场上的lncRNA芯片通常将lncRNA与mRNA设计在一起，RNASeq数据中也包含lncRNA, mRNA序列，因此可以通过分析lncRNA与mRNA表达相关性对lncRNA进行功能注释。
2. **分析流程图：**
![](http://i.imgur.com/31PHHQH.jpg)
3. **分析内容：**  
①计算LncRNA与mRNA表达相关性，根据设定的域值筛选lncRNA与mRNA关系对，构建LncRNA与mRNA共表达网络，如下是全局网络  
![这里写图片描述](http://img.blog.csdn.net/20170705122150223?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
②基于lncRNA与mRNA表达相关性以及lncRNA与mRNA基因组位置近邻关系，得到lncRNA的潜在靶标基因，对差异表达的lncRNA靶标基因进行功能注释以及功能富集分析，如下是功能富集的GO的Barplot图和差异lncRNA的Heatmap图。  
![这里写图片描述](http://img.blog.csdn.net/20170705122224470?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170705122236976?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
③研究lncRNA与mRNA的共表达网络的拓扑学特性，基于度筛选网络拓扑上重要的lncRNA，这些lncRNA极有可能是与研究背景相关的lncRNA，如下是重要lncRNA与mRNA的局部共表达子网络。
![这里写图片描述](http://img.blog.csdn.net/20170705122329682?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

④客户提供研究背景相关一组基因，根据表达相关性可以找出与这组基因相关的lncRNA，从而构建出感兴趣的共表达网络。通过构建的共表达网络能进一步找到感兴趣的 hub  lncRNA。

###**lncRNA深度挖掘分析**
**一、差异lncRNA靶基因预测**  
lncRNA的靶基因较为复杂,主要分为正式和反式两种作用机制.lncRNA作用机制与miRNA类似,均可以通过调控相应的mRNA来行使功能,所以靶基因的预测在科学研究中都显得非常必要。  
**二、靶基因Gene Ontology分析**  
我们将靶基因向gene ontology数据库的各节点映射,计算每个节点的基因数目.  
**三、靶基因Pathway分析**  
信号通路分析需要完备的注释信息支持，通过整合KEGG、Biocarta、Reactome等多个数据库的信息可以精确检验来进行Pathway的显著性分析。  
**四、lncRNA与调控基因的表达机制**  
通过整合lncRNA的信息和靶基因之间的关系,我们可以得到一个lncRNA与靶基因之间的调控网络图.  
**五、 转录因子结合位点预测**  
对于差异表达lncRNA，提取转录起始位点上下游序列,使用预测程序对其转录因子结合位点进行预测.  
**六、基因关联分析**    
现在市面上的lncRNA芯片均含有mRNA的表达探针,通过将lncRNA的靶基因分析结果与芯片上mRNA的表达结果做关联分析,可以更进一步的分析lncRNA的功能。  
**七、信号通路调控网络构建：**  
实验中基因同时参与了很多Pathway，通过构建信号通路调控网络，从宏观层面看到Pathway之间的信号传递关系，在多个显著性Pathway中发现受实验影响的核心Pathway，以及实验影响的信号通路之间的调控机理。  
**八、lncRNA的功能分析**  
根据lncRNA最新的功能数据库，利用生物信息学工具，做出Function-Tar-Net图表，从而得出lncRNA与功能的关系
 
###**lncRNA功能实验**
1. LncRNA定量PCR
2. LncRNA原位杂交
3. 5’-RACE, 3’-RACE实验(lncRNA全长扩增实验)
4. lncRNA干扰实验
5. lncRNA过表达实验
需要先通过5’-RACE实验找到lncRNA转录起始位点
6. lncRNA细胞功能实验
细胞增殖、细胞凋亡、细胞周期、细胞迁移

### 基因丰度和基因表达丰度
**基因丰度**是指基因组中该基因的拷贝数量。
基因丰度高，即这个基因的数量多，那么可能这个基因的表达量也会多，但是不一定，主要还是要看该基因的启动子强弱。所以基因丰度高不代表表达丰度也高。

**基因表达的丰度**是指基因转录成mRNA的数量。
基因表达丰度高是指该基因转录成mRNA多，那么表达的蛋白也多，对于表型的影响就大。

(基因丰度是某个基因在基因组中的总数量，其中有的能表达，有的不能表达；而能被表达出来的基因占该基因的总数的比例就是该基因的表达丰度。)