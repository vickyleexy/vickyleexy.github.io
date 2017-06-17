此笔记根据《machine learning in action》和周志华教授的《机器学习》所作。

## 决策树的原理
### **决策树的构造**
- **优点：**计算复杂度不高，输出结果易于理解，对中间值的确实不敏感，可以处理不相关特征数据。
- **缺点：**可能会产生过度匹配问题。
- **适用数据类型：**数值型和标称型

《machine learning  in action》:
```
If so return 类标签；
Else
    寻找划分数据集的最好特征
    划分数据集
    创建分支节点
       for每个划分的子集
          调用函数createBranch并增加返回结果到分支节点中
    return 分支节点
```
上面的伪代码createBranch是一个递归函数，在倒数第二行调用了它自己。
《机器学习》：
![这里写图片描述](http://img.blog.csdn.net/20170602222239103?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
###**决策树的一般流程**
1. **收集数据：**可以使用任何方法。
2. **准备数据：**树构造算法只适用于**标称型数据**（标称型目标变量的结果只在有限目标集中取值，如真与假[标称型目标变量主要用于分类]）,因此数值型数据必须离散化。
3. **分析数据：**可以使用任何方法，构造书完成之后，我们应该检查图形是否符合预期。
4. **训练算法：**构造树的数据结构。
5. **测试算法：**使用经验树计算错误率。


**决策树的变量可以有两种：**

1.  数字型（Numeric）：变量类型是整数或浮点数，如“年收入”。用“>=”，“>”,“<”或“<=”作为分割条件（排序后，利用已有的分割情况，可以优化分割算法的时间复杂度）。

2. 名称型（Nominal）：类似编程语言中的枚举类型，变量只能重有限的选项中选取，比如“婚姻情况”，只能是“单身”，“已婚”或“离婚”。使用“=”来分割。

**一些决策树算法采用二分法划分数据，本文并不采用这种方法，而采用*ID3算法*。**

### **熵的推导**
参考PRML：
熵的含义：
考虑一个集合，包含N个完全相同的物体，这些物体要被分到m个箱子里，使得第i个箱子中有$n_i$ 个物体。考虑把物体分配到箱子中的不同方案的数量，有N种方式选择第一个物体，有（N-1）种方式选择第二个物体，以此类推。因此总共有N！种方式把N个物体分配到箱子中（也可以如此考虑：将N个物体先进行全排列，然后选前$n_1$个放入第1个箱子，选前$n_2 $个放入第2个箱子，依次类推，则放入箱子的方案有N！种）。然而，我们并不想区分每个箱子内部物体的排列。所以上述的方案数量需要除以每个箱子内部排列的数量，即在第i个箱子中，有$n_i！$种方案对物体进行全排列，因此，把N个箱子分配到箱子中的总方案数量为：
<center>
<img src="http://img.blog.csdn.net/20170602233318436?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="30%" height="30%"  />
</center>
这被称为乘数。
熵被定义为通过适当的参数放缩后的对数乘数，即
<center>
<img src="http://img.blog.csdn.net/20170602234657912?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="20%" height="20%"  />
</center>
在下面的推导中，将会用到Stirling公式的估计：
![这里写图片描述](http://img.blog.csdn.net/20170603110124302?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170603110027505?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
**熵的推导：**
![这里写图片描述](http://img.blog.csdn.net/20170603112104061?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

决策树算法中比较有代表性的是ID3[Quinlan,1979,1986]，C4.5[Quinlan,1993]和CART[Breiman et al.,1984]
### **ID3算法**
维基百科解释：https://en.wikipedia.org/wiki/ID3_algorithm
ID3是以“信息增益”为准则来选择划分属性的。
![这里写图片描述](http://img.blog.csdn.net/20170604211002327?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### **C4.5算法**
C4.5算法主要以“信息率”来选择最优划分属性。
![这里写图片描述](http://img.blog.csdn.net/20170604211626464?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170604211640230?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### **CART算法**
CART决策树是以“基尼指数”为准则来选择划分属性。此算法不仅可用于分类还可以用于回归。
![这里写图片描述](http://img.blog.csdn.net/20170604212130490?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


## 决策树的构建

**《 machine learning in action》之决策树**
### **计算给定数据集的香农熵**
创建文件trees.py
```
# -*- coding=utf-8 -*-
#计算给定数据集的熵
from math import log

def calcShannonEnt(dataSet):
    numEntries = len(dataSet)
    labelCounts = {}  #实例总数
    for featVec in dataSet:
        #为所有可能分类创建字典
        currentLabel = featVec[-1]       #将最后一列，即分类结果存入currentLabel
        if currentLabel not in labelCounts.keys():       #若分类结果已经在labelCounts这个字典中
            labelCounts[currentLabel] = 0              #若当前label不存在,则扩展字典,加入此键值
        labelCounts[currentLabel] += 1                 #字典中的每个键值都记录了当前类别的数量
    #print 'labelCounts,currentLabel:',labelCounts,currentLabel
    #print 'labelCounts.keys()',labelCounts.keys()
    #print 'labelCounts.values()',labelCounts.values()
    shannonEnt = 0.0
    for key in labelCounts:
        prob = float(labelCounts[key])/numEntries
        shannonEnt -= prob*log(prob,2)  #求以2为底的对数，其和即为熵
        #print 'labelCounts[key]',labelCounts[key]
    return shannonEnt
```
### **创建或导入dataSet**
```
#鱼类数据集
def createDataSet():
    dataSet = [[1,1,'yes'],
               [1,1,'yes'],
               [1,0,'no'],
               [0,1,'no'],
               [0,1,'no']]
    labels = ['no surfacing','flippers']
    return dataSet,labels
```

**在命令提示符下输入：**
```
In[9]: import trees
In[10]: myDat,labels = trees.createDataSet()
In[11]: myDat
Out[11]: 
[[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
In[12]: labels
Out[12]: 
['no surfacing', 'flippers']
In[13]: trees.calcShannonEnt(myDat)
#混合的数据越多，熵越高，测试：
In[18]: myDat[0][-1] = 'maybe'
In[19]: myDat
Out[19]: 
[[1, 1, 'maybe'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
In[20]: trees.calcShannonEnt(myDat)
Out[20]: 
1.3709505944546687
```

### **划分数据集**
取出符合要求某个特征属性值的样本，并将其此特征从数据集中去除。
```
#按照给定特征划分数据集
def splitDataSet(dataSet,axis,value):  #待划分的数据集,划分数据集的特征(第几列),需要返回的特征的值，（axis-->a,value-->v）
    #为了不修改原始数据集,创建新的list对象
    retDataSet = []
    for featVec in dataSet:
        #将符合特征的数据抽取出来
        if featVec[axis] == value:  #Python中的数据.即featVec中的数据从第0列开始,[0,1,2,3,4,5,……]
            reducedFeatVec = featVec[:axis] #[:axis]表示前axis行,若axis为3,则表示取festVec的前3列,即第[0,1,2]列
            reducedFeatVec.extend(featVec[axis+1:])    #[axis+1:] 表示跳过axis+1列,从下一列数据取到最后一列的数据,即跳过第[3]列,从第[4]列到最后一列
            retDataSet.append(reducedFeatVec)
    return retDataSet
```
**在Python命令行中输入：**
```
In[16]: reload(trees)
Out[16]: 
<module 'trees' from '/home/vickyleexy/PycharmProjects/Classification of contact lenses/trees.py'>
In[17]: myDat,labels = trees.createDataSet()
In[18]: myDat
Out[18]: 
[[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
In[19]: trees.splitDataSet(myDat,0,1) #抽取出数据集中第0列中数据值为1的样本，并将此列剔除，组成新的样本集
Out[19]: 
[[1, 'yes'], [1, 'yes'], [0, 'no']]
In[20]: trees.splitDataSet(myDat,0,0) #抽取出数据集中第0列中数据值为0的样本，并将此列剔除，组成新的样本集
Out[20]: 
[[1, 'no'], [1, 'no']]
In[21]: trees.splitDataSet(myDat,1,0) #抽取出数据集中第1列中数据值为0的样本，并将此列剔除，组成新的样本集
Out[21]: 
[[1, 'no']]
```

### **选取最好的数据集划分方式**
即选取取出信息增益最大的特征。**信息增益**是熵减少或者说信息无序度的减少，信息增益越大，信息无序度减少越大，信息的可确定性越强。
```
#选择最好的数据集划分数据,即计算每种特征信息熵 Gain(D,a)
def chooseBestFeatureToSplit(dataSet):
    numFeatures = len(dataSet[0]) - 1                     #可选特征的个数
    baseEntropy = calcShannonEnt(dataSet)  # 计算总体信息熵Ent(D)
    bestInfoGain = 0.0;       #初始化最好的信息增益
    bestFeature = -1;        #初始化选取的最好的特征
    #创建唯一的分类标签列表
    for i in range(numFeatures):           #遍历各个特征
        featList = [example[i] for example in dataSet]   #将第i列特征的值选取出来,并存入featList
        uniqueVals = set(featList)         #将featList存为集合的格式,即去除featList中重复的元素,因此uniqueVals中为每个特征的中不同的属性
        newEntropy = 0.0
        #计算每种划分方式的信息熵
        for value in uniqueVals:        #遍历每个特征中的各个属性
            subDataSet = splitDataSet(dataSet,i,value)       #选取符合条件的特征,并将此特征从样本中去除，以便进行下面的进一步计算
            prob = len(subDataSet)/float(len(dataSet))       #符合此特征中此属性的个数占总体样本的比例,即|D^v|/|D|
            newEntropy += prob * calcShannonEnt(subDataSet)  #计算各个特征中每个属性的加权信息熵的和
        infoGain = baseEntropy - newEntropy               #计算信息增益,即Gain(D,a)
        #计算最好额信息增益
        if (infoGain > bestInfoGain):
            bestInfoGain = infoGain
            bestFeature = i
    print '最好的特征,最好的信息增益：',bestFeature,bestInfoGain
    return bestFeature

```

**在Python命令行下输入以下代码进行测试：**
```
In[9]: reload(trees)
Out[9]: 
<module 'trees' from '/home/vickyleexy/PycharmProjects/Classification of contact lenses/trees.py'>
In[10]: myDat,labels = trees.createDataSet()
In[11]: myDat
Out[11]: 
[[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
In[12]: trees.chooseBestFeatureToSplit(myDat)
最好的特征,最好的信息增益： 0 , 0.419973094022
Out[12]: 
0
```
### **递归构建决策树**
递归结束的条件：

-  程序遍历完所有划分数据集的属性
-  每个分支下的所有实例都具有相同的分类

若已经处理了所有的属性,但类标签依然不是唯一的,此时采用多数表决的方法决定叶子节点的分类。
在trees.py文件的顶部添加operator库
```
import operator
```
统计剩余样本中属于哪一类别最多的数量最多：
```
def majorityCnt(classList):     #classList类别列表
    classCount = {}             #新建立一个字典
    for vote in classList:       #遍历所有的类别
        #统计各类别剩余样本的数量
        if vote not in classCount.keys():     #如果此类别不在classCount的key中
            classCount[vote] = 0
            classCount[vote] += 1
    sortedClassCount = sorted(classCount.iteritems(),key = operator.itemgetter(1),reverse = True) #将统计好的剩余样本类别和其数量，根据其数量进行从大到小的排序
    return sortedClassCount[0][0]     #取出剩余样本中数量最大的类别名称
```
**创建树的函数代码：**
```
#创建树的函数代码
def createTree(dataSet,labels):
    classList = [example[-1] for example in dataSet]
    #类别完全相同则停止继续划分
    if classList.count(classList[0]) == len(classList):  #若classList中第一个类别的数量等于样本的总数量,即样本类别完全相同
        return classList[0]
    #遍历完所有特征时返回出现次数最多的
    if len(dataSet[0]) == 1:
        return majorityCnt(classList)
    bestFeat = chooseBestFeatureToSplit(dataSet)
    bestFeatLabel = labels[bestFeat]
    myTree = {bestFeatLabel:{}}
    del(labels[bestFeat]) #去掉已经使用的（bestFeat）类标签
    #得到最好的特征这一列,包含其所有属性值的列表
    featValues = [example[bestFeat] for example in dataSet]
    uniqueVals = set(featValues)        #去掉featValues列表中重复的属性
    for value in uniqueVals:             #遍历最好的特征中所有的属性
        subLabels = labels[:]            #复制del(labels[bestFeat])后的结果,并将其存在新列表变量subLabels中
        myTree[bestFeatLabel][value] = createTree(splitDataSet(dataSet,bestFeat,value),subLabels)
    return myTree

```

命令行中输入：
```
In[66]: reload(trees)
Out[66]: 
<module 'trees' from '/home/vickyleexy/PycharmProjects/Classification of contact lenses/trees.py'>
In[67]: myDat,labels = trees.createDataSet()
In[68]: myTree = trees.createTree(myDat,labels)
最好的特征,最好的信息增益： 0 , 0.419973094022
最好的特征,最好的信息增益： 0 , 0.918295834054
In[69]: myTree
Out[69]: 
{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}}}
```

## 使用matplotlib绘制树形图并测试算法

在[**决策树02——决策树的构建**](http://blog.csdn.net/u012150360/article/details/72868619)中，我们将已经进行分类的数据存储在字典中，然而字典的表示形式非常不直观，也不容易理解，所以我们将字典中的信息绘制成树形图。
### **Matplotlib注解功能**
　　Matplotlib提供一个注解工具annotations，它可以在数据图形上添加文本注释。

　　以下将使用Matplotlib的注解功能绘制树形图，它可以对文字着色，并提供多种形状以供选择，而且我们还可以反转箭头，将它指向文本框而不是数据点。

　　新建名为treeplotter.py的新文件，将输入下面的程序代码：
```
# -*-coding=utf-8 -*-

#使用文本朱姐绘制树节点
import matplotlib.pyplot as plt

#定义文本框和箭头格式
#定义决策树决策结果的属性（决策节点or叶节点）,用字典来定义
#下面的字典定义也可以写作 decisionNode = {boxstyle：’sawtooth‘,fc=’0.8‘}
decisionNode = dict(boxstyle = "sawtooth", fc = "0.8")       #决策节点,boxstyle为文本框类型,sawtooth是锯齿形,fc是边框内填充的颜色
leafNode = dict(boxstyle = "round",fc="0.8")                #叶节点,定义决策树的叶子结点的描述属性
arrow_args = dict(arrowstyle = "<-")                         #箭头格式

#绘制带箭头的注释
def plotNode(nodeTxt,centerPt,parentPt,nodeType):           #nodeTxt是显示的文本,centerPt是文本的中心点,parentPt是箭头的起点坐标,nodeType是一个字典 注解的形状
    createPlot.ax1.annotate(nodeTxt,xy = parentPt, xycoords = 'axes fraction',  #xy为箭头的起始坐标,0,0 is lower left of axes and 1,1 is upper right
                            xytext = centerPt,textcoords = 'axes fraction', #xytext为注解内容的坐标
                            va = "center",ha = "center",bbox = nodeType,arrowprops = arrow_args) #bbox注解文本框的形状,arrowprops是指箭头的形状

def createPlot():
    fig = plt.figure(1,facecolor='white')  #类似于matlab的figure,定义一个画布,其背景为白色
    fig.clf()                 #把画布清空
    createPlot.ax1 = plt.subplot(111,frameon=False) # createPlot.ax1为全局变量,绘制图像的句柄,subplot为定义了一个绘图,111表示figure中的图有1行1列,即1个,最后的1代表第一个图,
    plotNode(U'决策节点',(0.5,0.1),(0.1,0.5), decisionNode)
    plotNode(U'叶节点',(0.8,0.1),(0.3,0.8), leafNode)
    plt.show()
```
**注意：以上程序运行时会出现中文变成小方框的现象，将以下几行代码添加到文件的开始处。**
```
from pylab import *
mpl.rcParams['font.sans-serif'] = ['SimHei']  #指定默认字体
mpl.rcParams['axes.unicode_minus'] = False
```
**在命令行输入：**
```
In[70]: import treePlotter
Backend TkAgg is interactive backend. Turning interactive mode on.
In[71]: treePlotter.createPlot()
```
![这里写图片描述](http://img.blog.csdn.net/20170614231531328?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 构造注解树
　　我们虽然有x, y坐标，但是如何放置所有的树节点却是个问题。我们必须知道有多少个叶节点，以便可以正确确定x轴的长度，我们还需要知道树有多少层，以便可以正确的确定y轴的高度。
　　这里我们定义两个新函数getNumLeafs()和getTreeDepth()，来获取叶节点的输煤和树的层数。将下面的两个函数添加到treePlotter.py文件中。
```
#获取叶节点的数目和树的层次
def getNumLeafs(myTree):
    numLeaf = 0
    firstStr = myTree.keys()[0]
    secondDict = myTree[firstStr]
    for key in secondDict.keys():
        if type(secondDict[key]).__name__ =='dict':         #测试节点的数据类型是否为字典 ,type(secondDict[key]) ==dict 也是可以的
            numLeaf += getNumLeafs(secondDict[key])
        else: numLeaf += 1
    return numLeaf

def getTreeDepth(myTree):
    maxDepth = 0
    firstStr = myTree.keys()[0]
    secondDict = myTree[firstStr]
    for key in secondDict.keys():
        if type(secondDict[key]).__name__ == 'dict':
            thisDepth = 1 + getTreeDepth(secondDict[key])
        else: thisDepth = 1
        if thisDepth > maxDepth :maxDepth = thisDepth
    return maxDepth
```
函数retrieveTree()输出预先存储的树信息，将 下面代码添加到文件treePlotter.py中：
```
def retrieveTree(i):
    listOfTrees = [{'no surfacing':{0:'0',1:{'flippers':{0:'no',1:'yes'}}}},
                   {'no surfacing': {0: '0', 1: {'flippers': {0: {'head':{0:'no',1:'yes'}}, 1: 'no'}}}}
                   ]
    return listOfTrees[i]
```

**在命令行中输入：**
```
In[2]: import treePlotter
Backend TkAgg is interactive backend. Turning interactive mode on.
In[3]: treePlotter.retrieveTree(0)
Out[3]: 
{'no surfacing': {0: '0', 1: {'flippers': {0: 'no', 1: 'yes'}}}}
In[4]: myTree = treePlotter.retrieveTree(0)
In[5]: treePlotter.getNumLeafs(myTree)
Out[5]: 
3
In[6]: treePlotter.getTreeDepth(myTree)
Out[6]: 
2
```

将下面代码添加到treePlotter.py中，注意前面已经定义了createPlot()，此时我们需要更新前面的代码。
```
#plotTree函数
#在父子节点间填充文本信息
def plotMidText(cntrPt,parentPt,txtString):
    xMid = (parentPt[0] - cntrPt[0])/2.0 + cntrPt[0]
    yMid = (parentPt[1] - cntrPt[1])/2.0 + cntrPt[1]
    createPlot.ax1.text(xMid,yMid,txtString)

#自顶向下作图,绘制图形的x轴有效范围是0.0～1.0, y轴有效范围也是0.0~1.0
def plotTree(myTree,parentPt,nodeTxt):
    numLeafs = getNumLeafs(myTree)    #secondDict[key]的叶节点的数量
    depth = getTreeDepth(myTree)      #secondDict[key]的树深度
    print 'numLeafs,depth:',numLeafs,',',depth
    firstStr = myTree.keys()[0]
    # 全局变量plotTree.totalW 存储树的宽度，全局变量PlotTree.totalD 存储树的深度，使用这两个变量计算树节点的摆放位置，这样可以将树绘制在水平方向和垂直方向的中心位置。
    cntrPt = (plotTree.xOff +(1.0 + float(numLeafs))/2.0/plotTree.totalW,plotTree.yOff) #注释1
    #标记子节点属性
    plotMidText(cntrPt,parentPt,nodeTxt)        #这一次循环中的cntrPt(即上式)为cbtrPt,parentPt为上一轮计算出的cntrPt
    plotNode(firstStr,cntrPt,parentPt,decisionNode)  #因还没画到叶节点,所以这里画的是决策节点,即此时筛选secondDict[key]还是字典
    secondDict = myTree[firstStr]
    #计算下一轮要用的y
    plotTree.yOff = plotTree.yOff - 1.0/plotTree.totalD    #下面的循环中要使用的y
    for key in secondDict.keys():
        if type(secondDict[key]).__name__ == 'dict':
            plotTree(secondDict[key],cntrPt,str(key))
        else:
            plotTree.xOff = plotTree.xOff + 1.0/plotTree.totalW
            plotNode(secondDict[key],(plotTree.xOff,plotTree.yOff),cntrPt,leafNode)
            plotMidText((plotTree.xOff,plotTree.yOff),cntrPt,str(key))
    plotTree.yOff = plotTree.yOff + 1.0/plotTree.totalD #注释2

def createPlot(inTree):
    fig = plt.figure(1, facecolor='white')
    fig.clf()
    axprops = dict(xticks=[],yticks=[])    #创建一个型为{'xticks': [], 'yticks': []}的字典
    createPlot.ax1 = plt.subplot(111,frameon=False,**axprops)
    plotTree.totalW = float(getNumLeafs(inTree))      #树的宽度
    plotTree.totalD = float(getTreeDepth(inTree))     #输的深度
    plotTree.xOff = -0.5/plotTree.totalW; plotTree.yOff = 1.0; #定义节点位置的初始值
    plotTree(inTree,(0.5,1.0),'')     #（0.5,1.0）为初始化parentPt的值，注释3
    plt.show()
```
在命令行输入：
```
In[35]: reload(treePlotter)
Out[35]: 
<module 'treePlotter' from '/home/vickyleexy/PycharmProjects/Classification of contact lenses/treePlotter.py'>
In[36]: myTree = treePlotter.retrieveTree(0)
In[37]: myTree
Out[37]: 
{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}}}
In[38]: treePlotter.createPlot(myTree)
numLeafs,depth: 3 , 2
numLeafs,depth: 2 , 1
```
![这里写图片描述](http://img.blog.csdn.net/20170615230521666?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**注释：**
**1.**`cntrPt = (plotTree.xOff + (1.0 + float(numLeafs))/2.0/plotTree.totalW, plotTree.yOff)`
　　在这行代码中，首先由于整个画布根据叶子节点数和深度进行平均切分，并且x轴的总长度为1,即如同下图：
![这里写图片描述](http://img.blog.csdn.net/20170615230919401?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　其中方形为非叶子节点的位置，@是叶子节点的位置，因此每份即上图的一个表格的长度应该为1/plotTree.totalW,但是叶子节点的位置应该为@所在位置，则在开始的时候plotTree.xOff的赋值为-0.5/plotTree.totalW,即意为开始x位置为第一个表格左边的半个表格距离位置，这样作的好处为：在以后确定@位置时候可以直接加整数倍的1/plotTree.totalW,

 　　plotTree.xOff即为最近绘制的一个叶子节点的x坐标，在确定当前节点位置时每次只需确定当前节点有几个叶子节点，因此其叶子节点所占的总距离就确定了即为float(numLeafs)/plotTree.totalW*1(因为总长度为1)，因此当前节点的位置即为其所有叶子节点所占距离的中间即一半为float(numLeafs)/2.0/plotTree.totalW*1，但是由于开始plotTree.xOff赋值并非从0开始，而是左移了半个表格，因此还需加上半个表格距离即为1/2/plotTree.totalW*1,则加起来便为(1.0 + float(numLeafs))/2.0/plotTree.totalW*1，因此偏移量确定，则x位置变为plotTree.xOff + (1.0 + float(numLeafs))/2.0/plotTree.totalW.

**2.** `plotTree.yOff = plotTree.yOff + 1.0/plotTree.totalD`  
这行代码中是需要的，当分支最后一个不是字典的时候,字典循环完需要返回上一层继续进行函数
例如：
```
In[40]: myTree['no surfacing'][3] = 'maybe'
In[41]: myTree
Out[41]: 
{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}, 3: 'maybe'}}
In[42]: treePlotter.createPlot(myTree)
numLeafs,depth: 4 , 2
numLeafs,depth: 2 , 1
```
![这里写图片描述](http://img.blog.csdn.net/20170615231547780?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE1MDM2MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
    　
**3.**`plotTree(inTree,(0.5,1.0),'') `
  在这行代码中，对于plotTree函数参数赋值为(0.5, 1.0)，因为开始的根节点并不用划线，因此父节点和当前节点的位置需要重合，利用2中的确定当前节点的位置便为(0.5, 1.0)

**总结：**利用这样的逐渐增加x的坐标，以及逐渐降低y的坐标能能够很好的将树的叶子节点数和深度考虑进去，因此图的逻辑比例就很好的确定了，这样不用去关心输出图形的大小，一旦图形发生变化，函数会重新绘制，但是假如利用像素为单位来绘制图形，这样缩放图形就比较有难度了

### 测试和存储分类器
程序比较测试数据与决策树上的数值，递归执行该过程直到进入叶子节点，最后将测试数据定义为叶子节点所属的类型。

```
#使用决策树的分类算法
def classify(inputTree,featLabels,testVec):    #testVec即为需要分类的数据
    firstStr = inputTree.keys()[0]
    secondDict = inputTree[firstStr]
    featIndex = featLabels.index(firstStr)        #将标签字符串转换为索引
    print featIndex
    for key in secondDict.keys():
        if testVec[featIndex] == key:
            if type(secondDict[key]).__name__ == 'dict':
                classLabel = classify(secondDict[key],featLabels,testVec)
            else:
                classLabel = secondDict[key]
    return classLabel

```
**在命令行输入：**
```
In[19]: reload(trees)
Out[19]: 
<module 'trees' from '/home/vickyleexy/PycharmProjects/Classification of contact lenses/trees.py'>
In[20]: myDat,labels = trees.createDataSet()
In[21]: myDat
Out[21]: 
[[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
In[22]: labels
Out[22]: 
['no surfacing', 'flippers']
In[23]: myTree = trees.createTree(myDat,labels)
最好的特征,最好的信息增益： 0 , 0.419973094022
最好的特征,最好的信息增益： 0 , 0.918295834054
In[24]: myDat
Out[24]: 
[[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
In[25]: labels
Out[25]: 
['flippers']
In[26]: myDat,labels = trees.createDataSet()
In[27]: trees.classify(myTree,labels,[1,1])
0
1
Out[27]: 
'yes'
In[28]: trees.classify(myTree,labels,[1,0])
0
1
Out[28]: 
'no'

```


### 决策树的存储
为了节省时间，最好能够在每次执行分类时调用已经构造好的决策树，使用Python的pickle模块可以在磁盘上保存对象，并在需要的时候读取出来。
```
#使用pickle模块存储决策树
def storeTree(inputTree,filename):
    import pickle
    fw = open(filename,'w')
    pickle.dump(inputTree,fw)
    fw.close()

def grabTree(filename):
    import pickle
    fr = open(filename)
    return pickle.load(fr)

```
在命令行中输入:
```
In[29]: trees.storeTree(myTree,'classifierStorage.txt')
In[30]: trees.grabTree('classifierStorage.txt')
Out[30]: 
{'no surfacing': {0: 'no', 1: {'flippers': {0: 'no', 1: 'yes'}}}}
```