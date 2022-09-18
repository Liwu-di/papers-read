# DF-TAR: A Deep Fusion Network for Citywide Traffic Accident Risk Prediction with Dangerous Driving Behavior  
1. 摘要  
	* 使用危险驾驶统计数据和外部环境特征训练DF-TAR模型。通过对各种框架的广泛实验，DFTAR模型通过将危险驾驶与交通事故风险建模相结合，将基线模型的准确性提高了54%。  
2. 介绍  
	1. 在本文中，我们探讨了在交通事故风险预测中使用危险驾驶统计的想法。更具体地说，研究问题有两个方面，如下：  
		1 RQ1。每种犯罪类型的危险驾驶案件数量是否与交通事故数量相关？  
		2 RQ2。危险驾驶案件的统计是否提高了交通事故风险预测的性能？  
	2. 对于RQ1，我们计算了每种犯罪类型（例如超速）和每个（分区）的危险驾驶案件；然后测量危险驾驶案件数量与实际事故记录数量之间的地理和时间相关性。对于 RQ2，我们提出了一个名为 DF-TAR（用于交通事故风险预测的深度融合网络）的模型，并使用各种特征（包括从前一阶段获得的危险驾驶统计数据以及外部环境特征）对其进行训练。由于每个特征集具有不同的特征（例如，动态或静态特征），DF-TAR是基于融合不同的神经网络从每个集合中学习表示来捕获特征的基本部分并利用特征的互补性每个网络通过将它们融合成一个统一的架构。最后，我们进行了消融研究，以阐明危险驾驶统计数据的重要性。  
		* ![总体结构图 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/18.png)  
	3. 贡献  
		* 危险驾驶统计的有用性：我们通过相关性分析检查危险驾驶统计的有用性。快速加速(0.88)、快速启动(0.84) 和急转弯(0.83)与交通事故的发生位置（在街道级）。同样，关于时间相关性（在小时间隔级别），快速启动（0.81）、急转弯（0.70）和突然掉头（0.67）与交通事故有很强的相关性。出乎意料的是，超速与交通事故的相关性非常不一致。因此，我们确认某些类型的危险驾驶与交通事故高度相关  
		* 预测性能的改进：为了有效地结合危险驾驶统计数据，我们提出了一种用于交通事故风险预测的深度学习模型 DF-TAR，该模型以端到端的方式融合了不同的架构，它通过利用每种架构在表示不同类型数据方面的优势来提高预测性能。特别是，将危险驾驶统计数据注入模型可将MAE提高多达32%，RMSE提高多达5%。因此，DF-TAR显着优于基线模型，MAE提高了54%，RMSE提高了18%。  
3. 预备知识  
	1. 行车记录仪，数据  
		* 数字行驶记录仪 (DTG) 是一种记录工具，用于记录移动车辆的驾驶日志数据。 DTG设备存储带有时间戳的驾驶日志。2016年9月和2018 年9月的两个月度数据集由韩国运输安全局 (KTSA) 提供。包括十种商用车辆类型。出现在五个大都市，因为人多，车多。  
		* ![数据类型图 表格](https://github.com/Liwu-di/papers-read/blob/main/pic/19.png)  
	2. 危险驾驶准则  
		* 在本文中，我们使用了韩国广泛采用的KTSA定义的标准。每个规则都是通过对诸如速度、加速度和方向等变量施加阈值条件来指定的。有九种危险驾驶违法行为阈值条件取决于车辆类型；例如，卡车的加速度阈值是5公里/小时/秒，而公共汽车是6公里/小时/秒。  
		* ![危险类型图 表格](https://github.com/Liwu-di/papers-read/blob/main/pic/20.png)  
	3. 问题定义  
		* ![手写图 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/29.jpg)  
3.  方法  
	1. 相关性分析  
		* 假设如果危险驾驶行为与过去的事故记录具有显着的高相关性，那么整合危险驾驶行为是合理的，可以提高交通事故风险预测的准确性。危险驾驶行为数据根据定义2.3得到的地理和时间方面进行分组——即一个街道级别和一个小时间隔级别。然后，使用Pearson相关系数对相关分数进行量化。  
		* ![相关系数 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/21.png)  
		* 为数据分析的标准做法，在进行上述计算之前，我们通过去除异常值（例如，RPM 值为9999或日志发生在所选城市边界之外）和填补缺失值来预处理给定的驾驶日志数据。此外，我们定义要删除的空闲状态（即停放的车辆），假设这些记录引起交通事故的可能性很小。  
	2. 发现  
		* ![相关系数结果 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/22.png) 
		* ![相关系数结果 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/23.png) 
		* 我们将不同年份两个月危险驾驶行为发生率的发现分组如下。  
			1. 日期。总的来说，危险驾驶案件最多发生在周末。发生率最低的是假期。
			2. 一天中的时间。危险驾驶行为发生最多的时段是晚上8点到11点之间。  
			3. 车辆类型。公务出租车（64%）、城市公交（17%）和出租公交（7%）是危险驾驶行为频率最高的三大车型。
			4.  行为类型。驾驶员风险驾驶的前三名是 RA (51%)、QS (33%) 和 RD (7%)。  
			5. 城市。  
		* 关于“超速”的违反直觉的结果。在给出进一步的发现之前，值得注意的是超速(OS)对相关分数的负面影响。 OS行为不仅在地理和时间方面显示出不一致的值（0.3≤SD≤0.6），而且在许多情况下，无论值如何，计算的分数也没有统计学意义。可能的原因有： 
			1. OS多发生在深夜，道路上的车辆数量极少  
			2. 这些车辆的司机可能是高技能的司机，他们对开快很有信心  
		* 地理相关性（街道级别）。关于发生地点，在一个街道内，危险驾驶案件的总数与交通事故密切相关（0.88）。具体而言，RA（0.88）、QS（0.84）和ST（0.83）是与过去事故有很强相关性的前三名危险驾驶案例。  
		* 时间相关性（小时-间隔级别）。在时间分析方面，我们比较了一个小时内危险驾驶行为的频率，因为它是给定事故记录中的最小单位。分析结果表明，一般而言，发生时间也有很强的相关性  
	3. DF_TAR  
		* ![模型结构图 图片](![相关系数结果 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/24.png) )  
		* 该架构的动机是语音识别称为CLDNN（卷积、长短期记忆和完全连接的深度神经网络）。DF-TAR与原始CLDNN之间的区别在于，我们通过将输入分成四个组的四个特征集来扩展架构，以有效地与危险驾驶行为特征相结合，同时仍能很好地与其他环境数据一起工作。此外，我们在架构中引入了一个融合块作为门控激活单元，并应用多个损失函数来增强学习过程。因此，我们通过融合不同的神经网络来利用从不同特征集学习表示的优势  
			* 我们使用卷积块来学习每个区域空间特征的隐藏表示  
			* 循环块用于时间相关特征的时间建模   
			* 融合块用于融合来自循环块的学习表示同时过滤不相关的表示  
			* 全连接块用于将聚合的潜在表示映射到更可分离的空间并进行预测  
		* ![手写图 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/30.jpg)  
		* ![手写图 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/31.jpg)  
		* ![手写图 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/32.jpg)  
		* ![手写图 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/33.jpg)  
4. 实验  
	1. 数据描述  
		1. 在这里，与最近的研究一样，过去和未来的时间间隔是12小时（正式地，我们设置 Δt=Δt'=12）。但是，可以使用不同的Δt选择来训练所提出的模型。本研究中使用的数据集是2016年9月和2018年9月收集的来自韩国五个大城市的真实数据集。我们将数据集分为以下四组。  
			1. 交通事故风险（1个特性）。该数据集是定义2.5计算的交通事故风险评分。它代表了特定城市区域在特定时间间隔内的交通事故风险数量。  
			2. 危险驾驶行为（9 个特征）。该数据集代表一个地区在定义2.3得出的特定时间间隔内的九个危险驾驶案例。  
			3. 静态环境特征（98个特征）。我们将静态环境特征分组如下。  
				1. 人口统计数据：它由15个特征组成，代表一个地区的人口数量以及性别、年龄等的分布。  
				2. 兴趣点(POI)数据：POI数据集包含商业建筑（例如餐厅、食品店和娱乐场所）、卫生设施和教育机构的数量。我们有41个特征代表一个地区每个企业的数量。  
				3. 道路网络和规范：道路数据代表车道数量、道路等级（例如城市道路和高速公路）、道路类型（例如普通道路和桥梁）、连接数量和速度限制。我们在这个数据集中有 42 个特征。  
			4. 动态环境特征（24个特征）。  
				1. 天气和空气质量数据：它包含各种与天气相关的信息，例如温度、湿度、风速和风向，以及灰尘量（例如PM10和PM2.5）。该数据集中有13个特征。  
				2. 交通量：由于当局不公开交通量数据，因此这里的交通量由给定驾驶日志数据集中特定时间段内特定车辆的数量表示。  
				3. 历数据：日历数据包含日期和时间信息。共有10个特征。它还将星期几和假期表示为单热向量。  
	2. 实验设置  
		* 我们选择70%的数据集作为训练集，其中5%用于验证。其余30%的数据集是测试集。我们根据过去12小时的特征集预测每个城市地区未来12小时的全市交通事故风险。我们还使用min-max标准化对所有输入特征进行标准化。  
		* 详细参数略  
		* 基准模型  
			* 历史平均值 (HA)。过去12小时内的平均事故风险评分。  
			*  线性回归(LR)。最小化过去12小时交通事故风险分数与接下来12小时预测分数之间的残差平方和的线性回归量。  
			* 极端梯度提升(XGB)。使用过去12小时交通事故风险评分训练的基于树的梯度提升技术的分布式版本。   
			* SDAE。一种堆叠去噪自动编码器模型，用于使用人类移动数据进行实时风险预测。  
			* TARPML基于堆叠长短期记忆(LSTM)的全市交通事故风险预测方法。  
			* Hetero-ConvLSTM用于交通事故预测的最先进的深度学习框架，使用 ConvLSTM网络实现异构时空数据。  
			* TA-STAN。具有空间和时间注意模块的编码器-解码器框架。  
		* 任务：通过实验，我们期望回答以下三个问题  
			1. DF-TAR的结果是否优于基线和以前的研究？  
			2.  每个特征组对预测性能的影响：危险驾驶行为是否最显着？  
			3. DF-TAR的预测结果与ground truth一致吗？  
		* 评价指标：MAE，RMSE  
	3. 实验结果  
		* ![实验结果 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/25.png)  
		* ![实验结果 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/26.png)  
		* 总的来说，平均而言，我们观察到HA优于简单线性回归(LR)和极端梯度提升模型，表明每个地区的交通事故具有一定的周期性和季节性  
		* SDAE、TARPML和TA-STAN模型优于其他机器学习模型，因为深度学习方法可以比传统方法更好地概括复杂和异构的输入数据  
		* Hetero-ConvLSTM是第二好的模型，由于其结合了Spatial Graph特征和各种环境特征，其性能优于其他深度学习模型。同样，我们提出的DF-TAR模型表现最好，在使用所有特征（包括危险驾驶统计）进行训练时，MAE和 RMSE比基线提高了54%和18%，因为它代表了交通事故可能发生在哪个地区发生。  
	4. 消融实验  
		* ![实验结果 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/27.png)  
		* 即使只有危险驾驶行为数据也能在MAE中提高9%的性能，RMSE为1%——与使用所有环境特征相比  
		* 此外，正如人们所预料的那样，结合更多的数据源——外部环境特征或危险驾驶行为——将MAE中的预测误差降低了 32%，RMSE降低了5%.因此，这意味着交通事故实际上与天气、道路状况、驾驶员的危险驾驶行为等其他因素有关。鉴于这些结果，作为 RQ2的答案，我们有信心应用危险驾驶行为可以提高交通事故风险预测性能。  
	5. 案例学习  
		* ![实验结果 图片](https://github.com/Liwu-di/papers-read/blob/main/pic/28.png)  
		* 为了评估所提出模型的有效性，我们从两个城市中选择不同的时间间隔来比较测试集中的预测风险评分和实际交通事故风险。首尔（首都）和大田的第一个区（按字母顺序）的可视化。可视化显示，我们提出的模型可以预测接近真实风险值的交通事故风险，即使事件是零星的。
		* 最后，我们研究了时变特征的影响，以确定预测值是否来自模型的学习能力。以图 5a为例，它说明了江南区从 2018年9月24日晚上8点到 2018年9月25日早上7点（12小时间隔）的结果。由于天气和空气质量数据没有显着变化（没有降雨但空气质量良好），天气可能不是事故的关键因素。然后，关于交通量和危险驾驶案件，他们在该时间间隔内显示出高交通量（高于平均水平91%）和危险驾驶案件（高于平均水平56%）。具体而言，RA犯罪——根据第3节的调查结果，与交通事故的相关性最高之一——在此期间发生最多。Daejeon的结果也表现出类似的模式，如图5b所示。因此，这一观察证明DF-TAR可以通过捕捉危险驾驶统计数据、历史事故分布和环境特征的动态模式来学习相应地调整推理。  
5. 局限性  
	* 虽然实验数据比较大，结果很有希望，但是如果有更多的海量数据，我们可以在更高的层次（例如，几个月或几年之间）进行相关性分析，并获得更全面的发现。通过将数据集的长度扩展到数月或数年，预测模型将更精确地预测更长预测长度（例如，几天）的未来交通事故。此外，随着更广泛的数据，特别是对于基于深度学习的方法，该模型将变得更加通用。  
6. 相关工作  
	1. 经典学习为本的研究  
	2. 基于深度学习的研究  
7. 结论  
	* 本文介绍了一种用于全市交通事故风险预测的深度融合网络，我们称之为 DF-TAR。在训练模型之前，我们量化了危险驾驶行为和过去事故记录之间的相关分数，以证实它们之间的关系。  
	* 我们的研究结果表明，危险驾驶行为与地理和时间方面的交通事故密切相关。我们使用提取的危险驾驶行为和各种环境数据来训练提出的DF-TAR模型。评估结果表明，所提出的模型优于基线。  
	* 对于未来的工作，通过适当的数据采集机制，可以将危险驾驶行为与实时交通事故预测系统结合使用，以帮助个人避免与道路上每辆车相关的潜在风险。此外，这种方法也可以与自动驾驶车辆一起采用，以在道路上集体提醒对方。最终，我们希望我们的研究结果和提出的模型将证明有利于促进安全驾驶和预防未来的交通事故  
	








 



