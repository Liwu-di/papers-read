# Uncertainty Quantification of Sparse Travel Demand Prediction with Spatial-Temporal Graph Neural Networks
## 摘要
起点-终点 (O-D) 旅行需求预测是交通运输中的一项基本挑战。很少有研究解决细粒度 O-D 矩阵中的不确定性和稀疏性问题。这提出了一个严重的问题，因为大量的零点偏离了确定性深度学习模型背后的高斯假设。为了解决这个问题，我们设计了一个时空零膨胀负二项式图神经网络（STZINBGNN）来量化稀疏旅行需求的不确定性。STZINB-GNN 使用两个具有不同空间和时间分辨率的真实世界数据集进行检查。结果表明 STZINB-GNN 优于基准模型，尤其是在高时空分辨率下，因为它具有高精度、紧密的置信区间和可解释的参数。 STZINB-GNN 的稀疏参数对各种交通应用具有物理解释。
## 引言
1. 预测旅行需求的起点 - 目的地（O-D）矩阵引起了很多关注这项任务具有挑战性，因为需求数据的特征因移动服务而异。例如，地铁站的 O-D 矩阵通常是密集的，并且可能满足连续高斯分布，这隐含地成为确定性深度学习模型的基础。但对于叫车服务或共享单车服务，O-D 区域可能更加精细，这会导致 O-D 矩阵中的条目稀疏和离散。这种稀疏性问题在具有高时空分辨率的 O-D 矩阵中变得更加严重，因为最初密集的 O-D 矩阵可能变得更加稀释。随着出行公司越来越多地采用实时干预，以高时空分辨率准确预测稀疏和离散 O-D 矩阵可以显着提高出行服务的质量。
2. 尽管使用深度学习 (DL) 模型对密集和低分辨率 O-D 矩阵的预测进行了广泛研究，但很少有研究解决高分辨率旅行需求数据中的稀疏问题。密集 O-D 矩阵中的连续数据条目可以遵循高斯分布。然而，稀疏 O-D 矩阵中的大量零条目明显偏离了高斯假设。当 O-D 矩阵稀疏且以整数值分散时，像负二项分布这样的离散分布会更合适。此外，当数据集具有高时空分辨率时，自然会出现大量零。这些零点对于运输管理很重要，因为它们表示需求特别低的区域。为了解决稀疏问题，我们提出了一种时空零膨胀负二项式图神经网络 (STZINB-GNN) 来量化不确定性并提高预测性能。我们利用零膨胀负二项式 (ZINB) 分布来捕获稀疏 O-D 矩阵中的大量零，以及每个非零条目的负二项式 (NB) 分布。与变分自动编码器模型不同，我们设计了带有附加参数 𝜋 的时空嵌入，以学习输入为零的可能性。我们的模型利用时空图神经网络的表示能力来拟合概率分布的参数。我们比较了各种概率层来评估𝜋在捕获稀疏性方面的有效性。根据经验，我们证明了我们的模型在具有细粒度时空分辨率的数据集中的优越性。
3. 我们的主要贡献包括：
   1. 我们提出了 STZINB-GNN 来量化 O-D 旅行需求的时空不确定性，使用参数 𝜋 来学习数据稀疏性
   2. 概率 GNN 的参数成功地量化了稀疏和离散的不确定性，特别是在高分辨率数据集中
   3. 我们通过使用两个具有不同时空分辨率的真实世界旅行需求数据集证明了 STZINB-GNN 优于其他模型。
## 相关工作
1. 为了将我们的贡献与以前的工作区分开来，我们将强调我们的模型在旅行需求预测中的时空可解释性和不确定性量化。 
2. 稀疏出行需求预测的不确定性 除了平均值之外，人们普遍认为模型还应该预测不确定性。主导大多数研究的确定性模型隐含地假设同方差（即共同方差），这显着简化了方差结构。在非深度学习模型中，Jang [8] 表明旅行需求遵循零膨胀负二项分布。罗哈斯等人。 [28] 还指出，使用零通胀模型有望为间歇性旅行需求建模。然而，最近的需求预测论文通过选择低分辨率（例如 60 分钟）来回避这一挑战 [11, 12]。考虑更高分辨率（例如 5 分钟）具有挑战性，因为确定性 DL 模型不再适用。因此，结合零膨胀分布和确定性 DL 来分析稀疏 O-D 矩阵可能是一种可行的替代方案。
3. 设计指标来评估不确定性量化的质量也很重要。科斯拉维等人。 [14] 提出了平均预测区间宽度和预测区间覆盖概率来量化数据的不确定性。 Sankararaman 和 Mahadevan [29] 引入了基于似然的度量，这也是衡量预测数据分布和地面实况数据分布对齐情况的有效指标。基于现有的研究空白，我们的论文试图设计一个模型，该模型可以用适当的时空模式识别和严格的预测区间界限来表述稀疏的旅行需求预测问题。
## 方法
1. 定义
    1. ![OD 图](https://github.com/Liwu-di/papers-read/blob/main/pic/34.png)
    2. ![OD 图说明](https://github.com/Liwu-di/papers-read/blob/main/pic/35.png)
    3. ![defination](https://github.com/Liwu-di/papers-read/blob/main/pic/36.png)
2. 零膨胀负二项分布
   1. 负二项分布（Negative binomial distribution）是统计学上一种描述在一系列独立同分布的伯努利试验中，成功次数达到指定次数（记为r）时失败次数的离散概率分布。比如，如果我们定义掷骰子随机变量x值为x=1时成功，所有x≠1为失败，这时我们反复掷骰子直到1出现3次（成功次数r=3），此时非1数字出现次数的概率分布即为负二项分布。
   2. 若每次伯努利试验有两种可能的结果，分别为成功或者失败。在每次试验中，成功的概率为p，失败的概率为（1-p）。反复进行该伯努利试验，直到观察到第r次成功发生。此时试验失败次数X的分布即为负二项分布（或称帕斯卡分布），那么：若随机变量X服从参数为r和p的负二项分布，则记为X~NB(r,p).在实际生活中，我们可以使用负二项分布描述某种机器在坏掉前，能够工作的天数的分布。此时，“成功”的事件可以指机器正常工作一天，“失败”的事件可以指机器故障的一天。如果我们使用负二项分布来描述运动员在获取r个奖牌前尝试的次数的分布，此时，“失败”的事件指运动员的一次尝试，“成功”的事件指运动员获取一枚奖牌。如果使用负二项分布来描述掷一枚硬币出现r次正面前，出现硬币反面的次数的分布，“成功”的事件指出现硬币的正面，“失败”的事件指出现硬币的反面。
   3. ![负二项分布](https://github.com/Liwu-di/papers-read/blob/main/pic/37.png)
   4. 其中 𝑛 和 𝑝 是形状参数，分别决定成功的次数和单个失败的概率
   5. ![零膨胀负二项分布](https://github.com/Liwu-di/papers-read/blob/main/pic/38.png)
   6. 需要两个步骤来生成数据的分布：它们要么是概率为 𝜋 的零，要么是遵循 NB 分布的概率为 1 - 𝜋 的非零。概率分布中的参数 𝜋、𝑛、𝑝 由时空 GNN 参数化。  
3. STZINB-GNN 
   1. 我们引入 STZINB-GNN，一种可泛化的深度学习架构，以在 ZINB 假设下为每个 O-D 矩阵条目捕获时空相关性 𝑍 = M (𝑋1:𝑡, 𝐴)，并预测未来 𝑘 时间窗口提前 𝑓𝑍 𝐼 𝑁 𝐵 （𝑋𝑡+1：𝑡+𝑘|𝑍|）。 M 是提出的模型框架，它采用历史需求𝑋1:𝑡 ∈ Z |𝑉|×𝑡𝑇 和邻接矩阵𝐴 ∈ R |𝑉 |× |𝑉 |作为输入，并学习参数嵌入 𝑍 ∈ R |𝑉 |×𝑘×3 的未来需求 𝑋𝑡+1:𝑡+𝑘 ∈ R |𝑉 |×𝑘：
   2. ![总体公式](https://github.com/Liwu-di/papers-read/blob/main/pic/39.png)
   3. ![总体结构](https://github.com/Liwu-di/papers-read/blob/main/pic/40.png)
   4. STZINB-GNN 的整体架构如图 1 所示。我们在 𝑗 𝑡ℎ 时间窗口 𝑋𝑗:𝑗+𝐵 ∈ Z |𝑉 |×𝐵𝑇 将一批大小为 𝐵 的输入 O-D-T 张量转换为张量 X𝑗 ∈ Z |𝑉 |×𝐵×𝑇作为模型输入。我们使用扩散图卷积网络 (DGCNs) 来捕获 O-D 对的空间邻接关系，并使用时间卷积网络 (TCNs) 进行时间相关性。 DGCN 和 TCN 的输出，包括空间嵌入𝑛𝑠、𝑝𝑠、𝜋𝑠和时间嵌入𝑛𝑡、𝑝𝑡、𝜋𝑡，用于参数化 ZINB 分布。这些空间和时间嵌入包含对其空间和时间局部性的 ZINB 参数的独立估计。然后，我们使用 Hadamard 乘积将 𝑛𝑠 、 𝑝𝑠 、 𝜋𝑠 和 𝑛𝑡 、 𝑝𝑡 、 𝜋𝑡 融合到 𝑍 中，可以用其他非线性操作（如全连接层）代替。通过融合分布参数的空间和时间嵌入，我们得到满足的最终 ZINB 参数集𝑍
   5. ![更加详细公式](https://github.com/Liwu-di/papers-read/blob/main/pic/41.png)
   6. 请注意，图 1 中的融合 𝑍 可以解释为未来需求分布的参数集。这个过程类似于变分自动编码器 (VAE) 的概念，我们学习制定分布的潜在形状变量 [7, 15, 16]。 VAE 理论用于将深度学习技术引入变分推理的统计领域，从而提供更强大的潜在变量表示。我们的编码组件使用带有额外稀疏参数的时空嵌入架构，解码部分与未来需求的概率估计有关
   7. 为了解决零会给基于 KL 散度的变分下界提供无限值的问题，我们直接使用负似然作为我们的损失函数，以更好地将分布拟合到数据中。让 𝑦 是对应于来自 𝑍 的具有参数 𝑛、𝑝、𝜋 的预测矩阵条目之一的真实值。 ZINB 的对数似然由 𝑦 = 0 和 𝑦 > 0 部分组成，可以近似为 [23, 24]：
   8. ![对数似然](https://github.com/Liwu-di/papers-read/blob/main/pic/42.png)、
   9. 贴几个引用文献中的图，解释了对数似然（我觉得，狗头）
   10. ![引用文献](https://github.com/Liwu-di/papers-read/blob/main/pic/44.png)
   11. ![引用文献](https://github.com/Liwu-di/papers-read/blob/main/pic/45.png)
   12. ![引用文献](https://github.com/Liwu-di/papers-read/blob/main/pic/46.png)
   13. ![引用文献](https://github.com/Liwu-di/papers-read/blob/main/pic/47.png)
   14. ![引用文献](https://github.com/Liwu-di/papers-read/blob/main/pic/48.png)
   15. ![引用文献](https://github.com/Liwu-di/papers-read/blob/main/pic/49.png)
   16. 其中𝜋、𝑛、𝑝也是根据𝑦 = 0或𝑦 > 0的指标选取计算的，Γ是Gamma函数。最终的负对数似然损失函数由下式给出：
   17. ![损失函数](https://github.com/Liwu-di/papers-read/blob/main/pic/43.png)
   18. 请注意，我们的模型也可以通过将概率层修改为其他分布并使用𝑍来表示相关的形状参数集来推广到其他分布。例如，如果我们使用高斯分布作为我们的模型，我们可以使用均值和方差的空间和时间嵌入来参数化概率层，从而量化遵循高斯分布的数据不确定性。利用概率层的灵活性，我们设计了各种基准模型来与 ZINB 模型进行比较。我们的脚本可以在 Github1 中找到。
4. Adjacency Matrix for O-D Pairs
   1. 本研究使用 O-D 对作为顶点，不同于之前使用区域作为顶点的 GNN 方法 [4, 5, 30, 41]。因此，我们需要对 O-D 对的空间相关性进行建模，并以不同的方式构建邻接矩阵。直观地说，具有相似起点或目的地的 O-D 对在图形表示中也很接近，因为乘客很可能在相邻的 O-D 对而不是远程的 O-D 对之间转移。受到 Ke 等人的工作的启发。 [12]，我们将邻接矩阵表示为：![邻接矩阵](https://github.com/Liwu-di/papers-read/blob/main/pic/50.png)
   2. lng和lat表示经纬度，ℎ𝑎𝑣𝑒𝑟𝑠𝑖𝑛𝑒函数表示地球上两点之间的距离
   3. 基本思想是通过平均起点和终点的相似性来利用 O-D 对的地理相邻性。很明显，𝑖、𝑗的顺序不影响ℎ𝑎𝑣𝑒𝑟𝑠𝑖𝑛𝑒函数的输出，这意味着𝐴是对称的。最终的邻接矩阵 𝐴 是𝐴 𝑂 和𝐴 𝐷 的二次均值，其中起点或目的地之间的距离在邻接矩阵中具有相同的影响。
   4. 未来的研究可以在构建邻接矩阵时为起点或终点分配不同的权重，甚至可以结合人口统计图来丰富𝐴的信息。本文侧重于模型的不确定性量化和可解释性。
5. Diffusion Graph Convolution Network  
   1. 在概率论和统计学中，扩散过程是随机微分方程的解。这是一个连续时间的马尔可夫过程，几乎可以肯定是 连续的样本路径。布朗运动、反射布朗运动和奥恩斯坦-乌伦贝克过程是扩散过程的例子。扩散过程的样本路径模拟嵌入流动流体中的粒子的轨迹，并由于与其他粒子的碰撞而受到随机位移，这称为布朗运动。那么粒子的位置是随机的；它作为空间和时间函数的概率密度函数由平流-扩散方程控制。
   2. 了捕捉 O-D 对之间流动动力学的随机性，我们将空间相关性建模为扩散过程 [1, 20]。引入扩散过程有助于学习从一个 O-D 对到另一个的空间依赖性。该过程的特点是在给定图上随机游走，概率为 𝛼 ∈ [0, 1] 和前向转移矩阵 𝑊~ 𝑓 = 𝐴/𝑟𝑜𝑤𝑠𝑢𝑚(𝐴) [37]。在足够大的时间步长之后，扩散过程的马尔可夫性质保证它收敛到一个平稳分布 P ∈ R |𝑉 |× |𝑉 | . P 的每一行代表从该节点扩散的概率。可以以封闭形式计算平稳分布 [31]：
   3. ![封闭形式](https://github.com/Liwu-di/papers-read/blob/main/pic/51.png)
   4. 其中𝑘是扩散步骤，通常设置为有限数𝐾。变量 𝑘 仅用于演示扩散过程。我们可以类似地用后向转移矩阵𝑊~𝑏=𝐴𝑇/𝑟𝑜𝑤𝑠𝑢𝑚(𝐴𝑇)定义后向扩散过程。因为我们的邻接矩阵𝐴是对称的，所以𝑊~𝑓 = 𝑊~𝑏。前向和后向扩散过程模拟了乘客从一对 O-D 转移到另一对的动态，例如从学校旅行到家庭市场旅行的转变。 DGCN 层的构建块可以写成 [37]：
   5. ![图卷积扩散](https://github.com/Liwu-di/papers-read/blob/main/pic/52.png)
   6. 其中𝐻𝑙代表𝑙𝑡ℎ隐藏层； Chebyshev 多项式𝑇𝑘 (𝑋) = 2𝑋𝑇𝑘−1 (𝑋) −𝑇𝑘−2 (𝑋) 用于逼近 DGCN 中的卷积运算，边界条件为𝑇0 (𝑋) = 𝐼 和𝑇1 (𝑋) = 𝑋（注意， Chebyshev 多项式用于近似扩散 P 而不是使用封闭形式）；添加 𝑙 𝑡ℎ 层 Θ 𝑘 𝑓 ,𝑙 和 Θ 𝑘 𝑏,𝑙 的学习参数来控制每个节点如何转换接收到的信息； 𝜎 是激活函数（例如 ReLU、Linear）。在我们的模型中，我们堆叠 3 个 DGCN 层以更好地捕捉 O-D 动态。
6. Temporal Convolutional Network  
   1. TCNs 与循环神经网络（RNNs）相比的优势包括：1）TCNs 可以使用不同长度的序列作为输入，对不同的时间分辨率和尺度更适应； 2) TCNs 具有轻量级架构和快速训练[3]。 TCN 的总体思路是在 𝑙 𝑡ℎ 层中应用宽度为 𝑤𝑙 的共享门控一维卷积，以便在当前时间点传递来自 𝑤𝑙 邻居的信息。每个 TCN 层𝐻𝑙 接收来自前一层𝐻𝑙−1 的信号，并使用 [19] 进行更新：
   2. ![TCN](https://github.com/Liwu-di/papers-read/blob/main/pic/53.png)
   3. 其中Γ𝑙是对应层的卷积滤波器，∗是共享卷积操作，𝑏代表偏差。如果之前的隐藏层跟随𝐻𝑙−1 ∈ R 𝐵× |𝑉 |×𝑤𝑙−1 ，则卷积滤波器为 Γ𝑙 ∈ R 𝑤𝑙×𝑤𝑙−1 使得𝐻𝑙 ∈ R 𝐵× |𝑉 |×𝑤𝑙 。如果每个 TCN 层中没有填充，很明显𝑤𝑙 < 𝑤𝑙−1。 TCN也是一种sequence-to-sequence模型，可以直接连续输出对未来目标窗口的预测。此外，TCN 感受野是灵活的，可以通过 Γ𝑙 的数量和核来控制。在我们接下来关于使用不同时间分辨率缩放我们的模型的讨论中很有用。我们还堆叠了 3 个 TCN 层。
## 数值实验
1. 我们使用来自芝加哥数据门户 (CDP) 2 和智能位置数据库 (SLD) 3 的两个真实数据集来评估模型性能。 CDP 数据集包含芝加哥地区交通网络提供商（拼车公司）的行程记录。芝加哥市分为 77 个区域，每 15 分钟记录一次带有接送区域的行程请求。我们使用 2019 年 9 月 1 日至 2019 年 12 月 30 日 4 个月的观测数据。数据集分为各种空间分辨率，以方便讨论模型性能。我们随机选择 10 × 10 O-D 对与空间稀疏数据样本具有相同的时间周期； SLD 数据集也用于 Ke 等人的工作。 [12]。具体来说，我们选择了 2018 年 1 月至 2018 年 4 月曼哈顿地区（按行政邮政编码划分为 67 个区域）的出租汽车 (FHV) 行程记录。该 SLD 数据具有与 CDP 数据集相似的特征，包括时间戳，以及接送区 ID。由于数据集包括个人旅行信息的时间戳，我们改变时间分辨率（5 分钟、15 分钟和 60 分钟间隔）来测试我们的模型性能。除了完整的 O-D 矩阵 (67 × 67)，我们还采样了 10 × 10 个小的 O-D 对样本以与 CDP 数据集对齐。我们使用 60% 的数据进行训练，10% 用于验证，最后 30% 用于测试。表 1 总结了分析中使用的数据场景。随着 SLD 数据集的时间分辨率从 60 分钟增加到 5 分钟，零率从 50% 增加到 88%。如图 2 所示，O-D 流量的分布非常倾斜，因为大多数 O-D 流量都集中在较小的值附近。在 60 分钟分辨率下的 O-D 流有几个大于 8 的观测值，但在 5 分钟分辨率下，最小值是 5 分钟间隔内的 3 次行程。这种稀疏（和小值）的 O-D 矩阵是大多数确定性深度学习模型的挑战。
2. ![数据](https://github.com/Liwu-di/papers-read/blob/main/pic/54.png)
3. 我们使用点估计统计和分布特征的指标来评估和比较各种模型的性能。使用平均绝对误差 (MAE) 评估预期中值的预测精度（即点估计精度）![公式](https://github.com/Liwu-di/papers-read/blob/main/pic/55.png) 其中𝑥^𝑖和𝑥𝑖分别是𝑖𝑡ℎ数据点的预测值和真实值。为了评估估计的不确定性，我们在 10%-90% 置信区间上使用平均预测区间宽度 (MPIW)![公式](https://github.com/Liwu-di/papers-read/blob/main/pic/56.png)其中𝐿𝑖和𝑈𝑖对应于观察的置信区间的下限和上限𝑖。 MPIW 的定义可以扩展到数据的其他分位数。更紧（更小）的预测间隔是更可取的。除了 MPIW，我们还使用 Kullback-Leibler Divergence (KL-Divergence) 来评估模型输出分布与测试集分布的接近程度 [17]。我们将 KL-Divergence 定义为：![公式](https://github.com/Liwu-di/papers-read/blob/main/pic/57.png)由于许多 𝑥𝑖 和 𝑥^𝑖 值可能为零，因此使用小扰动 𝜖 = 10−5 来避免因除以 0 而导致的数值问题。由于 KL-Divergence 测量两个分布之间的差异，因此需要较小的值。为了比较离散 O-D 矩阵条目的模型性能，我们使用真零率和 F1 分数测量。真零率量化了模型在真实数据中复制稀疏性的程度。另一方面，F1 分数衡量离散预测的准确性。即使 F1 分数是为分类模型设计的，我们仍然可以将离散值视为多个标签，并相应地定义精度和召回率 [25]。较大的真零率和 F1 分数值表明模型性能更好。
4. 为了探索 STZINB-GNN 的优势，我们将 STZINB-GNN 结果与其他三个模型进行比较：（1）历史平均值（HA）作为统计基线。它是通过对历史数据中相同的每日时间间隔（例如上午 8:00-上午 8:15）的需求进行平均来计算的，以预测未来一步的未来需求。 (2) 时空图卷积网络 (STGCN)4 是最先进的交通预测深度学习模型 [42]。 STGCN 还使用图卷积进行空间嵌入，使用时间卷积进行时间嵌入。但是，它无法量化需求的不确定性，只能产生点估计； （3）在时空嵌入概率层中具有概率假设的模型：负二项式（STNB-GNN）、高斯式（STG-GNN）和截断正态（STTN-GNN），如图1所示。这三个分布有两个参数，少于 STZINB-GNN 的三个参数。这些模型的其他组件和参数相同。当 O-D 矩阵分辨率较高时，STZINB-GNN 的性能优于其他模型，但在分辨率变得更粗糙时性能较差。表 2 以粗体突出显示字段，以指示每个数据集的最佳性能。为了公平比较，包括 STG-GNN、STTTNGNN、HA 和 STGCN 在内的连续输出模型的输出被四舍五入到最接近的整数，以与 STNB-GNN 和 STZINB-GNN 进行比较。如表 2 所示，当空间和时间分辨率较低时，如 CDP_SAMP10 和 SLD_SAMP10 情况，STZINB-GNN 的性能与 STNB-GNN 相似。在最稀疏的情况下，SLD_5min（88% 的零）、STG-GNN、STTN-GNN 和 STGCN 未能有效地捕捉到偏斜的数据分布，导致低真零率和 F1 分数。另一方面，STZINB-GNN 成功地学习了数据的稀疏性。它的预测具有非常小的 MPIW，比其他模型小近 10 倍。当时间分辨率降低时，STNB-GNN、STTN-GNN 和 STGCN 开始优于 STZINB-GNN。在 SLD_60min 的情况下，STNBGNN 比 STGCN 更适合数据。另一方面，STZINB-GNN 只能更好地预测真零条目。没有一个模型在所有分辨率级别下都优于其他模型 [36]。概率层的负二项分布可以有效捕捉出行需求的离散值。图 3 显示了 SLD（纽约）数据中连续两天的预测和真实平均旅行需求，图 3(a) 为 SLD_15min，图 3(b) 为 SLD_60min。通过更精细的分辨率，STNB-GNN 和 STZINB-GNN 模型更有可能准确预测平均出行需求。当分辨率降低时，例如在 60 分钟的情况下，所有深度学习模型都提供相似的性能。这种 60 分钟的分辨率通常用于大多数深度学习研究，但我们的结果证明了当时间单位短于 60 分钟时离散概率假设的重要性。
5. ![对比表格](https://github.com/Liwu-di/papers-read/blob/main/pic/58.png)
6. ![对比图](https://github.com/Liwu-di/papers-read/blob/main/pic/59.png)
7. STZINB-GNN 在量化不确定性方面提供了卓越的性能，因为它使用更窄的 MPIW 进行固定概率覆盖，特别是在时间分辨率很高的情况下。图 4 可视化了 MPIW 的散点图和 4489 个 O-D 对在三个时间分辨率下的地面实况旅行需求。图 4(a) 表明 STZINB-GNN 导致的 MPIW 明显小于粒度 5 分钟分辨率情况下的其他模型。这是因为在 SLD_5min 的情况下，每个 O-D 对的平均行程需求小于 2，并且大多数都为零。在 STZINB-GNN 中引入稀疏参数 𝜋 有效地捕获了零点。 STG-GNN 和 STTN-GNN 无法捕捉数据分布的偏度，导致 MPIW 较大。然而，当时间分辨率降低时，具有稀疏参数 𝜋 的零膨胀变得不那么重要。图 4(b) 和 4(c) 显示 SLD_15min 和 SLD_60min 情况下每 15 分钟和 60 分钟时间间隔的平均出行需求较大。在这两种情况下，STZINB-GNN 只有在平均出行需求较小时才具有较小的 MPIW。当平均需求很大时，STG-GNN 和 STTN-GNN 的性能优于 STZINBGNN。然而，它们的输出预测分布并不稳定，因为相同的出行需求值对应不同的MPIW，方差很大。图 4 的底行比较了 STZINB-GNN 和 STNB-GNN 对特定 O-D 对的期望和 MPIW，该对在所选时间范围内具有最大的需求流。在 SLD_5min 和 SLD_15min 的情况下，STZINB-GNN 提供比 STNB-GNN 更紧凑的置信区间，即使它的点预测不太准确。结果与表 2 中的结果以及图 4 顶行中的 MPIW 比较结果一致。
8. ![结果图](https://github.com/Liwu-di/papers-read/blob/main/pic/59.png)
9. STZINB-GNN 在量化不确定性方面提供了卓越的性能，因为它使用更窄的 MPIW 进行固定概率覆盖，特别是在时间分辨率很高的情况下。图 4 可视化了 MPIW 的散点图和 4489 个 O-D 对在三个时间分辨率下的地面实况旅行需求。图 4(a) 表明 STZINB-GNN 导致的 MPIW 明显小于粒度 5 分钟分辨率情况下的其他模型。这是因为在 SLD_5min 的情况下，每个 O-D 对的平均行程需求小于 2，并且大多数都为零。在 STZINB-GNN 中引入稀疏参数 𝜋 有效地捕获了零点。 STG-GNN 和 STTN-GNN 无法捕捉数据分布的偏度，导致 MPIW 较大。然而，当时间分辨率降低时，具有稀疏参数 𝜋 的零膨胀变得不那么重要。图 4(b) 和 4(c) 显示 SLD_15min 和 SLD_60min 情况下每 15 分钟和 60 分钟时间间隔的平均出行需求较大。在这两种情况下，STZINB-GNN 只有在平均出行需求较小时才具有较小的 MPIW。当平均需求很大时，STG-GNN 和 STTN-GNN 的性能优于 STZINBGNN。然而，它们的输出预测分布并不稳定，因为相同的出行需求值对应不同的MPIW，方差很大。图 4 的底行比较了 STZINB-GNN 和 STNB-GNN 对特定 O-D 对的期望和 MPIW，该对在所选时间范围内具有最大的需求流。在 SLD_5min 和 SLD_15min 的情况下，STZINB-GNN 提供比 STNB-GNN 更紧凑的置信区间，即使它的点预测不太准确。结果与表 2 中的结果以及图 4 顶行中的 MPIW 比较结果一致。
10. ![结果图](https://github.com/Liwu-di/papers-read/blob/main/pic/60.png)
11. ZINB 分布中的稀疏参数 𝜋 衡量一个区域有零需求的可能性。请注意，我们预测的每个 O-D 对都有参数 𝜋，它可以捕获一个区域的流入和流出稀疏度。由于 O-D 关系很难在地图中说明，因此我们将重点放在特定区域，并在选择该区域作为起点或目的地时投影 O-D 活动。图 5 显示了时代广场在 2018 年 4 月 25 日早晚高峰期间的 O-D 活动（即参数 𝜋）热图。可以发现存在空间局部性，社区更有可能通勤到他们的邻居。此外，时间模式也不同。如图 5 所示，许多游客在晚上探索邻近地区，但在早高峰时非常不活跃。因此，稀疏参数 𝜋 使 STZINB-GNN 具有高度可解释性。在交通决策或运营管理者中使用稀疏参数𝜋分配移动服务非常重要。我们的框架有可能扩展到其他具有许多零且对事件敏感的预测任务，例如事故预防或残疾人辅助运输服务。
12. ![结果图](https://github.com/Liwu-di/papers-read/blob/main/pic/61.png)
## 结论
在本文中，我们提出了一个可泛化的时空 GNN 框架来预测稀疏旅行需求的概率分布并量化其不确定性。我们引入了具有稀疏参数 𝜋 的零膨胀负二项分布。我们使用空间扩散图神经网络来捕捉空间相关性和时间卷积网络来捕捉时间依赖性。 STZINB-GNN 框架分别嵌入分布参数的空间和时间表示，并将它们融合以获得每个时空数据点的分布。 STZINB-GNN 使用两个具有不同空间和时间分辨率的真实世界数据集进行评估。我们发现，当数据以高分辨率表示时，STZINB-GNN 的性能优于基线模型，但在分辨率变粗时性能更差。这也反映在预测区间中，其中 STZINB-GNN 具有紧密的置信区间。由于参数𝜋具有物理解释，我们的模型可以帮助交通决策者有效地将移动服务分配给零或非零需求区域。该框架有可能扩展到其他使用高度稀疏的数据点，例如异常检测和事故预测。

