# ANN-Toolbox
@[TOC](ANN-Toolbox)

ANN Toolbox for Excel -- an Artificial Neural Networks add-on for Excel Users

Auther: Jackie PENG

Email: jackie_pengzhao@163.com

## General Introduction

这里先给出工具箱的简单操作指引（文章中所使用的演示数据来自于Matlab的神经网络工具箱演示数据——“Breast Cancer”，这组数据共有699条样本数据，每个样本包含9个输入数据$x_1$`$x_9$，代表9种医疗检验项目的结果；另外，每个样本包含两个输出数据$y_1$与$y_2$，取值分别为1或0，代表病人是否被确诊为乳腺癌。通过机器学习，最终的目标是根据$x_1$到$x_9$的输入数据由机器来判断病人罹患乳腺癌的几率为多大）

下面是操作指引：
### 1， 界面：
ANN Toolbox的所有功能都集成在一个RibbonX菜单上：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012126109.png)

四个功能部分分别为神经网络文件操作、神经网络创建、训练及数据操作以及可视化输出
 
通过ANN File功能块可以打开、关闭或保存内存中的ANN文件。在系统中同时只能有一个ANN处于打开状态，在打开或创建新的ANN之前必须先关闭之前的ANN
目前定义了两种ANN文件格式：.nn以及.rnn文件，前者是经典MLP（多层感知器网络）和CNN（卷积网络）的共有文件格式，后者是RNN（时序网络）的存储格式，二者不通用
 
ANN从文件中导入或创建新的ANN后，在ANN File位置会显示ANN的相关信息，包括输入数据的维数（变量的数量）以及输出数据的维数：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019100701215111.png)

同样可以通过Create ANN功能块创建一个新的ANN，创建时有向导进行指引，如点击MLP按钮创建一个MLP网络：

- *第一步输入网络的名称：*

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012217362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

- *第二步建立网络的结构：*
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012242325.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

网络的输入层与输入的数据维数相同，例如输入的每条数据包含100个参数，则输入层的size应该为100
输出层的size与任务种类有关，如果是模式识别分类型任务，则输出的size与分类的数量相同，如数字识别应该有10分类，字母识别应该有26分类（26个字母）。如果是数据拟合数据预测类任务，则与需要预测的参数数量相同，比如需要预测产品价格，那么输出size为1
在Function框中可以选择输出层神经元的激活函数，如果输出值在0~1之间选择LOGS，-1~1之间选择TANH，任意值选择ReLU
在Hidden Layers框中对隐藏层的结构进行控制：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012521140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

点击“+”可以添加一个隐藏层，“-”删除一个隐藏层，理论上可以添加任意隐藏层，经验表明普通的任务一个隐藏层效果就很好，更多隐藏层并不能提高解决问题的能力，反而会加大训练的难度。
在下拉列表选择任何一个隐藏层，都可以在Size框内输入隐藏层神经元的数量

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012549195.png)

增加神经元的数量可以显著地提升神经网络的能力，但训练的计算量会增加，对于手写数字识别，100个神经元已经可以达到97+%的正确率
目前的效率建议神经元数量不要超过1000

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012627770.png)

在激活函数框中可以选择不同的激活函数用于激活神经元，不同的激活函数区别不大，在有些情况下对性能有影响，通常选择LOGS
按照上面步骤可以添加多个隐藏层

- *第三步，设置训练算法*

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012718748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

目前有四种算法：
1 GD: standard Gradient Descendant，
2 SGD: Stochastical Gradient Descendant， 
3 LM: Levenborg Marquardts,以及
4 SCG: 

不是每种算法每次又有效，不同算法训练效果和训练速度也有很大区别，总体来说SGD比较可靠，LM和SCG比较快但在大型网络情况下反而速度很慢

- 第四步设置训练停止条件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019100701280026.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

基本上有三种类型的条件可以设置：
训练最大回合数或样例数，误差函数（MSE：mean squar error）的值低于某个值或者正确率（POC）超过某个值
 
在创建过程中只要Finish按钮可用就可以跳过所有步骤直接完成创建，除了网络结构和名称之外的其他数据都可以在创建完成之后随时修改

完成后同样在菜单栏可以看到ANN的信息：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007012847416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

只要ANN已导入，可以随时使用Parameters按钮查看ANN的各个参数，除了结构和名称等基本参数外，与训练算法和训练停止条件相关的所有参数都可以随时修改：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013048921.png)
点击上述按钮后，出现下面对话框：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013119511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

如果对训练结果不满意，可以点击Initialize按钮将网络初始化，初始化后无法恢复，在操作前有确认
ANN的结构和所有权值都可以通过以下按钮来直接输入到单元格中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013144909.png)
ANN创建好后可以保存，未来可以随时读取再用，尤其是训练好的ANN，如果不保存则难以再次利用

### 训练数据的导入和神经网络的训练：
训练数据的管理通过以下按钮激活训练数据管理器来实现：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013251561.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013301218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

训练数据管理器提供两种方法导入数据：
- 直接从单元格中取数：
通过选择单元格区域并点击build按钮来实现，前面有几个选项可以选择按行取数还是按列取数，如果数据带标题需要忽略第一行或第一列，只需要勾选“Has Title”框即可，数据读取后会有说明，如果不对不同重新选择数据，重新选择按行或按列并重新点击Build即可：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013334105.png)
通常ANN无法处理太大的数据或者散差极大的数字，如上下波动超过10的数据通常需要做“白化”或“归一化”即把数据按比例缩放到均值为0、方差为1的区间内，如果数据未经归一化，点击Normalize按钮可以归一数据。
 输入数据和目标Target数据需要分开导入，处理好并白化好的数据建议保存供下次使用。
 
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013432594.png)
- 通过单击Load按钮导入

这种情况下输入数据和target数据都保存在同一个文件中
 
相比MLP网络，CNN和RNN使用的数据格式与MLP不一样，但是可以点击Convert按钮转化

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013528880.png)

所有导入的数据默认前70%用于训练，中间15%用于验证，末尾15%用于独立测试（这部分数据不参与训练，代表“全新数据”）但是比例可以自行调整，但是如果有需要，可以通过“Randomize”按钮打乱三种类型数据的顺序（数据本身的排序不变），随机分配训练、验证和测试数据。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013553129.png)

完成数据导入后，导入的数据信息会显示在工具栏上：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013719889.png)

如果ANN的输入输出和数据的结构匹配那么Start Training按钮将会显示为绿色，表示可以进行训练，否则该按钮为灰色不可用
 
Visualize组中的显示数据按钮可以把整理好的数据输出到单元格中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013743449.png)

如果数据和ANN匹配，可以立即把数据输入到ANN中并把ANN的输出结果批量输出到单元格中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007013851193.png)

这个按钮可以把数据批量输入ANN并计算结果，如果数据中已经有了Target数据，系统会计算分类正确率，如果是预测类问题应该计算预测准确度，但该功能还未实现。
 
如果要开始训练，直接单击Start Training按钮，训练的时间长短取决于ANN规模的大小，训练数据的多寡和需要达到的训练精度，短则小于1秒，长则数小时甚至更长。训练进度会显示在状态栏上，但是训练过程中系统会在完成一个回合的训练后再刷新页面，如果页面停止响应，耐心等待即可。
 
训练完成后显示训练结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019100701404751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)
对比训练完成前和训练完成后的ANN performance：
左侧是训练前的ANN输出结果，两个神经元的输出近似于0.5左右的随机波动（LOGS单元），无法形成有效输出
右侧是训练后的ANN输出结果，两个神经元的输出非常明确要么为1要么约为0，形成有效输出，同时输出值与Target目标值基本一致（注意这一部分数据是未参与训练的测试数据，因此是神经网络根据“从没见过”的数据做出的判断，绝大部分正确，只有第694行数据出现犹豫，出现错误判断）：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191007014108625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoZXBoZXJkcHB6,size_16,color_FFFFFF,t_70)

## Supported Neural Networks

## Data Organization

## Train

### Algorithms

### Training Parameters

## Applications

## Test data
