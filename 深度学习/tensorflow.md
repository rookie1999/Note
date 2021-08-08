> 视频网址：https://www.bilibili.com/video/BV1B7411L7Qt
>
> 代码位置：D:\pycharm\code\tf_study



# Tensorflow

## 1 环境安装

* Anacode 环境配置

  ```
  conda create -n tf_study python=3.7
  conda install cudatoolkit=10.1
  conda install cudnn=7.6.5
  pip install tensorflow=2.1
  conda install scikit-learn=0.24.1
conda install pandas=1.1.5
  ```

  如果电脑有Nvidia GPU，可以下载cudatoolkit和cudnn实现GPU加速

  验证是否安装成功
  
  ```python
  import tensorflow as tf
  tf.__version__  # '2.1.0'
  ```

## 2 搭建第一个神经网络训练模型

### 2.1 人工智能的三个学派

人工智能： 让机器具备人的思维和意识

人工智能有三个学派

* 行为主义：基于控制论，构建感知-动作控制系统
  * 控制论，如平衡、行走、避障等自适应系统
* 符号主义：基于算数逻辑表达式，求解问题时先把问题描述为表达式，再求解表达式
  * 可用公式描述、实现理性思维，如专家系统
* 连接主义：仿生学，模仿神经元连接关系
  * 仿脑神经元连接、实现感性思维，如神经网络

**基于连接主义的神经网络设计过程**

1. 准备数据

   * 采集大量“特征/标签”数据

2. 搭建网络

   * 搭建神经网络结构

3. 优化参数

   * 训练网络获取最佳参数（反向传播）

4. 应用网络

   * 将网络保存为模型，输入新数据，输出分类或预测结果（前向传播）

   * 输出的概率值中最大的一个就是结果

### 2.2 常用函数

* tf.constant

* tf.convert_to_tensor

* tf.zeros

* tf.ones

* tf.fill

* tf.random.normal

* tf.random.truncated_normal

  * 值均在μ±2σ之间

* tf.random.uniform

* tf.cast

* tf.reduce_min

* tf.reduce_max

* tf.reduce_mean

* tf.reduce_sum

* tf.add

* tf.subtract

* tf.multiply

* tf.divide

* tf.pow

* tf.sqrt

* tf.square

* tf.matmul

  * 矩阵乘法

* tf.data.Dataset.from_tensor_slices

  * 切分传入张量的第一维度，生成输入特征/标签对

* tf.Variable

  * 可训练的变量

* tf.GradientTape()

  * ```python
    with tf.GradientTape() as tape:
        loss = tf.pow(w, 2)  # loss = w^2, dw = 2w
    grad = tape.gradient(loss, w)  # loss函数对w求导的梯度
    ```

* tf.one_hot

* tf.nn.softmax

* tf.assign_sub

* tf.argmax

  * 返回最大值的索引值

* tf.where(expr, a, b)

  * 如果布尔表达式expr为真，返回a；为假，返回b

* np.mgrid[x起始:x结束:x步长, y起始:y结束:y步长]

  * 返回两个二维数组x, y
  * x, y 含有相同形状的一维数组

* np.ravel()

  * 把矩阵变成一维数组

* np.c_[数组1, 数组2, ...]  

  * 将数组的元素配对

* tf.nn.softmax_cross_entropy_with_logits(y_true, y_predict)

  * 直接返回通过softmax函数之后的交叉熵loss

* tf.nn.l2_loss

  * L2正则化

* tf.boolean_mask(tensor, mask, axis=None)

* tf.clip_by_value(v, a, b)

  * 将v限制在[a, b]之间。如果小于a，返回a；大于b，返回b

### 2.3 神经网络实现鸢尾花分类

1. 准备数据

   1.1 数据集读入

   1.2 数据集乱序

   1.3 生成训练集和测试集

   1.4 配成（输入特征，标签）对，每次读入一小批（batch）

2. 搭建网络

   2.1 定义神经网络中所有可训练参数

3. 参数优化

   3.1 嵌套循环，with结构更新参数，显示当前loss

4. 测试效果

   4.1 计算当前参数前向传播后的准确率，显示当前acc

5. acc/loss可视化

```python
# Time: 4/21/2021 9:06 PM
# 利用鸢尾花数据集，实现前向传播、反向传播，可视化曲线
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
from sklearn import datasets

# 1.1 读入数据
dataset = datasets.load_iris()
X = dataset.data
y = dataset.target
# 1.2 数据集乱序
np.random.seed(116)
shuffle_index = np.random.permutation(X.shape[0])
X = X[shuffle_index]
y = y[shuffle_index]
# 1.3 生成训练集和测试集
X_train = X[:-30]
y_train = y[:-30]
X_test = X[-30:]
y_test = y[-30:]
# 1.4 数据类型转换，为了之后矩阵乘法不报错
X_train = tf.cast(X_train, dtype=tf.float32)
X_test = tf.cast(X_test, dtype=tf.float32)
# 1.5 生成(输入特征,标签)对，并生成以batch为单位（把数据集分批次，每个批次batch组数据）
train_db = tf.data.Dataset.from_tensor_slices((X_train, y_train)).batch(32)
test_db = tf.data.Dataset.from_tensor_slices((X_test, y_test)).batch(32)

# 2.1 定义所有可训练的参数
# 定义神经网络的可训练参数
# 4个输入特征3分类，所以input layer 4 units, output layer 3 units
# 用tf.Variable()标记参数可训练
w1 = tf.Variable(tf.random.truncated_normal([4, 3], stddev=0.1, seed=1))
b1 = tf.Variable(tf.random.truncated_normal([3], stddev=0.1, seed=1))

# 2.2 定义NN的超参数
learning_rate = 0.1  # 学习率
epoch = 500  # 循环500轮数据集
train_loss_res = []  # 将每轮的loss记录在此列表中
test_acc = []  # 将每轮的acc记录在此列表中
loss_all = 0  # 记录每一个epoch内的不同batch的loss和

# 3.1 训练网络
for epoch in range(epoch):
    for step, (x_train, y_train) in enumerate(train_db):  # batch级别的循环
        # x_train.shape=(batch, n)
        # y_train.shape=(batch,)
        with tf.GradientTape() as tape:
            # 前向传播
            y = tf.matmul(x_train, w1) + b1
            y = tf.nn.softmax(y)
            # 真实标签值的独热码
            y_hat = tf.one_hot(y_train, depth=3)
            # 均方误差mse
            loss = tf.reduce_mean(tf.square(y_hat - y))
            loss_all += loss
        # 计算每个参数的梯度
        grads = tape.gradient(loss, [w1, b1])
        # 更新参数
        w1.assign_sub(learning_rate * grads[0])
        b1.assign_sub(learning_rate * grads[1])

    # 每个epoch，打印loss信息
    loss_average = loss_all / 4
    print("Epoch {0}, loss: {1}".format(epoch, loss_average))
    train_loss_res.append(loss_average)
    loss_all = 0

    # 测试部分，测试每一个epoch时参数修改后模型的性能
    # total_correct为预测正确的样本个数，total_number为测试的总样本数
    total_correct, total_number = 0, 0
    for x_test, y_test in test_db:
        # 先进行预测
        y = tf.matmul(x_test, w1) + b1
        y = tf.nn.softmax(y)
        pred = tf.argmax(y, axis=1)  # 返回每行概率的最大值的索引，即预测的分类
        pred = tf.cast(pred, dtype=y_test.dtype)
        correct = tf.cast(tf.equal(pred, y_test), dtype=tf.int32)
        total_correct += tf.reduce_sum(correct)
        total_number += x_test.shape[0]
    # 每一轮的总的准确率
    acc = total_correct / total_number
    test_acc.append(acc)
    print("Test_acc", acc)
    print("-" * 20)

# 绘制loss曲线
plt.title('Loss Function Curve')
plt.xlabel('Epoch')
plt.ylabel('Loss')
plt.plot(train_loss_res, label='$loss$')  # 画出train_loss_res，连线的图标是loss
plt.legend()
plt.show()

# 绘制Accuracy曲线
plt.title('Acc Curve')
plt.xlabel('Epoch')
plt.ylabel('Acc')
plt.plot(test_acc, label='$Accuracy$')
plt.legend()
plt.show()
```

## 3 神经网络的优化方法

### 3.1 神经网络(NN)的复杂度

> NN复杂度：多用NN层数和NN参数的个数表示

* 空间复杂度
  * 层数 = 隐藏层的层数 + 1个输出层
    * 计算具有运算能力的层数
  * 总参数 = 总w + 总b
* 时间复杂度
  * 乘加运算的次数

### 3.2 欠拟合和过拟合

* 欠拟合的解决办法
  * 增加输入特征项
  * 增加网络参数
  * 减少正则化参数
* 过拟合的解决办法
  * 数据清洗（减少噪声）
  * 增大训练集
  * 采用正则化
  * 增大正则化参数

### 3.3 神经网络参数优化器

待优化参数w，损失函数loss，学习率lr，每次迭代一个batch，t代表当前batch迭代的总次数

1. 计算t时刻损失函数关于当前参数的梯度$g_t = \Delta{ loss}=\frac{\part loss}{\part{w_t}}$
2. 计算t时刻的一阶动量$m_t$和二阶动量$V_t$
   * 一阶动量：与梯度相关的函数
   * 二阶动量：与梯度平方相关的函数
3. 计算t时刻下降梯度$\eta_t = lr·m_t/\sqrt{V_t}$
4. 计算t+1时刻参数$w_{t+1} = w_t - \eta_t = w_t - lr·m_t/\sqrt{V_t}$

#### 3.3.1 随机梯度下降SGD

$m_t = g_t,\quad V_t = 1$

$\eta_t = lr·m_t/\sqrt{V_t} = lr·g_t$

$w_{t+1} = w_t - \eta_t = w_t - lr·g_t$

#### 3.3.2 SGDM

在SGD基础上增加了一阶动量

$m_t = \beta·m_{t-1} + (1-\beta)·g_t,\quad V_t = 1$

$\eta_t = lr·m_t/\sqrt{V_t}=lr·(\beta·m_{t-1} + (1-\beta)·g_t)$

$w_{t+1} = w_t - \eta_t = w_t - lr·(\beta·m_{t-1} + (1-\beta)·g_t)$

```python
# 初始化参数
m_w, m_b = 0, 0
beta = 0.9

# sgd-momentum
m_w = beta * m_w + (1 - beta) * grad[0]
m_b = beta * m_w + (1 - beta) * grad[1]
w1.assign_sub(lr * m_w)
b1.assign_sub(lr * m_b)
```

#### 3.3.3 Adagrad

在SGD的基础上增加了二阶动量

$m_t = g_t,\quad V_t = \sum_{\tau=1}^tg_{\tau}^2$

$\eta_t = lr·m_t/(\sqrt{V_t}) = lr·g_t / (\sqrt{\sum_{\tau=1}^{t}g_{\tau}^2})$

$w_{t+1} = w_t - \eta_t$

```python
# 初始化参数
v_w = 0, v_b = 0

# adagrad
v_w += tf.square(grads[0])
v_b += tf.square(grads[1])
w1.assign_sub(lr * grads[0] / tf.sqrt(v_w))
b1.assign_sub(lr * grads[1] / tf.sqrt(v_b))
```

#### 3.3.4 RMSProp

在SGD的基础上增加了二阶动量

$m_t = g_t,\quad V_t = \beta·V_{t-1} + (1 - \beta)g_t^2$

$\eta_t = lr·m_t/\sqrt{V_t} = lr·g_t/\sqrt{\beta·V_{t-1}+(1-\beta)g_t^2}$

$w_{t+1} = w_t - \eta_t = w_t - lr·g_t/\sqrt{\beta·V_{t+1}+(1-\beta)·g_t^2}$ 

```python
# 初始化参数
v_w, v_b = 0, 0
beta = 0.9

# adadelta
v_w = beta * v_w + (1 - beta) * tf.square(grads[0])
v_b = beta * v_b + (1 - beta) * tf.square(grads[1])
w1.assign_sub(lr * grads[0] / tf.sqrt(v_w))
b1.assign_sub(lr * grads[1] / tf.sqrt(v_b))
```

#### 3.3.5 Adam

同时结合SGDM一阶动量和RMSProp二阶动量

$m_t = \beta_1·m_{t-1} + (1-\beta_1)·g_t$

修正一阶动量的偏差：$\hat{m_t} = \frac{m_t}{1-\beta_1^t}$

$V_t = \beta_2·V_{step-1} + (1-\beta_2)·g_t^2$

修正二阶动量的偏差：$\hat{V_t} = \frac{V_t}{1-\beta_2^t}$

$\eta_t = lr·\hat{m_t}/\sqrt{\hat{V_t}} = lr·\frac{m_t}{1-\beta_1^t}/\sqrt{\beta_2·V_{step-1} + (1-\beta_2)·g_t^2}$

$w_{t+1} = w_t - \eta_t = w_t - lr·\frac{m_t}{1-\beta_1^t}/\sqrt{\frac{V_t}{1-\beta_2^t}}$

```python
# 初始化参数
m_w, m_b = 0, 0
v_w, v_b = 0, 0
beta1, beta2 = 0.9, 0.999
delta_w, delta_b = 0, 0
global_step = 0

# adam
m_w = beta1 * m_w + (1 - beta1) * grads[0]
m_b = beta1 * m_b + (1 - beta1) * grads[1]
v_w = beta2 * v_w + (1 - beta2) * tf.square(grads[0])
v_b = beta2 * v_b + (1 - beta2) * tf.square(grads[1])

m_w_correction = m_w / (1 - tf.pow(beta1, int(global_step)))
m_b_correction = m_b / (1 - tf.pow(beta1, int(global_step)))
v_w_correction = v_w / (1 - tf.pow(beta2, int(global_step)))
v_b_correction = v_b / (1 - tf.pow(beta2, int(global_step)))

w1.assign_sub(lr * m_w_correction / tf.sqrt(v_w_correction))
b1.assign_sub(lr * m_b_correction / tf.sqrt(v_b_correction))
```

## 3 使用八股搭建神经网络

### 3.1 搭建网络八股Sequential

用Tensorflow API：tf.keras 搭建网络八股

**顺序网络结构的六步法**

1. import
2. train, test
3. model = tf.keras.models.Sequential
4. model.compile
5. model.fit
6. model.summary

* model = tf.keras.models.Sequential([网络结构])  

  * 描述各层网络

  * ```
    网络结构举例：
    拉直层:  tf.keras.layers.Flatten()
    全连接层:  tf.keras.layers.Dense(神经元个数, activation='激活函数', kernel_regularizer=哪种正则化)
    	activation:'relu', 'sigmoid', 'softmax', 'tanh'
    	kernel_regularization:  
    		tf.keras.regularizers.l1()
    		tf.keras.regularizers.l2()
    卷积层:  tf.keras.layers.Conv2D(filters=卷积核个数, kernel_size='卷积核尺寸', 			strides=卷积步长, padding='valid' or 'same')
    LSTM层:  tf.keras.layers.LSTM()
    ```

* model = compile(optimizer=优化器, loss=损失函数, metrics=['准确率'])

  * ```
    Optimizer可选:
    	'sgd' or tf.keras.optimizers.SGD(lr=学习率, momentum=动量参数)
    	'adagrad' or tf.keras.optimizers.Adagrad(lr=学习率)
    	'adadelta' or tf.keras.optimizers.Adadelta(lr=学习率)
    	'adam' or tf.keras.optimizers.Adam(lr=学习率, beta_1=0.9, beta_2=0.999)
    loss可选:
    	'mse' or tf.keras.losses.MeanSquaredError()
    	'sparse_categorical_crossentropy' or tf.keras.losses.
    			SparseCategoricalCrossentropy(from_logits=false)
    		注：from_logits为True，表示输出是以原始输出，False是输出以softmax的概率分布
    metrics可选:
    	'accuracy':  y_和y都是数值，如y_ = [1], y = [1]
    	'categorical_accuracy': y_和y都是独热码或者概率分布
    		如y = [0, 1, 0], y_ = [0.256, 0.695, 0.048]
    	'sparse_categorical_accuracy': y_是数值，y是独热码或者概率分布
    注：y_是真实值
    ```

* model.fit(训练集的输入特征, 训练集的标签, batch_size= , epochs= ,validation_data=(测试集的输入特征, 测试集的输出特征), validation_split=从训练集划分多少比例给测试集, validation_freq=多少次epoch测试一次)

  * validation_data和validation_split二者选其一
  * fit函数返回对象是history，封装了loss和acc一些参数

* model.summary()

  * 打印网络的详细信息

```python
# 1. import
import numpy as np
import tensorflow as tf
from sklearn import datasets

# 2. train test
dataset = datasets.load_iris()
x_train = dataset.data
y_train = dataset.target
# 数据乱序
np.random.seed(116)
shuffle_idx = np.random.permutation(x_train.shape[0])
x_train = x_train[shuffle_idx]
y_train = y_train[shuffle_idx]

# 3. models.Sequential
model = tf.keras.models.Sequential([tf.keras.layers.Dense(3, activation='softmax',
                                                          kernel_regularizer=tf.keras.regularizers.l2())])
# 4. model.compile
model.compile(optimizer=tf.keras.optimizers.SGD(lr=0.1),
             loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=False),
              metrics=['sparse_categorical_accuracy'])

# 5. model.fit
model.fit(x_train, y_train, batch_size=32, epochs=50, validation_split=0.2, validation_freq=20)

#6. model.summary
model.summary()
```

**跳连的非顺序网络结构的六步法**

使用类class搭建神经网络结构

1. import
2. train, test
3. class MyModel(Model) model = MyModel
4. model.compile
5. model.fit
6. model.summary

```python
class MyModel(Model):
    def __init__(self):
        super(MyModel, self).__init__()
        # 定义网络结构块的代码
        
    def call(self, x):
		# 调用网络结构块，实现前向传播
        return y
    
model = MyModel()

# Example
class IrisModel(Model):
    def __init__(self):
        super(IrisModel, self).__init__()
        self.d1 = Dense(3)
       
    def call(self, x):
        y = self.d1(x)
        return y
```

### 3.2 MNIST数据集

提供6万张28*28像素的0-9手写数字识别图片和标签，用于训练，1万张用于测试

```python
# 导入MNIST数据集
from tf.keras.datasets import mnist
(x_train, y_train), (x_test, y_test) = mnist.load_data()
```

使用Sequential神经网络训练Mnist数据集

```python
# 1. import
import tensorflow as tf
from tensorflow.keras import datasets

# 2. train  test
(x_train, y_train), (x_test, y_test) = datasets.mnist.load_data()
x_train = x_train / 255
x_test = x_test / 255

# 3. models.Sequential
model = tf.keras.models.Sequential([
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dense(10, activation='softmax')
])

# 4. model.compile
model.compile(optimizer=tf.keras.optimizers.Adam(), loss=[tf.keras.losses.SparseCategoricalCrossentropy(from_logits=False)],
              metrics=['sparse_categorical_accuracy'])

# 5. model.fit
model.fit(x_train, y_train, batch_size=32, epochs=5, validation_data=(x_test, y_test), validation_freq=1)

# 5. model.summary
model.summary()
```

### 3.3 FASHION数据集

提供6万张28*28像素的0-9手写数字识别图片和标签，用于训练，1万张用于测试

```python
# 导入FASHION数据集
fashion = tf.keras.datasets.fashion_mnist
(x_train, y_train), (x_test, y_test) = fashion.load_data()
```

## 4 tf.keras搭建神经网络八股

1. 自制数据集，解决本领域应用
2. 数据增强，扩充数据集
3. 断点续训，存取模型
4. 参数提取，把参数传入文本
5. acc/loss可视化，查看训练效果
6. 应用程序，给图识物

### 4.1 自制数据集

* 将数据集放入文件家中，同时将数据集中的文件与其对应的标签存入txt文件中
* 生成数据集

```python
import numpy as np
from PIL import Image

train_path = '../class4/MNIST_FC/mnist_image_label/mnist_train_jpg_60000/'
train_txt = '../class4/MNIST_FC/mnist_image_label/mnist_train_jpg_60000.txt'
x_train_savepath = '../class4/MNIST_FC/mnist_image_label/mnist_x_train.npy'
y_train_savepath = '../class4/MNIST_FC/mnist_image_label/mnist_y_train.npy'

test_path = '../class4/MNIST_FC/mnist_image_label/mnist_test_jpg_10000/'
test_txt = '../class4/MNIST_FC/mnist_image_label/mnist_test_jpg_10000.txt'
x_test_savepath = '../class4/MNIST_FC/mnist_image_label/mnist_x_test.npy'
y_test_savepath = '../class4/MNIST_FC/mnist_image_label/mnist_y_test.npy'

def generate_datasets(path, txt):
    # 返回path下的数据集与其对应的标签
    # path -- 图片路径
    # txt -- 存有图片名和对应的标签值
    f = open(txt, 'r')
    contents = f.readlines()
    f.close()
    x, y_ = [], []
    for content in contents:
        value = content.split()  # 以空格隔开，路径名和标签值
        img_path = path + value[0]
        img = Image.open(img_path)  # 读入图片
        img = np.array(img.convert('L'))  # 图片变为8位宽灰度值的np.array格式
        img = img / 255.  # 数据归一化
        x.append(img)
        y_.append(value[1])
        print('loading:', content)
    x = np.array(x)
    y_ = np.array(y_).astype(np.int64)
    return x, y_


if os.path.exists(x_train_savepath) and os.path.exists(y_train_savepath) and os.path.exists(
        x_test_savepath) and os.path.exists(y_test_savepath):
    print('-------Load Datasets-------')
    x_train_save = np.load(x_train_savepath)
    y_train = np.load(y_train_savepath)
    x_test_save = np.load(x_test_savepath)
    y_test = np.load(y_test_savepath)
    x_train = x_train_save.reshape(len(x_train_save), 28, 28)
    x_test = x_test_save.reshape(len(x_test_save), 28, 28)
else:
    print('-------Generate Datasets-------')
    x_train, y_train = generate_datasets(train_path, train_txt)
    x_test, y_test = generate_datasets(test_path, test_txt)
```

### 4.2 数据增强 Data Augmentation

是一种在Computer Vision中常用的一种增大数据量的方法

```python
image_gen_train = tf.keras.preprocessing.image.ImageDataGenerator(
    rescale=所有数据都将乘以该值,
	rotation_range=随机旋转角度数范围,
	width_shift_range=随机宽度偏移量,
	height_shift_range=随机高度偏移量,
	# 水平翻转
	horizontal_flip=是否随机水平翻转,
	# 随机缩放
	zoom_range=随机缩放的范围[1-n, 1+n])
image_gen_train.fit(x_train)  # 参数是四维数据，最后一维是通道数
x_train_augmented = image_gen_train.flow(x_train, y_train, batch_size=32)


# example
image_gen_train = ImageDataGenerator(
    rescale=1. / 255,
    rotation_range=45,
    width_shift_range=.15,
    height_shift_range=.15,
    horizontal_flip=False,
    zoom_range=0.5
)
```

### 4.3 断点续训

用来存取模型

**读取模型**

* model.load_weights(路径文件名)

```python
checkpoint_save_path = './checkpoint/mnist.ckpt'
if os.path.exists(checkpoint_save_path + '.index'):
    # 如果模型之前保存过(即生成ckpt文件)，会生成参数对应的索引表文件
    print('------Load the model------')
    model.load_weights(checkpoint_save_path)
```

**保存模型**

```python
# 先定义回调函数，指定一系列的参数
cp_callback = tf.keras.callbacks.ModelCheckpoint(filepath=路径文件名,
                                                save_weights_only=True/False,
                                                save_best_only=True/False)

# 在调用model.fit的时候指定callbacks参数
history = model.fit(callbacks=[cp_callback])


# example
cp_callback = tf.keras.callbacks.ModelCheckpoint(filepath=checkpoint_save_path,
                                                save_weights_only=True,
                                                save_best_only=True)
history = model.fit(x_train, y_train, batch_size=32, epochs=5, validation_data=(x_test, y_test), validation_freq=1, callbacks=[cp_callback])
```

### 4.4 参数提取

把参数存入文本

**提取可训练参数**

* model.trainable_variables

**设置print输出格式**

由于当数据很大的时候，print打印的输出中会包含省略号

```python
np.set_printoptions(threshold=超过多少显示省略号)

# example
np.set_printoptions(threshold=np.inf)  # np.inf表示无限大
```

**把参数存入文本**

```python
print(model.trainable_variables)
file = open('./weights.txt', 'w')
for v in model.train_variables:
    file.write(str(v.name) + '\n')
    file.write(str(v.shape) + '\n')
    file.write(str(v.numpy()) + '\n')
file.close()
```

### 4.5 acc/loss可视化

```python
history = model.fit(训练集数据, 训练集标签, batch_size= , epochs= ,
                   validation_split=训练集中用作测试集的比例, 
                   validation_data=(测试集数据, 测试集标签), 
                   validation_freq=测试频率)
```

history:

* 训练集loss：loss
* 测试集loss：val_loss
* 训练集准确率：sparse_categorical_accuracy
* 测试集准确率：val_sparse_categorical_accuracy

```python
# 显示训练集和验证集的acc和loss曲线
acc = history.history['sparse_categorical_accuracy']
val_acc = history.history['val_sparse_categorical_accuracy']
loss = history.history['loss']
val_loss = history.history['val_loss']

plt.subplot(1, 2, 1)  # 1行2列，画出第一列
plt.plot(acc, label='Training Accuracy')
plt.plot(val_acc, label='Validation Accuracy')
plt.title('Training and Validation Accuracy')
plt.legend()

plt.subplot(1, 2, 2)  # 画出第二列
plt.plot(loss, label='Training Loss')
plt.plot(val_loss, label='Validation Loss')
plt.title('Training and Validation Loss')
plt.legend()
plt.show()
```

### 4.6 前向传播执行应用

* predict(输入特征, batch_size=整数)
  * 返回前向传播计算结果

1. 复现模型

   ```python
   model = tf.keras.models.Sequential([
       tf.keras.layers.Flatten(),
       tf.keras.layers.Dense(128, activation='relu'),
       tf.keras.layers.Dense(10, activation='softmax')
   ])
   ```

2. 加载参数

   ```python
   model.load_weights(model_save_path)
   ```

3. 预测结果

   ```python
   result = model.predict(x_predict)
   ```

## 5 CNN

### 5.1 感受野

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_1.png)

一个`5*5`的原始输入经过`3*3`的卷积核得到`3*3`的输出，此时输出的一个像素点在原始输入的映射区域是`3*3`，感受野是3

再经过一个`3*3`的卷积核得到`1*1`的输出，此时输出的感受野是1

如果一个`5*5`的原始输入直接经过一个`5*5`的卷积核，得到`1*1`的输出，此时输出的一个像素点在原始输入的映射区域是`5*5`，感受野是5

但是，由于两个`3*3`和一个`5*5`的卷积核参数个数不同，进行的运算量也不同（一般采用两个`3*3`）

### 5.2 TF描述卷积计算层

```python
tf.keras.layers.Conv2D(
	filters=卷积核个数,
	kernel_size=卷积核尺寸, # 正方形写核长整数，或(核高h, 核宽w)
	strides=滑动步长, #横纵向相同写步长整数，或(纵向步长h, 横向步长w)
    padding='same' or 'valid',  # valid是默认值
    activation='relu' or 'sigmoid' or 'tanh' or 'softmax',  # 如果有BN,则此处不写激活函数
    input_shape=(高, 宽, 通道数)  # 输入特征图维度，可省略
)
```

实例

```python
model = tf.keras.models.Sequential([
    tf.keras.layers.Conv2D(6, 5, padding='valid', activation='sigmoid'),
    MaxPool2D(2, 2),
    Conv2D(6, (5, 5), padding='valid', activation='sigmoid'),
    MaxPool2D(2, (2, 2))
    Conv2D(filters=6, kernel_size=(5, 5), padding='valid', activation='sigmoid'),  # 推荐使用
    MaxPool2D(pool_size=(2, 2), strides=2),
    Flatten(),
    Dense(10, activation=(2, 2), strides=2)
])
```

### 5.3 Batch Normalization

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_2.png)

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_3.png)

由于进行了批标准化（Batch Normalization），使得数据完全符合正态分布，而失去原来的非线性特性，因此，加入两个可训练参数：缩放因子$\gamma_k$和偏移因子$\beta_k$

BN层位于卷积层之后，激活层之前

```python
model = tf.keras.models.Sequential([
    Conv2D(filters=6, kernel_size=(5, 5), padding='same'),  # 卷积层
    BatchNormalization(),  # 卷积层
    Activation('relu'),  # 激活层
    MaxPool2D(pool_size=(2, 2), strides=2, padding='same'),
    Dropout(0.2),  # dropout层
])
```

### 5.4 池化Pooling

池化操作用于减少特征数据量

最大值池化可提取图片纹理，均值池化可保留背景特征

```python
tf.keras.layers.MaxPool2D(
	pool_size=池化核尺寸,  # square写核长整数,或(核高h,核宽w)
    strides=池化步长,  # 步长整数，或(纵向步长h，横向步长w),默认是pool_size
    padding='valid' or 'same'  # valid是默认值
)
tf.keras.layers.AveragePool2D(
	pool_size=池化核尺寸,  # square写核长整数,或(核高h,核宽w)
    strides=池化步长,  # 步长整数，或(纵向步长h，横向步长w),默认是pool_size
    padding='valid' or 'same'  # valid是默认值
)
```

### 5.5 卷积神经网络

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_4.png)

卷积就是特征提取器，就是CBAPD

### 5.6 Cifar10数据集

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_5.png)

```python
# 1. import
import os
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
from tensorflow.keras import Model
from tensorflow.keras.layers import Conv2D, BatchNormalization, Activation, MaxPool2D, Dropout, Flatten, Dense

np.set_printoptions(threshold=np.inf)

# 2. train test
cifar = tf.keras.datasets.cifar10
(x_train, y_train), (x_test, y_test) = cifar.load_data()
x_train, x_test = x_train / 255., x_test / 255.


# 3. model
class BaselineModel(Model):
    def __init__(self):
        super(BaselineModel, self).__init__()
        self.conv1 = Conv2D(filters=6, kernel_size=(5, 5), strides=1, padding='same')
        self.bn1 = BatchNormalization()
        self.a1 = Activation('relu')
        self.p1 = MaxPool2D(pool_size=(2, 2), strides=2, padding='same')
        self.d1 = Dropout(0.2)
        self.f1 = Flatten()
        self.n1 = Dense(128, activation='relu')
        self.d2 = Dropout(0.2)
        self.n2 = Dense(10, activation='softmax')

    def call(self, inputs, training=None, mask=None):
        x = self.conv1(inputs)
        x = self.bn1(x)
        x = self.a1(x)
        x = self.p1(x)
        x = self.d1(x)

        x = self.f1(x)
        x = self.n1(x)
        x = self.d2(x)
        y = self.n2(x)
        return y


model = BaselineModel()

# 4. model.compile
model.compile(optimizer='adam', loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=False),
              metrics=['sparse_categorical_accuracy'])
# 断点续训
model_save_path = './baseline.ckpt'
if os.path.exists(model_save_path + '.index'):
    print('--------Load the model--------')
    model.load_weights(model_save_path)
cp_callback = tf.keras.callbacks.ModelCheckpoint(filepath=model_save_path, save_weights_only=True, save_best_only=True)

# 5. model.fit
history = model.fit(x_train, y_train, epochs=5, batch_size=32, validation_data=(x_test, y_test),
                    validation_freq=1, callbacks=[cp_callback])

# 6. model.summary
model.summary()

# 参数提取
print(model.trainable_variables)
file = open('./baseline_weights.txt', 'w')
for v in model.trainable_variables:
    file.write(str(v.name) + '/n')
    file.write(str(v.shape) + '/n')
    file.write(str(v.numpy()) + '/n')
file.close()

# 显示训练集和验证集的acc/loss曲线
acc = history.history['sparse_categorical_accuracy']
val_acc = history.history['val_sparse_categorical_accuracy']
loss = history.history['loss']
val_loss = history.history['val_loss']

plt.subplot(1, 2, 1)
plt.plot(acc, label='Training Accuracy')
plt.plot(val_acc, label='Validation Accuracy')
plt.title('Training and Validation Accuracy')
plt.legend()

plt.subplot(1, 2, 2)
plt.plot(loss, label='Training Loss')
plt.plot(val_loss, label='Validation Loss')
plt.title('Training and Validation Loss')
plt.legend()
plt.show()
```

### 5.7 LeNet5

一共有五层计算单元（卷积层的计算单元包括卷积和池化）

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_6.png)

原始的LeNet当时还没有Batch Normalization和Dropout层，流行的激活函数是sigmoid

```python
class LeNet5(Model):
    def __init__(self):
        super(LeNet5, self).__init__()
        self.conv_1 = Conv2D(filters=6, kernel_size=(5, 5), strides=1, padding='valid', activation='sigmoid')
        self.pool_1 = MaxPool2D(pool_size=(2, 2), strides=2, padding='valid')

        self.conv_2 = Conv2D(filters=16, kernel_size=(5, 5), strides=1, padding='valid', activation='sigmoid')
        self.pool_2 = MaxPool2D(pool_size=(2, 2), strides=2, padding='valid')

        self.flatten = Flatten()
        self.n1 = Dense(120, activation='sigmoid')
        self.n2 = Dense(84, activation='sigmoid')
        self.n3 = Dense(10, activation='softmax')

    def call(self, inputs, training=None, mask=None):
        x = self.conv_1(inputs)
        x = self.pool_1(x)

        x = self.conv_2(inputs)
        x = self.pool_2(x)

        x = self.flatten(x)
        x = self.n1(x)
        x = self.n2(x)
        y = self.n3(x)
        return y
```

### 5.8 AlexNet8

一共有8层计算单元

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_7.png)

```python
class AlexNet8(Model):
    def __init__(self):
        super(AlexNet8, self).__init__()
        self.conv_1 = Conv2D(filters=96, kernel_size=(3, 3), strides=1, padding='valid')
        self.bn_1 = BatchNormalization()
        self.act_1 = Activation('relu')
        self.pool_1 = MaxPool2D(pool_size=(3, 3), strides=2)

        self.conv_2 = Conv2D(filters=256, kernel_size=(3, 3), strides=1, padding='valid')
        self.bn_2 = BatchNormalization()
        self.act_2 = Activation('relu')
        self.pool_2 = MaxPool2D(pool_size=(3, 3), strides=2)

        self.conv_3 = Conv2D(filters=384, kernel_size=(3, 3), strides=1, padding='same', activation='relu')

        self.conv_4 = Conv2D(filters=384, kernel_size=(3, 3), strides=1, padding='same', activation='relu')

        self.con_5 = Conv2D(filters=256, kernel_size=(3, 3), strides=1, padding='same', activation='relu')
        self.pool_5 = MaxPool2D(pool_size=(3, 3), strides=2)

        self.flatten = Flatten()
        self.dense_6 = Dense(2048, activation='relu')
        self.dropout_6 = Dropout(0.5)
        self.dense_7 = Dense(2048, activation='relu')
        self.dropout_7 = Dropout(0.5)
        self.dense_8 = Dense(10, activation='softmax')

    def call(self, inputs, training=None, mask=None):
        x = self.conv_1(inputs)
        x = self.bn_1(x)
        x = self.act_1(x)
        x = self.pool_1(x)

        x = self.conv_2(x)
        x = self.bn_2(x)
        x = self.act_2(x)
        x = self.pool_2(x)

        x = self.conv_3(x)

        x = self.conv_4(x)

        x = self.con_5(x)
        x = self.pool_5(x)

        x = self.flatten(x)
        x = self.dense_6(x)
        x = self.dropout_6(x)
        x = self.dense_7(x)
        x = self.dropout_7(x)
        y = self.dense_8(x)
        return y
```

### 5.9 VGGNet-16

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_8.png)

```python
class VGGNet16(Model):
    def __init__(self):
        super(VGGNet16, self).__init__()
        self.c1 = Conv2D(filters=64, kernel_size=(3, 3), strides=1, padding='same')
        self.b1 = BatchNormalization()
        self.a1 = Activation('relu')

        self.c2 = Conv2D(filters=64, kernel_size=(3, 3), strides=1, padding='same')
        self.b2 = BatchNormalization()
        self.a2 = Activation('relu')
        self.p2 = MaxPool2D(pool_size=(2, 2), strides=2)
        self.d2 = Dropout(0.2)

        self.c3 = Conv2D(filters=128, kernel_size=(3, 3), strides=1, padding='same')
        self.b3 = BatchNormalization()
        self.a3 = Activation('relu')

        self.c4 = Conv2D(filters=128, kernel_size=(3, 3), strides=1, padding='same')
        self.b4 = BatchNormalization()
        self.a4 = Activation('relu')
        self.p4 = MaxPool2D(pool_size=(2, 2), strides=2)
        self.d4 = Dropout(0.2)

        self.c5 = Conv2D(filters=256, kernel_size=(3, 3), strides=1, padding='same')
        self.b5 = BatchNormalization()
        self.a5 = Activation('relu')

        self.c6 = Conv2D(filters=256, kernel_size=(3, 3), strides=1, padding='same')
        self.b6 = BatchNormalization()
        self.a6 = Activation('relu')

        self.c7 = Conv2D(filters=256, kernel_size=(3, 3), strides=1, padding='same')
        self.b7 = BatchNormalization()
        self.a7 = Activation('relu')
        self.p7 = MaxPool2D(pool_size=(2, 2), strides=2)
        self.d7 = Dropout(0.2)

        self.c8 = Conv2D(filters=512, kernel_size=(3, 3), strides=1, padding='same')
        self.b8 = BatchNormalization()
        self.a8 = Activation('relu')

        self.c9 = Conv2D(filters=512, kernel_size=(3, 3), strides=1, padding='same')
        self.b9 = BatchNormalization()
        self.a9 = Activation('relu')

        self.c10 = Conv2D(filters=512, kernel_size=(3, 3), strides=1, padding='same')
        self.b10 = BatchNormalization()
        self.a10 = Activation('relu')
        self.p10 = MaxPool2D(pool_size=(2, 2), strides=2)
        self.d10 = Dropout(0.2)

        self.c11 = Conv2D(filters=512, kernel_size=(3, 3), strides=1, padding='same')
        self.b11 = BatchNormalization()
        self.a11 = Activation('relu')

        self.c12 = Conv2D(filters=512, kernel_size=(3, 3), strides=1, padding='same')
        self.b12 = BatchNormalization()
        self.a12 = Activation('relu')

        self.c13 = Conv2D(filters=512, kernel_size=(3, 3), strides=1, padding='same')
        self.b13 = BatchNormalization()
        self.a13 = Activation('relu')
        self.p13 = MaxPool2D(pool_size=(2, 2), strides=2)
        self.d13 = Dropout(0.2)

        self.flatten = Flatten()
        self.dense_14 = Dense(512, activation='relu')
        self.dropout_14 = Dropout(0.2)
        self.dense_15 = Dense(512, activation='relu')
        self.dropout_15 = Dropout(0.2)
        self.dense_16 = Dense(10, activation='softmax')

    def call(self, inputs, training=None, mask=None):
        x = self.c1(inputs)
        x = self.b1(x)
        x = self.a1(x)

        x = self.c2(x)
        x = self.b2(x)
        x = self.a2(x)
        x = self.p2(x)
        x = self.d2(x)

        x = self.c3(x)
        x = self.b3(x)
        x = self.a3(x)

        x = self.c4(x)
        x = self.b4(x)
        x = self.a4(x)
        x = self.p4(x)
        x = self.d4(x)

        x = self.c5(x)
        x = self.b5(x)
        x = self.a5(x)

        x = self.c6(x)
        x = self.b6(x)
        x = self.a6(x)

        x = self.c7(x)
        x = self.b7(x)
        x = self.a7(x)
        x = self.p7(x)
        x = self.d7(x)

        x = self.c8(x)
        x = self.b8(x)
        x = self.a8(x)

        x = self.c9(x)
        x = self.b9(x)
        x = self.a9(x)

        x = self.c10(x)
        x = self.b10(x)
        x = self.a10(x)
        x = self.p10(x)
        x = self.d10(x)

        x = self.c11(x)
        x = self.b11(x)
        x = self.a11(x)

        x = self.c12(x)
        x = self.b12(x)
        x = self.a12(x)

        x = self.c13(x)
        x = self.b13(x)
        x = self.a13(x)
        x = self.p13(x)
        x = self.d13(x)

        x = self.flatten(x)
        x = self.dense_14(x)
        x = self.dropout_14(x)
        x = self.dense_15(x)
        x = self.dropout_15(x)
        y = self.dense_16(x)
        return y
```

### 5.10 InceptionNet

Inception 结构块（1x1卷积进行数据降维，减少channel的数量）

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_9.png)

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_10.png)

```python
class ConvBNRelu(Model):
    def __init__(self, filters, kernel_size, strides=1, padding='same'):
        super(ConvBNRelu, self).__init__()
        self.model = tf.keras.models.Sequential([
            Conv2D(filters=filters, kernel_size=kernel_size, strides=strides, padding=padding),
            BatchNormalization(),
            Activation('relu')
        ])

    def call(self, inputs, training=None, mask=None):
        y = self.model(inputs, training=training)
        return y

class InceptionBlock(Model):
    def __init__(self, filters, strides=1):
        super(InceptionBlock, self).__init__()
        self.c1 = ConvBNRelu(filters, kernel_size=(1, 1), strides=strides)
        self.c2_1 = ConvBNRelu(filters, kernel_size=(1, 1), strides=strides)
        self.c2_2 = ConvBNRelu(filters, kernel_size=(3, 3), strides=1)
        self.c3_1 = ConvBNRelu(filters, kernel_size=(1, 1), strides=strides)
        self.c3_2 = ConvBNRelu(filters, kernel_size=(5, 5), strides=1)
        self.p4_1 = MaxPool2D(pool_size=(3, 3), strides=1, padding='same')
        self.c4_2 = ConvBNRelu(filters, kernel_size=(1, 1), strides=strides)

    def call(self, inputs, training=None, mask=None):
        x1 = self.c1(inputs)
        x2_1 = self.c2_1(inputs)
        x2_2 = self.c2_2(x2_1)
        x3_1 = self.c3_1(inputs)
        x3_2 = self.c3_2(x3_1)
        x4_1 = self.p4_1(inputs)
        x4_2 = self.c4_2(x4_1)
        y = tf.concat([x1, x2_2, x3_2, x4_2], axis=3)
        return y


class InceptionNet10(Model):
    def __init__(self, num_blocks, num_classes, init_channels=16, **kwargs):
        """
        :param num_blocks: 网络的block数量的一半，每两个InceptionBlock组成一个block
        :param num_classes: 分类的类别数
        :param init_channels: 初始的channel数量
        :param kwargs: Model的初始化参数
        """
        super(InceptionNet10, self).__init__(**kwargs)
        self.in_channels = init_channels
        self.out_channels = init_channels
        self.num_blocks = num_blocks
        self.c1 = ConvBNRelu(filters=init_channels, kernel_size=(3, 3))
        self.blocks = tf.keras.models.Sequential()
        for block_id in range(num_blocks):
            for layer_id in range(2):
                if layer_id == 0:
                    block = InceptionBlock(self.out_channels, strides=2)
                else:
                    block = InceptionBlock(self.out_channels, strides=1)
                self.blocks.add(block)
            self.out_channels *= 2
        self.p1 = GlobalAveragePooling2D()
        self.dense1 = Dense(num_classes, activation='softmax')

    def call(self, inputs, training=None, mask=None):
        x = self.c1(inputs)
        x = self.blocks(x)
        x = self.p1(x)
        y = self.dense1(x)
        return y


model = InceptionNet10(num_blocks=2, num_classes=10)
```

### 5.11 ResNet

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_11.png)

进行加法的两个运算量的维度是否相同，导致ResNet块的不同

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_12.png)

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_14.png)

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_15.png)

```python
class ResnetBlock(Model):
    def __init__(self, filters, strides=1, residual_path=False):
        """
        :param filters:
        :param strides:
        :param residual_path: whether to make dimension adjustments
        """
        super(ResnetBlock, self).__init__()
        self.residual_path = residual_path
        self.c1 = Conv2D(filters=filters, kernel_size=(3, 3), strides=strides, padding='same', use_bias=False)
        self.b1 = BatchNormalization()
        self.a1 = Activation('relu')

        self.c2 = Conv2D(filters=filters, kernel_size=(3, 3), strides=1, padding='same', use_bias=False)
        self.b2 = BatchNormalization()
        if self.residual_path:
            self.down_c1 = Conv2D(filters=filters, kernel_size=(1, 1), strides=strides, padding='same', use_bias=False)
            self.down_b1 = BatchNormalization()
        self.a2 = Activation('relu')

    def call(self, inputs, training=None, mask=None):
        x = self.c1(inputs)
        x = self.b1(x)
        x = self.a1(x)
        x = self.c2(x)
        x = self.b2(x)
        if self.residual_path:
            residual = self.down_c1(inputs)
            residual = self.down_b1(residual)
            x = tf.add(x, residual)
        y = self.a2(x)
        return y

class ResNet18(Model):
    def __init__(self, block_list, initial_filters=64):
        super(ResNet18, self).__init__()
        self.num_blocks = len(block_list)
        self.out_filters = initial_filters
        self.c1 = Conv2D(self.out_filters, kernel_size=(3, 3), strides=1, padding='same', use_bias=False)
        self.b1 = BatchNormalization()
        self.a1 = Activation('relu')
        self.blocks = tf.keras.models.Sequential()
        # ResNet Architecture
        for block_id in range(len(block_list)):
            for layer_id in range(block_list[block_id]):
                if block_id != 0 and layer_id == 0:
                    # For the first layer of blocks other than the first block
                    block = ResnetBlock(self.out_filters, strides=2, residual_path=True)
                else:
                    block = ResnetBlock(self.out_filters, strides=1, residual_path=False)
                self.blocks.add(block)
            self.out_filters *= 2
        self.p1 = GlobalAveragePooling2D()
        self.dense = Dense(10)

    def call(self, inputs, training=None, mask=None):
        x = self.c1(inputs)
        x = self.b1(x)
        x = self.a1(x)
        x = self.blocks(x)
        x = self.p1(x)
        y = self.dense(x)
        return y


model = ResNet18([2, 2, 2, 2])
```

### 5.12 总结

![](D:\大学\笔记\深度学习\tensorflow_pic\cnn_13.png)

