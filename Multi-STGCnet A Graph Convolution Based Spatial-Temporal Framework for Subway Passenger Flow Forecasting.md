## 师兄推荐的：Selective Cross-City Transfer Learning for Traffic Prediction via Source City Region Re-Weighting  
1. 推荐理由：~~跨城市交通~~可能会是一个特殊点，一个比较少的竞争方向
2. 摘要部分：  
	* 目的是为了解决数据不足的问题（少部分城市有大量数据，其他没有）  
	* 方法是迁移学习，但是迁移学习有缺点：
		* 有可能在源城市学习的模型对于目标城市有不好的信息  
	* 提出的模型CroeeTRes，解决了这个问题：  
		1. 特征网络 使用领域适应技术，使用边和节点级的数据学习源和目标的空间特征  
		2. 权重网络 使用源-目标联合元学习去给对于目标城市微调有用的区域赋予高权重  
		3. 预测网络 使用学到的区域权重选择性在源城市上训练实现微调  
3. 引言：  
	* 从自然语言处理学习到了区域迁移学习，但是有问题：
		* 全局的区域特征难以抽取，NLP有共同的词集，跨城市没有相交的区域  
		* 评价源区域的有用程度分数，没有相交的区域，不好评价有用性  
4. 相关工作：  
	* 目前的工作都是关注如何 fine tuning，但是没有关注如何训练一个源模型，本文解决这样的问题，选择性的学习  
	* 选择性学习先例以及可行的原因：  
		* CV领域有预训练CNN，NLP有词嵌入，可概括的特征  
		* 领域之间有明显的连接，比如相同的词集  
		* 城市之间都没有  
	* 区域特征学习目前都集中单城市上  
5. 背景与动机：  
	1. 前言：  
		* 符号与定义：  
			* 区域： 城市用经纬度范围进行栅格化，每一个栅格就是区域，区域集合用C表示  
			* 时间间隔： 城市时间间隔划分为等长不相交的间隔1，……Tc。
			* 区域交通数据： 每个C中的区域分一个向量表示数据
			* 目前是栅格，以后改进为基于图
			* 于是训练的目标可以如下表示： ![训练目标 公式](https://github.com/Liwu-di/papers-read/blob/main/pic/1.png) 
			* fine tuning:  
				* 第一步：源训练，有两种方式：  
					* 监督学习  
					* 元学习  
				* 第二步：使用目标数据微调  
	2. 动机与问题定义：  
		* 迁移学习希望缩小在目标数据集的误差，但是训练源模型的时候是在目标数据集学习，会有噪声和错误信息， 提出的模型在源训练时迁移，使用区域级选择  
		* 选择性迁移学习的目标是学习区域权重，然后再微调，如下公式![源区域权重 公式](https://github.com/Liwu-di/papers-read/blob/main/pic/2.png)  
		* 一方面提高了效果，另一方面，可视化  
6. 方法：  
	1. 整体架构  
		* 特征网络  
		* 权重网络  
		* 预测网络  
		* 结构图：![结构图 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/3.png)  
	2. 模型组件  
	3. how to learn Theta-f,w以及Theta, lambda-rs  
	4. 一些思考如图：  
		* ![思考手写图片 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/4.jpg)  
		* ![思考手写图片 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/5.jpg)  
		* ![思考手写图片 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/6.jpg)  
	5. 伪代码：  
		* ![思考手写图片 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/7.png)
7. 实验及结论  
	1. 数据集：  
		* 出租车和自行车，纽约，芝加哥，华盛顿  
		* 预测模型[ST-NET](https://dl.acm.org/doi/abs/10.1145/3308558.3313577?casa_token=BYrLv2Lrd8EAAAAA%3Al-TmLjK_I2lc8ItaWoF5G_fUPzKyhm4lePfUlAT7QXqvFsyAtyX3anJRLrbMZEonuklk9O9upaGhHIk)，特征网络[MVURE](http://fi.ee.tsinghua.edu.cn/public/publications/301ad6d8-92b8-11eb-96bc-0242ac120003.pdf)  
	2. 结果  
		* ST-NET 少数据时退化严重  
		* CrossTReS 始终很好，比最好的优秀8%  
		* 图如下：![experiment result](https://github.com/Liwu-di/papers-read/blob/main/pic/8.png)  
	3. 模型分析： 
		* 
8. 附录  
9. 代码： 
	* [github](https://github.com/Liwu-di/CrossTReS)  
	