协同过滤算法简介
==============
协同过滤算法分为两大类，分别为基于用户的协同过滤（UserCF）和基于物品的协同过滤（ItemCF）。
--------------------------------------------------------------------------------

# 一.基于用户过滤算法

> 俗话说“物以类聚、人以群分”，拿看电影这个例子来说，如果你喜欢《蝙蝠侠》、《碟中谍》、《星际穿越》、《源代码》等电影，另外有个人也都喜欢这些电影，而且他还喜欢《钢铁侠》，则很有可能你也喜欢《钢铁侠》这部电影。<br>
所以说，当一个用户 A 需要个性化推荐时，可以先找到和他兴趣相似的用户群体 G，然后把 G 喜欢的、并且 A 没有听说过的物品推荐给 A，这就是基于用户的系统过滤算法。</br>

## (一)原理
    基于用户的协同过滤推荐算法拆分为两个步骤：
    1. 找到与目标用户兴趣相似的用户集合
    2. 找到这个集合中用户喜欢的、并且目标用户没有听说过的物品推荐给目标用户
### 1.寻找用户之间的相似度
#### 相似度计算公式如下
##### (1) Jaccard公式
    Jaccard系数主要用于计算符号度量或布尔值度量的个体间的相似度，因为个体的特征属性都是由符号度量或者布尔值标识，因此无法衡量差异具体值的大小，只能获得“是否相同”这个结果，所以Jaccard系数只关心个体间共同具有的特征是否一致这个问题。计算公式如下：

##### (2) 皮尔逊相关系数
    皮尔逊相关系统是比欧几里德距离更加复杂的可以判断人们兴趣相似度的一种方法。它在数据不是很规范时，会倾向于给出更好的结果。假定有两个变量X、Y，那么两变量间的皮尔逊相关系数可通过以下公式计算：

##### (3) 欧几里德距离
    假定两个用户X、Y，均为n维向量，表示用户对n个商品的评分，那么X与Y的欧几里德距离就是：
    数值越小则代表相似度越高，但是对于不同的n，计算出来的距离不便于控制，所以需要进行如下转换：
    使得结果分布在(0,1]上，数值越大，相似度越高。

##### (4) 余弦距离
    余弦距离，也称为余弦相似度，是用向量空间中两个向量余弦值作为衡量两个个体间差异大小的度量值。
    与前面的欧几里德距离相似，用户X、Y为两个n维向量，套用余弦公式，其余弦距离表示为：
    即两个向量夹角的余弦值。但是相比欧式距离，余弦距离更加注意两个向量在方向上的相对差异，而不是在空间上的绝对距离，具体可以借助下图来感受两者间的区别：
#### 例子
    假设目前共有4个用户： A、B、C、D；共有5个物品：a、b、c、d、e。用户与物品的关系（用户喜欢物品）如下图所示：
    如何一下子计算所有用户之间的相似度呢？为计算方便，通常首先需要建立“物品—用户”的倒排表，如下图所示：
    然后对于每个物品，喜欢他的用户，两两之间相同物品加1。例如喜欢物品 a 的用户有 A 和 B，那么在矩阵中他们两两加1。如下图所示：
    计算用户两两之间的相似度，上面的矩阵仅仅代表的是公式的分子部分。以余弦相似度为例，对上图进行进一步计算：
    到此，计算用户相似度就大功告成，可以很直观的找到与目标用户兴趣较相似的用户。
### 2.推荐物品
    首先需要从矩阵中找出与目标用户 u 最相似的 K 个用户，用集合 S(u, K) 表示，将 S 中用户喜欢的物品全部提取出来，并去除 u 已经喜欢的物品。对于每个候选物品 i ，用户 u 对它感兴趣的程度用如下公式计算：
    其中 rvi 表示用户 v 对 i 的喜欢程度，在本例中都是为 1，在一些需要用户给予评分的推荐系统中，则要代入用户评分。

      举个例子，假设我们要给 A 推荐物品，选取 K = 3 个相似用户，相似用户则是：B、C、D，那么他们喜欢过并且 A 没有喜欢过的物品有：c、e，那么分别计算 p(A, c) 和 p(A, e)：
    看样子用户 A 对 c 和 e 的喜欢程度可能是一样的，在真实的推荐系统中，只要按得分排序，取前几个物品就可以了。
    
### 3.推荐人
    在社交网络的推荐中，“物品”其实就是“人”，“喜欢一件物品”变为“关注的人”，这一节用上面的算法实现给我推荐 10 个园友。
    
    
# 基于物品过滤算法
