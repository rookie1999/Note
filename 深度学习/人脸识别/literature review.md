## 人脸检测算法

### 1. Rapid Object Detection using a Boosted Cascade of Simple Features

citation: `P.A. Viola, M.J. Jones, Rapid object detection using a boosted cascade of simple features, in: CVPR, issue 1, 2001, pp. 511–518`

文献描述：

```
3个contribution：
1）积分图加速特征计算
2）集成学习  基于AdaBoost，集成多个弱分类器，变成强分类器
	每一个弱分类器可以表示 a single feature
3）分类器集成的方法——cascade
	差的分类器先尽可能地去除非人脸的地方，减小计算量，加快速度（因为确定哪个范围内有对象是很快的）
	首先是一个initial classifier，得到一些subwindows给后面 a sequence of classifiers（more complex than last）做输入
实时性很强  1秒15帧
```

![](D:\大学\笔记\深度学习\人脸识别\pic\17.png)

### 2. Eigenfaces for Recognition

citation: `M. Turk and A. Pentland, “Eigenfaces for Recognition,” J. Cognitive Neuroscience, vol. 3, no. 1, pp. 71-86, 1991`

文献描述：

```
系统通过将人脸的表示映射到一个低维的特征空间，得到特征向量，这些特征向量被称为特征脸eigenface
每一张人脸可以使用不同特征脸的权重组合来表示，通过比较每个人脸的权重区别，判断是否是一个人
```

识别过程：

```
先计算训练集的协方差矩阵，得到一系列特征值，取前M个最大特征值对应的特征向量作为特征脸，这些特征脸就是face space，计算训练集中每一张人脸在face space下的weight sum作为人脸表示

识别：
将新输入的图片映射到face space，根据weight sum判断是否是人脸
如果是，将weight sum与已保存的其他人脸表示进行比较，判断身份
```

**检测过程**：

第一种方法：使用Motion Detection检测是否有运动的物体，运动的物体是否为人类的head

第二种方法：使用“Face Space”确定人脸是否存在（该基本思想用于检测场景中人脸的存在：在图像中的每个位置，计算局部子图像与人脸空间之间的距离E。）

**PCA的作用**：减少分析的数据和变量同时尽量不破坏数据之间的关联

* 数据降维。减少变量个数； 

* 确保变量独立；提供一个合理的框架解释。

* 去除噪声，发现数据背后的固有模式。

```
PCA过程
特征中心化：将每一维的数据（矩阵A）都减去该维的均值，使得变换后（矩阵B）每一维均值为0； 计算变换后矩阵B的协方差矩阵C；  计算协方差矩阵C的特征值和特征向量；  选取大的特征值对应的特征向量作为”主成分”，并构成新的数据集；
```

**Learning to recognize new faces:**

是一种无监督学习的方式，当系统接收了多个输入，且这些输入被预测为unknown类别。如果这些输入的distance小于一个指定的threshold，那么这些图像会被定义为一个新的类别（系统学习到的新类别）

此时，可以选择重新计算eigenfaces

**局限性**

```
待识别图像中人脸尺寸接近特征脸中人脸的尺寸； 
待识别人脸图像必须为正面人脸图像。
```

### 3. Cascade CNN

citation: `H. Li, Z. Lin, X. Shen, J. Brandt, and G. Hua. A convolutional neural network cascade for face detection. In CVPR, pages 5325–5334, 2015.`

* 在初期就使用CNN去删除false positive detections
* rejects false positive detections quickly in the ealry, low resolution layers
* carefully verify the detections in the later, high resolution layers
* 引入CNN-based face bounding box calibration step in the cascade，加速CNN cascade，获取更高质量的定位
* multi-resolution CNN提供more discriminative features
* workflow
  * 先经过一个12-net去除大部分非人脸的窗口
  * 再通过一个12-calibration-net调整窗口大小和位置和通过NMS去除highly overlapped windows
  * 剩余的检测窗口裁剪出来，resize to 24*24，再通过24-net去除非人脸的窗口
  * 再通过一个24-calibration-net调整窗口大小和位置和通过NMS去除highly overlapped windows
  * 剩余的检测窗口裁剪出来，resize to 48*48，再通过24-net去除非人脸的窗口
  * 再通过一个48-calibration-net调整窗口大小和位置和NMS
  * 注意：NMS的 IOC ratio也是一个超参数，且两个NMS只作用于自己层对应的size，而最后一层是作用于所有的size
* 一个6个CNN：3个分类，3个用来做bounding box calibration，并且使用ReLU作为激活函数
* 校正网络 calibration-net
  * 对于矩形框的校正，使用三个参数水平平移量xn,垂直平移量yn,宽高缩放比sn，将原来$(x, y, w, h) -> (x-\frac{x_n w}{s_n}, y-\frac{y_n h}{s_n}, \frac{w}{s_n}, \frac{h}{s_n})$
  * 对于这三个参数，列出它们一些值，不同值组合成一个类，然后建立一个45类的分类器，对得分较高的几个类求其算术平均值，从而进行校正
* 在24-net中，本层输入会另外resize为12*12，送入一个类似12-net的结构中，最后与原本的全连接层的数据进行concatenate（to detect small faces and become more discriminative）

### 4. Joint Face Detection and Alignment using Multi-task Cascaded Convolutional Networks  

citation: `K. Zhang, Z. Zhang, Z. Li, and Y. Qiao. Joint face detection and alignment using multi-task cascaded convolutional networks. arXiv preprint:1604.02878, 2016.`

* 不仅可以face detection，还可以face alignment

* deep cascaded multi-task framework

  * first stage: a shallow CNN 排除大量的非人脸窗口

    * Proposal Network （P-Net）
    * 先是一个FCN (Fully convolutional network 全卷积网络)进行初步特征提取，标定大致的边框

    ```
    FCN的介绍：
    * 对输入的图片的每一个像素进行分类，以此达到对图片的特定部分进行分类的效果（CNN是对整个图片进行分类）
    * FCN可以接受任意尺寸的输入图像，通过反卷积层（deconv）对最后一个卷积层的feature map进行采样，使它恢复到与输入图像相同的尺寸（保留原始输入图像中的空间信息）
    * 对输出进行逐像素地使用softmax loss计算损失(每一个像素都是一个样本)
    ```
    
    * 再使用Bounding Box Regression调整边框，使用NMS过滤高度重合的边框
    * P-Net 非常快
    
  * second stage: 使用a more complex CNN进一步去除不包含人脸的窗口

    * Refine Network (R-Net)
    * 进一步去除错误的人脸窗口
    * 再使用Bounding Box Regression调整边框，使用NMS过滤高度重合的边框

  * third stage: 使用一个more powerful CNN refine the result，并输出facial landmarks positions

    * Output Network (O-Net)
    * 与R-Net类似，但是会额外输出 five facial landmark's positions

* 特别设计的网络结构，有好的real time performance

* 使用three tasks to train CNN detectors

  * face / non-face classification
    * loss: cross-entropy loss
  * bounding box regression
    * loss: Euclidean distance between the predicted box coordinates and the ground-truth box coordinates (box coordinate包含左上角的横坐标和纵坐标，以及高度、宽度)
  * facial landmark localization
    * 与bounding box regression相似
    * loss: Euclidean distance between the predicted landmarks and the ground-truth landmakrs (左眼，右眼，鼻子，左嘴角，右嘴角的横纵坐标)

* Online Hard sample mining

  * 通过前向传播计算loss之后，进行排序，然后选出前70%进行梯度计算和更新

* training

  * 有四种训练数据
  * 第一种是：Negative
    * IoU与真实值小于0.3
  * 第二种是：Positive
    * IoU与真实值大于0.65
  * 第三种是：Part faces
    * IoU与真实值在0.4到0.65之间
  * 第四种是Landmark faces
    * faces labeled 5 landmark‘s positions
  * Negative和Positive用来训练 face/non-face classification
  * Positive和Part faces用来训练bounding box regression
  * Landmark faces是用来训练facial landmark localization



## 人脸识别算法

### 1. FisherFace

citation: ` P. N. Belhumeur, J. P. Hespanha, and D. J. Kriegman, “Eigenfaces vs.Fisherfaces: Recognition using class-specific linear projection,” IEEE Trans. Pattern Anal. Machine Intell., vol. 19, pp. 711–720, July 1997.`

* insensitive to extreme variations in lighting and facial expressions
* EigenFace使用PCA进行降维，得到一个低维空间，使得每个类都在上面的投影的方差最大，但是由于照明和观看方向，同一张脸的图像之间的变化几乎总是比由于脸部身份变化而导致的图像变化大，所以EigenFace得到的并不一定是a discrimination method
* LDA降维，使得降维后的类间距离最大，类内距离最小

### 2. DeepFace

citation:` Y. Taigman, M. Yang, M. Ranzato, and L. Wolf. Deepface: Closing the gap to human-level performance in face verification. In CVPR, 2014.`

* 使用3D model进行人脸对齐

  ```
  2D alignment： 
  对Detection后的图片进行二维裁剪， scale, rotate and translate the image into six anchor locations，将人脸部分裁剪出来。
  
  3D alignment： 
  找到一个3D 模型，用这个3D模型把二维人脸crop成3D人脸。67个基点，然后Delaunay三角化，在轮廓处添加三角形来避免不连续。
  将三角化后的人脸转换成3D形状 三角化后的人脸变为有深度的3D三角网 将三角网做偏转，使人脸的正面朝前 最后放正的人脸
  ```

* 使用CNN以及一个large dataset去训练，泛化能力好

  ```
  C1：卷积层，卷积核尺寸11*11，共32个卷积核 
  M2：池化层，最大池化3*3，即stride = 2 
  C3：卷积层，卷积核尺寸9*9   ，共16个卷积核 
  L4： 卷积层，卷积核尺寸9*9   ，共16个卷积核。L表示local，意思是卷积核的参数不共享 
  L5： 卷积层，卷积核尺寸7*7   ，共16个卷积核。
  L6： 卷积层，卷积核尺寸5*5   ，共16个卷积核。
  F7： 全连接，4096个神经元 
  F8： 全连接，4030个神经元 
  前三层的目的在于提取低层次的特征，比如简单的边和纹理。其中 Max-pooling层使得卷积的输出对微小的偏移情况更加鲁棒。但没有用太多的Max-pooling层，因为太多的Max-pooling层会使得网络 损失图像信息。
  紧接着的三层都是使用参数不共享的卷积核，之所以使用参数不共享，有如下原因： 对齐的人脸图片中，不同的区域会有不同的统计特征，卷积的局部稳定性假设并不存在，所以使用相同的卷积核会导致信息的丢失 不共享的卷积核并不增加抽取特征时的计算量，而会增加训练时的计算量 使用不共享的卷积核，需要训练的参数量大大增加，因而需要很大的数据量，然而这个条件本文刚好满足。
  全连接层将上一层的每个单元和本层的所有单元相连，用来捕捉人脸图像不同位置的特征之间的相关性。其中，第7层（4096-d）被用来表示人脸。全连接层的输出可以用于Softmax的输入，Softmax层用于分类。
  ```

* 最终的人脸验证环节
  * 内积
  * 卡方加权（加权系数通过SVM训练出来）
  * siamese网络
    * 向NN输入两张图片，最后通过一个逻辑单元得到是否相似

### 3. FaceNet

citation:`F. Schroff, D. Kalenichenko, and J. Philbin. Facenet: A unified embedding for face recognition and clustering. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, pages 815–823, 2015.`

在理想状况下，我们希望使用“vector representation”之间的距离就可以直接反映人脸的相似度

* Triplet Loss

* Triplet selection

  ```
  想要快速收敛，在mini-batch中hard positive/hard negative
  hard negative可能会在开始阶段就收敛到local optinum
  negative选择semi-hard
  
  在一个minibatch中，我们根据当时的embedding，选择一次三元组，在这些三元组上计算triplet-loss, 再对embedding进行更新，不断重复，直到收敛或训练到指定迭代次数。
  ```

* network architecture

  ```
  1. Zeiler&Fergus [22] architecture
  
  M. D. Zeiler and R. Fergus. Visualizing and understanding convolutional networks. CoRR, abs/1311.2901, 2013. 2, 3, 4, 6
  
  2. Inception model
  
  C. Szegedy, W. Liu, Y. Jia, P. Sermanet, S. Reed, D. Anguelov, D. Erhan, V. Vanhoucke, and A. Rabinovich.
  Going deeper with convolutions. CoRR, abs/1409.4842, 2014. 2, 3, 4, 5, 6, 10 
  ```

### 4. Deep Face Recognition

citation:`O. M. Parkhi, A. Vedaldi, and A. Zisserman. Deep face recognition. In BMVC, 2015.`

* 构建一个人脸识别数据库,构建过程主要是程序实现的，少量人工参与
  1. 引导和过滤候选身份名称列表 (在IMDB网站上获得名人名字的名单列表)
  2. 通过谷歌来扩大每个人的图片量
  3. 使用一个Fisher Vector Faces descriptor 与SVM清除一些错误的图片
  4. 清除一些一样的照片，因为从不同网站上爬下来的照片，难免会有所重复
  5. 再人工清除一些照片 (使用一个CNN进行辅助，每个identity按照softmax的大小从高到低进行排列)
* 通过对比各种CNN网络，提出了一个简单有效的CNN网络，在各种公开的人脸识别数据库上得到很好的效果
  * 使用softmax loss和triplet loss去训练网络configuration A
  * 使用迁移学习训练configuration B 和 D的最后几层全连接层

### 5. A Discriminative Feature Learning Approach for Deep Face Recognition

citation: `Y. Wen, K. Zhang, Z. Li, and Y. Qiao. A discriminative feature learning approach for deep face recognition. In ECCV, 2016.`

* center loss

  * 得到discriminative features
  * 类间距离变大，类内距离变小
  * $L = L_s + \lambda L_c$

  ```
  center loss保留原有的分类模型，为每个类定义一个class center，同一个class的图像对应的特征都应该尽量靠近自己的class center，不同类的class center尽量远离
  
  不需要控制采样的方法
  ```

* 使用center loss + softmax loss（center loss作为正则项）

  * 每一轮先更新loss layer的参数W，在更新每一个类别的class center（通过计算其梯度），最后更新CNN的参数
  * The hyper parameter λ dominates the intra-class variations and α controls the learning rate of center c in model C. 
  * 取得了high accuracy，并且使用少量的训练数据和简单的网络结构

* 单纯使用softmax loss，只能得到separable features

* 使用triplet loss不稳定，难收敛，计算复杂度大



## 其他文献

### 1. Visualizing and understanding convolutional networks (未读)

citation: `M. D. Zeiler and R. Fergus. Visualizing and understanding convolutional neural networks. In ECCV, 2014.`



### 2. Mobilenets: Efficient convolutional neural networks for mobile vision applications (未读)

citation: `A. G. Howard, M. Zhu, B. Chen, D. Kalenichenko, W. Wang, T. Weyand, M. Andreetto, and H. Adam. Mobilenets: Efficient convolutional neural networks for mobile vision applications. arXiv:1704.04861, 2017.`

### 3 How transferable are features in deep neural networks?

citation: `J. Yosinski, J. Clune, Y. Bengio, and H. Lipson. How transferable are features in deep neural networks? In NIPS, 2014.`

* 神经网络的前3层基本都是general feature，进行迁移的效果会比较好；
* 深度迁移网络中加入fine-tune，效果会提升比较大，可能会比原网络效果还好；
* Fine-tune可以比较好地克服数据之间的差异性；

* 深度迁移网络要比随机初始化权重效果好；

* 网络层数的迁移可以加速网络的学习和优化。
  