## 1 神经网络

### 1.1 神经网络的介绍

对于一些线性不可分的数据，我们需要使用非线性的模型去拟合它

当我们使用一些线性模型比如线性回归或者逻辑回归，加上一些多项式特征，往往也可以得到较为准确的模型

只是如果样本的特征数量很大的时候，需要的多项式特征的数量会急速上升

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络1.png)

这个时候可以选择使用神经网络，神经网络可以用在线性数据和非线性数据

**神经元的结构**

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络2.png)

### 1.2 model representation

**神经网络的单个逻辑单元**

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络3.png)

**神经网络的结构**

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络4.png)

$x_0$：叫做bias unit

layer0：input layer 输入层

layer1：hidden layer 隐藏层

layer2：output layer 输出层

这是一个2-layer的神经网络

$\frac{1}{1+e^{-\theta^T·x}}$是sigmoid(logistic)  activation function

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络5.png)

$a_i^{(j)}$：表示第j层第i个逻辑单元的激活函数的值

$\Theta^{(j)}$：第j层映射到第j+1层的weights矩阵

**Forward Porpagation**

**vectorized implementation**

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络6.png)

神经网络与逻辑回归的最大区别就是神经网络最后计算output的特征$a_n$是从神经网络的训练中学习到的特征；而且神经网络的层数越高，学习到的特征越复杂

### 1.3 简单的神经网络example

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络8.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络9.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络10.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络11.png)

$\begin{align*}AND:\newline\Theta^{(1)} &=\begin{bmatrix}-30 & 20 & 20\end{bmatrix} \newline NOR:\newline\Theta^{(1)} &= \begin{bmatrix}10 & -20 & -20\end{bmatrix} \newline OR:\newline\Theta^{(1)} &= \begin{bmatrix}-10 & 20 & 20\end{bmatrix} \newline\end{align*}$

$A\quad XOR\quad B = AB + \overline{A}\overline{B}$

### 1.4 Multi-class Classification

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络12.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络13.png)

### 1.5 神经网络的损失函数

首先先定义一些变量

* L：神经网络的总层数
* $s_l$：在第$l$层中不包括bias unit的个数
* K：output layer的unit个数

$h_Θ(x)_k$ as being a hypothesis that results in the $k^{th}$ output

逻辑回归的损失函数：

$J(θ)=−\frac{1}{m}\sum_{i=1}^m[y^{(i)}log(h_θ(x^{(i)})) + (1−y^{(i)})log(1−h_θ(x^{(i)}))]+2mλ\sum_{j=1}^nθ_j^2$

神经网络的损失函数：

$J(\Theta)=-\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{K}[y_k^{(i)}log(h_{\Theta}(x^{(i)})_k) + (1-y_k^{(i)})log(1-h_{\Theta}(x^{(i)})_k)]\\ + \frac{\lambda}{2m}\sum_{l=1}^{L-1}\sum_{i=1}^{s_l}\sum_{j=1}^{s_l+1}(\Theta_{j,i}^{(l)})^2$

* 第一项将输出层的所有node在每个样本下的损失函数都加了起来
* 正则项将神经网络中的所有unit的$\Theta^2$都加了起来（不包括bias unit）
* the i in the triple sum does **not** refer to training example 

### 1.6 Back Propagation

Back propagation is neural-network terminology for minimizing our cost function.

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络14.png)

由于$J(\Theta)$可以由公式直接得到，所以我们主要关注损失函数的偏导，即梯度

这是Forward Propagation的逻辑

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络15.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络16.png)

Compute $\delta^{(L-1)}, \delta^{L-2},...,\delta^{(2)} \\ \delta^{(l)} = ((\Theta^{(l)})^T\delta^(l+1) .* a^(l) .* (1 - a^{(l)})$ 

$\delta^{(l)} = \frac{\part J(\Theta)}{\part z_j^{l}}$

$\frac{\partial J(\Theta)}{\part \Theta_ij^{(l)}} = \frac{\part J(\Theta)}{\part z_j^{(l+1)}}·\frac{\part z_j^{(l+1)}}{\part \Theta_ij^{(l)}} = a_j^{(l)}\delta_i^{l+1}$



![](D:\大学\笔记\深度学习\NG-DL_pic\神经网络17.png)

without vectorization:  $\Delta_{i,j}^{(l)} := \Delta_{i,j}^{(l)} + a_j^{(l)}\delta_i^{(l+1)}$

with vectorization:  $\Delta_{i,j}^{(l)} := \Delta_{i,j}^{(l)} + \delta^{(l+1)}(a^{(l)})^T$

### 1.7 Implementation Tricks

#### 1.7.1 Gradient Checking

为了确保神经网络的算法能够正常执行，很多时候我们需要确定gradient是否计算正确

$\frac{\part J(\Theta)}{\part \Theta_j} = \frac{J(\Theta_1,...,\Theta_j+\epsilon,...,\Theta_n) - J(\Theta_1,...,\Theta_j-\epsilon,...,\Theta_n)}{2\epsilon}$

一般，$\epsilon = 10^{-4}$

缺点：运算很慢，最好只用在调试代码的阶段

#### 1.7.2 Random Initialization

对于一些算法，比如梯度下降，我们需要传入初始theta值

如果我们传入的初始值（比如全为0），使得第一个hidden layer 的units均相等的话，此后每次更新都会得到相同的activation value，这表明神经网络每次只计算一个相同的feature

并且w具有相同的分量

To break symmetry  我们使用随机数

```python
w = np.random.randn((row, col)) * 0.01  # w越小，梯度越大
b = np.zeros((row, 1))  # b不会有symmetry问题
```

### 1.8 Training a neural network

1. Pick a network architecture (connectivity pattern between neurons)
   * No. of input units:	Dimension of features
   * No. output units:   Number of classes
   * Reasonable default: 1 hidden layer, or if > 1 hidden layer, have same no. of hidden units in every layer(usually the more the better)
2. Randomly initialize weights
3. Implement forward propagation to get $h_{\Theta}(x^{(i)})$ for any $x^{(i)}$
4. Implement code to compute cost function $J(\Theta)$
5. Implement code to compute partial derivatives $\frac{\part J(\Theta)}{\part \Theta_{j,k}^{(l)}}$

```latex
for i in range(1, m+1):
    Perform forward propagation and back propagation using example (x^(i), y^(i))
    (Get activations a^(l) and delta terms \delta^(l) for l = 2, ..., L)
```

6. Use gradient checking to compare $\frac{\part J(\Theta)}{\part \Theta_{j, k}^{(l)}}$ computed using back propagation **vs** using numerical estimate of gradient of $J(\Theta)$
   * Then disable gradient checking code
7. Use gradient descent or advanced optimization method with back propagation to try to minimize $J(\Theta)$ as a function of parameters $\Theta$

注意：神经网络是 non-convex function，有多个local minimum



## 2 Neural Network and Deep Learning

### 2.1 Supervised Learning  with Neural Network

| Input(x)          | Output(y)              | Application         | NN Type             |
| ----------------- | ---------------------- | ------------------- | ------------------- |
| Home features     | Price                  | Real Estate         | Standard NN         |
| Ad, user Info     | Click on ad? (0/1)     | Online Advertising  | Standard NN         |
| Image             | Object (1, ..., 100)   | Photo tagging       | CNN                 |
| Audio             | Text transcript        | Speech recognition  | RNN                 |
| English           | Chinese                | Machine translation | RNN                 |
| Image, Radar Info | Position of other cars | Autonomous driving  | Customized / Hybrid |



![NN Type](D:\大学\笔记\深度学习\NG-DL_pic\NNDL1.png)

Data Type

![Data Type](D:\大学\笔记\深度学习\NG-DL_pic\NNDL2.png)

### 2.2 Why is deep learning taking off?

因为大数据的到来，而神经网络相较于其他算法，在大数据上面的性能更好

![](D:\大学\笔记\深度学习\NG-DL_pic\NNDL3.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\NNDL4.png)

1. 数据
2. 算力
3. 算法

### 2.3 Activation Functions

![](D:\大学\笔记\深度学习\NG-DL_pic\NNDL6.png)

* sigmoid
  * $g(z) = \frac{1}{1 + e^{-z}}$
  * 不推荐使用这个，除非输出是一个二分类的问题（此时也只推荐在output layer上使用）
  * 两边的导数几乎为0，会导致梯度下降的很慢
  * 导数：$g'(z) = g(z) * (1 - g(z))$
* tanh
  * $g(z) = tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$
  * 有时会使用
  * 均值为0，范围为(-1, 1)
  * 两边的导数几乎为0，会导致梯度下降的很慢
  * 导数：$g'(z) = 1 - tanh^2(z)$
* ReLU
  * Rectified Linear Unit
  * y = max(0, z)
  * 推荐使用ReLU
  * 导数
    * 0,  if z < 0
    * 1,  if z > 0
    * undefined,  if z = 0
* Leaky ReLU
  * 由于ReLU在z<0的时候导数为0，所以leaky ReLU进行了改正
  * y = max(0.01z, z)
  * 0.01是一个超参数，可以进行修改
* Softmax
  * $y^{(i)} = \frac{e^{x(i)}}{\sum_{i=0}^{m}x^{(i)}}$
  * 概率和为1
  * 推荐使用在output layer

一般在神经网络中，hidden layer使用的是ReLU，output layer使用的是Softmax

### 2.4 Gradient

![](D:\大学\笔记\深度学习\NG-DL_pic\NNDL7.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\NNDL8.png)

### 2.5 Deep neural network

* 低层的神经网络学习到simple features，高层的神经网络通过这些simple features学习到complex features
* Circuit theory and deep learning: There are functions you can compute with a small L-layer deep neural network that shallow networks require exponentially more hidden units to compute

### 2.6 Forward Propagation and Backward Propagation

![](D:\大学\笔记\深度学习\NG-DL_pic\NNDL10.png)

* Forward Propagation for Layer l

Input $a^{[l-1]}$

Output $a^{[l]}$, cache ($z^{[l]}$)

$Z^[l] = W^{[l]}A^{[l-1]} + b^{[l]}$

$A^{[l]} = g^{[l]}(Z^{[l]})$

* Backward Propagation for Layer l

Input $da^{[l]}$

Output $da^{[l-1]}, dW^{[l]}, db^{[l]}$

$$dZ^{[l]}=dA^{[l]}*g^{[l]'}(Z^{[l]}) \\ dW^{[l]}=\frac{1}{m}dZ^{[l]}A^{[l-1]T}\\ db^{[l]}=\frac{1}{m}np.sum(dZ^{[l]}, axis=1, keepdims=True)\\dA^{[l-1]}=W^{[l]T}dZ^{[l]}$$

(* denotes elememt-wise multiplication)

![](D:\大学\笔记\深度学习\NG-DL_pic\NNDL9.png)

L : Layer number，input layer是第0层

$n^{[l]}$： 第$l$层的node个数

$a^{[l]} = g^{[l]}(z^{[l]})$： 第$l$层的activation function value，是第$l$层的激活函数作用在第$l$层的z上的值

**维度**

$W^{[l]} = (n^{[l]}, n^{[l-1]})$

$dW^{[l]} = (n^{[l]}, n^{[l-1]})$

$b^{[l]}=(n^{[l]}, 1)$

$db^{[l]}=(n^{[l]}, 1)$

$Z^{[l]} = (n^{[l]}, m)$

$A^{[l]} = (n^{[l]}, m)$



## 3 Improving Deep Neural Networks: Hyperparameters Tuning, Regularization and Optimization

### 3.1 Train / Dev / Test

Applying ML is a highly iterative process because of choices of hyperparameters.

把数据分为三部分：

* Training set训练集
* Development set（Dev set， 也叫hold-out cross validation set）
* Test set测试集

workflow是选择不同的算法拟合training set数据，然后测试这些算法模型在Dev set上的性能，得到一个最佳的模型，然后使用这个模型去预测test set，校验模型的准确率

dev set: 找到best performance算法

test set:验证模型的准确率

一般的划分是0.6 / 0.2 / 0.2

但是在大数据时代，如果数据是1，000，000个，dev set的大小可以是10，000个，test set的大小可以是10，000个

注意：train / dev / set需要come from the same distribution

### 3.2 Bias and Variance

![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter1.png)

* High bias
  * Train set error: 15%
  * Dev set error: 16%
* High variance
  * Train set error: 1%
  * Dev set error: 11%
* High bias and high variance
  * Train set error: 15%
  * Dev set error: 30%
* Low bias and Low variance
  * Train set error: 0.5%
  * Dev set error: 1%
* Base error: nearly 0%（以上bias和variance都是基于Base error，并且train set和dev set来自同一分布）

### 3.3 Basic Recipes

* 解决bias
  * 增加神经网络的层数
  * 训练more iterations
  * sometimes choose another neural network architecture
* 解决variance
  * 观察模型在dev set上的性能
  * 增加更多的数据
  * regularization
  * sometimes choose another neural network architecture

深度学习没有之前机器学习的“bias variance trade-off”问题

### 3.4 Regularization

> - The cost computation:
>     - A regularization term is added to the cost.
> - The backpropagation function:
>     - There are extra terms in the gradients with respect to weight matrices.
> - Weights end up smaller ("weight decay"): 
>     - Weights are pushed to smaller values

#### 3.4.1 Frobenius范数

Frobenius norm（Frobenius范数）：  $||W^{[l]}||_F^2=\sum_{i=1}^{n^{[l]}}\sum_{j=1}^{n^{[l-1]}}(w_{i,j}^{[l]})^2$

cost function: $J(W^{[1]},b^{[1]},...,W^{[L]},b^{[L]})=-\frac{1}{m}\sum_{i=1}^{m}L(\hat{y}^{(i)},y^{(i)})+\frac{\lambda}{2m}\sum_{l=1}^{L}||W^{[l]}||_F^2$

#### 3.4.2 Dropout regularization

> - Dropout is a regularization technique.
> - You only use dropout during training. Don't use dropout (randomly eliminate nodes) during test time.
> - Apply dropout both during forward and backward propagation.
> - During training time, divide each dropout layer by keep_prob to keep the same expected value for the activations. For example, if keep_prob is 0.5, then we will on average shut down half the nodes, so the output will be scaled by 0.5 since only the remaining half are contributing to the solution. Dividing by 0.5 is equivalent to multiplying by 2. Hence, the output now has the same expected value. You can check that this works even when keep_prob is other values than 0.5. 

![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter2.png)

我们定义一个base probability of eliminating nodes，然后对每一个node随机一个概率值，如果> base probability，就将该node从neural network中去除（不参与forward propagation和backward propagation）

**Inverted Dropout的实现**

```python
# p3是layer 3的eliminating node概率
p3 = np.random.randn(A3.shape[0], A3.shape[1])
keep_prob = 0.8
p3 = p3 < keep_prob # eliminate_prob = 0.2 保留原来的80%
a3 *= p3  # 去除node
a3 /= keep_prob  # 这一步是“inverted dropout”，让a3与除去node之前的值大致相同，这样之后训练出来的模型可以直接预测
```

**Dropout的原理**

因为每一个node在每一次iteration都会有概率被eliminate，所以cannot rely on any one feature，因此每一个weight都会尽可能地低

只在training的过程中使用Dropout

如果没有实现中的最后一步，需要在test过程中实现dropout，这会使算法更复杂

每一个layer都有一个`keep_prob`

如果`keep_prob = 1`，那么相当于不对这层使用dropout

注意：去除的neuron在backward propagation中也要去除，并且A在forward propagation中进行了scaling(除以keep_prob)，则dA也要在backward propagation中进行相同的scaling

### 3.4.3 Other Regularization Methods

1. Data Augmentation

   * 在Computer vision中，可以水平翻转图像得到新的数据
   * 随机裁剪图片，放大缩小图片

2. Early Stopping

   ![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter3.png)

   * 缺点是可以拟合的不是很好，但不会过拟合

3. L2 Regularization

   * try different values of lambda

Orthogonalization 正交化：每次对ML的调整只改变一个影响因子

### 3.5 Normalizing Inputs

当input features之间的scale相差很大时，training的过程会很慢

可以通过normalization的方式适当的加速training

​		$X = \frac{X - \mu}{\sigma}$

注意：test data此时也需要进行相同的尺度变换，即使用相同的$\mu,\sigma$进行normalize

### 3.6 Weight Initialization for Deep Networks

> - Different initializations lead to very different results
> - Random initialization is used to break symmetry and make sure different hidden units can learn different things
> - Resist initializing to values that are too large!
> - He initialization works well for networks with ReLU activations

在Deep Neural Networks，当layer number很大的时候，gradient很容易指数级的增长或减小

由于每一个node需要前一个层的node的权重和，所以我们希望输出与输入一样小

所以我们想要将Weight：  $Variance(W) = \frac{1}{n}$

$W^{[l]}=np.random.randn(shape) * np.sqrt(\frac{1}{n^{[l-1]}})$

对于ReLu函数，$Variance(W) = \frac{2}{n}$  (He Initialization)

对于tanh函数，$Variance(W) = \frac{1}{n}$  (Xavier Initialization)

Other version: $Variance(W) = \frac{2}{n^{[l-1]}+n^{[l]}}$

A well-chosen initialization can:

1. Speed up the convergence of gradient descent
2. Increase the odds of gradient descent converging to a lower training (and generalization) error

### 3.7 Gradient Checking

$gradient = \frac{f(\theta+\epsilon)-f(\theta-\epsilon)}{2*\epsilon}$

Take $W^{[1]},b^{[1]},...,W^{[L]},b^{[L]}$ and reshape into a big vector $\theta$

Take $dW^{[1]},db^{[1]},...,dW^{[L]},db^{[L]}$ and reshape into a big vector $d\theta$

for each i:

​	$d\theta_{appr}[i] = \frac{J(\theta_1,\theta_2,...,\theta_i+\epsilon,...)-J(\theta_1,\theta_2,...,\theta_i-\epsilon,...)}{2*\epsilon}$

​	($\epsilon = 10^{-7}$)

​	(比较$d\theta_{appr}[i]$和$d\theta[i]$)

​	Check  $\frac{||d\theta_{appr}-d\theta||_2}{||d\theta_{appr}||_2+||d\theta||_2} \approx 10^{-7}$

> implementation notes:
>
> * Use it only in debug
>
> * Remember regularization
> * Doesn't work with dropout

### 3.8 Mini-batch Gradient Descent

> - Shuffling and Partitioning are the two steps required to build mini-batches
> - Powers of two are often chosen to be the mini-batch size, e.g., 16, 32, 64, 128.

因为batch gradient descent在数据量很大的时候运行时，运算速度很慢，我们需要使用其他optimizing method来训练大数据的模型

mini-batch gradient descent

​	将数据集分成了k份，每一份子数据集有m/k个数据

​	i遍历从1到k，每次选取第i份子数据集进行gradient descent

（每一次从头到尾的遍历都是"one epoch,即one pass"）

当k=1，此时的mini-batch gradient descent变成batch gradient descent

当k=m，stochastic gradient descent随机梯度下降（缺点：lose the speed of vectorization)

**一般k的取值是2的倍数**

小批量梯度下降不能保证每一次iteration的cost都是减小的，但是总体的趋势是减小的

### 3.9 Exponentially Weighted Averages

指数加权平均数

$V_t = \beta*V_{t-1} + (1-\beta)*\theta_t$

$V_t$通过这个公式可以计算$\frac{1}{1-\beta}$个t之前的数据的平均数，$\beta$是之前数据的权重

![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter4.png)

data * exponentially decaying function, and then sum it up

​	$0.9^{10} \approx \frac{1}{e}$

​	$0.98^{50} \approx \frac{1}{e}$

​	$\epsilon = 1 - \beta, \quad (1-\epsilon)^{\frac{1}{\epsilon}} = \frac{1}{e}$

优点：low memory(one single real number)，运算效率高

**Bias correction**

由于一开始$V_0 = 0$，导致开始的数据权重很低，因此需要修正

​	$V_t = \frac{V_t}{1 - \beta^t}$

修正只在需要使用该值进行其他计算时使用，在$V_t$迭代的过程中不使用

### 3.10 Gradient Descent with Momentum

![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter5.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter6.png)



假设目前的cost function到minimum的方向是x方向，则在y方向上，由于mini-batch造成的oscillation随着exponentially weighted averages而抵消（既有+y，也有-y）

$\beta$ is momentum

- The larger the momentum $\beta$ is, the smoother the update, because it takes the past gradients into account more. But if $\beta$ is too big, it could also smooth out the updates too much. 
- Common values for $\beta$ range from 0.8 to 0.999. If you don't feel inclined to tune this, $\beta = 0.9$ is often a reasonable default. 
- Tuning the optimal $\beta$ for your model might require trying several values to see what works best in terms of reducing the value of the cost function $J$. 

### 3.11 RMSprop

Root mean square propagation

**Implementation details:**

On iteration t:

​	Compute dW, db on current mini-batch

​	$SdW = \beta_2*SdW + (1-\beta_2)(dW)^2$

​	$Sdb = \beta_2 * Sdb + (1 - \beta_2)(db)^2$

​	$W := W - \alpha\frac{dW}{\sqrt{SdW}}$

​	$b := b - \alpha\frac{db}{\sqrt{Sdb}}$

由于dW很小，所以$(dW)^2$更小，SdW变大，加快W的converge

由于db很大，所以$(db)^2$更大，Sdb变小，减慢b的converge

### 3.12 Adam Optimization Algorithm

结合了Momentum和RMSprop

**Implementation details:**

$$V_{dW} = 0, S_{dW} = 0, V_{db} = 0, S_{db} = 0$$

**Hyperparameter choices**

$\alpha$ : needs to be tune

$\beta_1$ : 0.9

$\beta_2$ : 0.999

$\epsilon : 10^-9$

The update rule is, for $l = 1, ..., L$: 

$$\begin{cases}
v_{dW^{[l]}} = \beta_1 v_{dW^{[l]}} + (1 - \beta_1) \frac{\partial \mathcal{J} }{ \partial W^{[l]} } \\
v^{corrected}_{dW^{[l]}} = \frac{v_{dW^{[l]}}}{1 - (\beta_1)^t} \\
s_{dW^{[l]}} = \beta_2 s_{dW^{[l]}} + (1 - \beta_2) (\frac{\partial \mathcal{J} }{\partial W^{[l]} })^2 \\
s^{corrected}_{dW^{[l]}} = \frac{s_{dW^{[l]}}}{1 - (\beta_2)^t} \\
W^{[l]} = W^{[l]} - \alpha \frac{v^{corrected}_{dW^{[l]}}}{\sqrt{s^{corrected}_{dW^{[l]}}} + \varepsilon}
\end{cases}$$
where:

- t counts the number of steps taken of Adam 
- L is the number of layers
- $\beta_1$ and $\beta_2$ are hyperparameters that control the two exponentially weighted averages. 
- $\alpha$ is the learning rate
- $\varepsilon$ is a very small number to avoid dividing by zero

Some advantages of Adam include:

- Relatively low memory requirements (though higher than gradient descent and gradient descent with momentum) 
- Usually works well even with little tuning of hyperparameters (except $\alpha$)

### 3.13 Learning Rate Decay

> Learning rate decay can be achieved by using either adaptive methods or pre-defined learning rate schedules.

由于mini-batch gradient descent不会很好地converge，因此我们定义：随着iteration的增加，减小learning rate的值，使其step become smaller，更好地converge minimum

$\alpha = \frac{1}{1 + decayRate * epochNum}\alpha_0$

| Epoch | alpha (alpha0=0.2) |
| ----- | ------------------ |
| 1     | 0.1                |
| 2     | 0.067              |
| 3     | 0.05               |
| 4     | 0.04               |

但是这种情况会让$\alpha$快速减小到0，所以需要使用 “Fixed Interval Scheduling”

$\alpha = \frac{1}{1 + decayRate * \frac{epochNum}{timeInterval}}\alpha_0$

```python
if epoch_num % time_interval == 0:
        learning_rate = learning_rate0 / (1 + decay_rate * epoch_num / time_interval)
```



Other learning rate decay methods**

* exponentially decay

  $\alpha = 0.95^{epochNum}\alpha_0$

* $\alpha=\frac{k}{epochNum}\alpha_0\quad or\quad \alpha=\frac{k}{t}\alpha_0$

  * k是常数
  * t是当前的循环次数

### 3.14 Tuning Process

* 第一优先级
  * learning rate $\alpha$
* 第二优先级
  * momentum $\beta$  一般是0.9
  * mini-batch size
  * number of hidden units
* 第三优先级
  * Adam optimization $\beta_1, \beta_2, \epsilon$ ，一般的取值分别是0.9， 0.999， $10^{-8}$
  * number of layers
  * learning rate decay

**调参原则**

* random rule: 随机选择超参数进行模型评估

  * 对于数据量大的时候，最好不要使用网格搜索

  * 选择appropriate scale

    * 对数scale

      * ```
        先确定range[a, b]
        确定选用对数尺度，计算[m, n] a=10^m,b=10^n
        随机一个数r属于[m, n]，得到random value val=10^-r
        ```

    * Hyperparameters for exponentially weighted averages

      * ```
        beta在[0.9, 0.999]范围中
        1-beta在[0.001, 0.1]范围中
        对1-beta取对数尺度
        r 是[-3, -1]的随机数
        random value val=1 - 10^r
        ```

      * 

* coarse to fine

  * 先大范围随机选择超参数，对于一块区域内比较好的超参数范围，再进行更密集地选择

* 每隔一段时间重新测试当前选择的超参数是否满意

* 训练的tips

  * Panda Approach
    * babysit one model
    * 由于算力等资源的缺乏，每次只完成当前超参数条件下的前部分训练，观测结果，再对超参数进行调整，重复上述过程

  * Caviar Approach  （鱼子）
    * Training many models in parallel

### 3.15 Batch Normalization

在神经网络中，由于normalize input features可以加快神经网络的训练

Batch Normalization使得不仅对input layer使用normalization，对一些hidden layers也会使用

推荐对非线性函数前的Z使用，而不是非线性函数后的A

$\mu = \frac{1}{m}\sum z^{(i)}$

$\sigma^2 = \frac{1}{m}\sum (z^{(i)}-\mu)^2$

$z_{norm}^{(i)} = \frac{z^{(i)}-\mu}{\sqrt{\sigma^2+\epsilon}}$

$\tilde{z^{(i)}} = \gamma z_{norm}^{(i)} + \beta$

这里的$\gamma,\beta$都是训练过程中学习到的参数，它们是为了让数据避免全部变成正态分布，而是不同的分布（$\gamma$改变方差，$\beta$改变均值）

![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter7.png)

由于Z被normalize，所以无论$b^{[l]}$的值是多少，最后数据都会被均值化，所以$b^{[l]}$的值无意义

而此时的bias是$\beta^{[l]}$

当训练数据集和测试数据集的分布不同时，模型的准确率很低，此时被称为"covariate shift"

而batch normalization可以有效地减小神经网络的参数受到earlier layers参数变化的影响，使得参数更stable（因为均值始终为0，方差为1）

当输入的分布改变的时候，参数受到的影响也会减小

**Batch Norm as regularization**

* Each mini-batch is scaled by the mean/variance computed on just that mini-batch.
* This adds some noise to the values $z^{[l]}$ within that minibatch. So similar to dropout, it adds some noise to each hidden layer's activations.
* This has a slightly regularization effect.
* mini-batch的size越大，noise越小，regularization的效果越小
* 不推荐使用batch norm进行regularization 

### 3.16 Batch Normalization at test time

![](D:\大学\笔记\深度学习\NG-DL_pic\hyperparameter8.png)

由于进行测试的时候或者是预测的时候，每次的数据不可能正好是一个mini-batch size，所以我们使用exponentially weighted averages去计算均值和方差$\mu,\sigma$，通过计算每一个mini-batch的$\mu,\sigma$然后使用exponentially weighted average，将其作为测试数据或者预测数据的均值和方差，再使用训练得到的$\gamma，beta$得到$Z_{norm}$

### 3.17 Softmax Regression

Softmax Regression是将output layer的sigmoid函数换成softmax函数

softmax函数：$softmax(z_i^{[l]}) = \frac{e^{z_i^{[l]}}}{\sum_{j=1}^{n[l]}e^{z_j^{[l]}}}$

可以解决多分类问题，对于当前样本，每次处理的数据分类结果概率和为1

**Loss function**

$L(\hat{y}, y) = -\sum_{j=1}y_j·log(\hat{y_j})$

**Cost function**

$J(W^{[1]}, b^{[1]}, ...) = \frac{1}{m}\sum_{i=1}^{m}L(\hat{y}, y)$



$dZ^{[L]} = \hat{Y} - Y$

### 3.18 Framework-Tensorflow

tensorflow的初步使用

好处：只需要实现forward propagation，tensorflow自动实现backward propagation

```python
import tensorflow as tf

w = tf.Variable(0, dtype=tf.float32)  # 变量
optimizer = tf.keras.optimizers.Adam(learning_rate=0.1)

# train_step 训练一次模型
def train_step():
    # In tensorflow, you only need to implement forward propagation
    # and do not need to implement backward propagation

    # 第一种方式
    # Using tf.GradientTape() to record the sequence of operations needed to compute the cost function
    # it will reverse the order to calculate the backward propagation
    with tf.GradientTape() as tape:
        cost = w ** 2 - 10 * w + 25
    trainable_variables = [w]
    grads = tape.gradient(cost, trainable_variables)  # 计算梯度gradient
    optimizer.apply_gradients(zip(grads, trainable_variables))  # 训练模型

    # 第二种方式
    # def cost():
    #     return w ** 2 - 10 * w + 25
    #
    # optimizer.minimize(cost, [w])


for i in range(1000):
    train_step()
print(w)
```

```python
import numpy as np
import tensorflow as tf

w = tf.Variable(0, dtype=tf.float32)
x = np.array([1., -10., 25.], dtype=np.float32)
optimizer_Adam = tf.keras.optimizers.Adam(learning_rate=0.1)

# 定义模型的训练过程
def training(x, w, optimizer, iters=1000):
    # 定义cost function
    def cost_fn():
        return x[0] * w ** 2 + x[1] * w +x[2]
    for i in range(iters):
        optimizer.minimize(cost_fn, [w])
    return w


w = training(x, w, optimizer_Adam)
print(w)
```

* tf.Variable
* tf.constant
* tf.cast
* tf.matmul
* tf.linalg.matmul
* tf.add
* tf.math.add
* tf.keras.activations.sigmoid
* tf.keras.activations.relu
* tf.one_hot(labels, depth, axis=0)

* tf.keras.initializer.GlorotNormal(seed=1)

  * 返回initializer:均值为0，标准差为`stddev = sqrt(2 / (fan_in + fan_out))`, where `fan_in` is the number of input units and `fan_out` is the number of output units

  * ```python
    initializer = tf.keras.initializers.GlorotNormal(seed=1)   
    W1 = tf.Variable(initializer(shape=(25, 12288)))
    b1 = tf.Variable(initializer(shape=(25, 1)))
    W2 = tf.Variable(initializer(shape=(12, 25)))
    b2 = tf.Variable(initializer(shape=(12, 1)))
    W3 = tf.Variable(initializer(shape=(6, 12)))
    b3 = tf.Variable(initializer(shape=(6, 1)))
    ```

* tf.zeros()
* tf.ones()
* tf.reduce_mean(tf.keras.losses.binary_crossentropy(y_true = ..., y_pred = ..., from_logits=True))
  
  * t's important to note that the "`y_pred`" and "`y_true`" inputs of tf.keras.losses.binary_crossentropy are expected to be of shape (number of examples, num_classes).
* tf.Data.dataset = dataset.prefetch(8)
  * What this does is prevent a memory bottleneck that can occur when reading from disk. `prefetch()` sets aside some data and keeps it ready for when it's needed. 
  * It does this by creating a source dataset from your input data, applying a transformation to preprocess the data, then iterating over the dataset the specified number of elements at a time. This works because the iteration is streaming, so the data doesn't need to fit into the memory.



## 4 Structuring Machine Learning Projects

### 4.1 ML Strategies

* Collect more data
* Collect more diverse training set
* Train algorithm longer with gradient descent
* Try Adam instead of gradient descent
* Try bigger network
* Try smaller network
* Try dropout
* Add L2 regularization
* Network architecture
  * Activation functions
  * #hidden units
  * ...

### 4.2 Orthogonalization

Orthogonalization 是每次调整一个参数修改一个维度上的配置（比如不会同时修改training set error和dev set error）

**Chain of assumptions in ML**

1. Fit training set well on cost function
   * bigger network
   * Adam
   * ...
2. Fit dev set well on cost function
   * Regularization
   * Bigger training set
3. Fit test set well on cost function
   * Bigger dev set
4. Performs well in real world
   * Change dev set
   * Change cost function

### 4.3 Single Number Evaluation Metric

精准率Precision

召回率Recall

结合精准率和召回率：F1 Score = $\frac{2}{\frac{1}{P}+\frac{1}{R}}$

dev set + single number evaluation metric: speed up this iterative process of improving machine learning algorithms

single number evaluation metric 也可以是error averages

evaluation metric用来判断classifier之间的优劣

### 4.4 Satisficing and Optimizing Metric

| Classifier | Accuracy | Running time |
| ---------- | -------- | ------------ |
| A          | 90%      | 80ms         |
| B          | 92%      | 95ms         |
| C          | 95%      | 1,500ms      |

Now you want to consider accuracy as well as running time.

Instead of using a linear weighted sum of two variables, you want to choose a classifier that **maximizes the accuracy but subject to the running time which should be less than 100ms**

In this case, accuracy: optimizing metric

running time: Satisficing metric

特别地，当有一个N matrix, 比较合理的是选择one matrix作为optimizing matrix，其他N-1 matrix 作为satisficing matrix

### 4.5 Dev / Test set

如果有多个来自不同源的数据要分为dev set和test set，最好的办法是shuffle them randomly into two sets，这样可以保证两个数据集来自同一分布

Guideline: Choose a dev set and test set to **reflect data you expect to get in the future** and consider important to do well on

传统的数据集大小：

​	train 70%	test 30%

​	train 60%	dev 20%	test 20%

对于数据量很大的时候

​	train 98%	dev 1%	test 1%

Size of test set: Set your test set to be big enough to give high confidence in the overall performance of your system

Size of dev set: Set your test set to be big enough to evaluate best algorithms

### 4.6 When to change Dev/Test set and Metrics?

when your evaluation metric is no longer correctly rank ordering preferences between algorithms, then that's a sign that you should change your evaluation metric or perhaps your development set or test set.

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy3.png)

Orthogonalization:

1. How to define  a metric to evaluate classifiers
2. Worry separately about how to do well on this metric.

If doing well on your metric + dev/test set does not correspond to doing well on your application, change your metric and/or dev/test set.



### 4.7 Why human-level performance?

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy1.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy2.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy4.png)

Human-level error is often regarded as a proxy for Bayes error.

The gap between bayes error and training error is often called '**Avoidable bias**'.

The gap between training error and dev error is often called '**Variance**'.

The gap between 0% and training error is often called '**bias**'.

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy5.png)

如果training error离上面的四个error都很远，那么选择谁都是可以的，此时我们需要使用方法减小avoidable error；如果离上面四个都很近，最好选择最小的Error，因为它最接近bayes error

### 4.8 Surpassing Human-level Performance

如果deep learning systems 超过了human-level performance，it is hard to define bayes error and find other techniques to improve bias problem.

**Problems where ML significantly surpasses human-level performance**

* Online advertising

* Product recommendations

* Logistics (predicting transit time)

* Loan approvals

These are all structured data

* Speech recognition
* Some image recognition
* Medical
  * ECG，Skin cancer

### 4.9 Improving your Model Performance

The two fundamental assumptions of supervised learning

1. You can fit the training set pretty well.
2. The training set performance generalizes pretty well to the dev/test set.

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy6.png)

### 4.10 Error Analysis

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy7.png)

如果在dev set中100个错误分类的样本中，有5个是狗，那么如果解决了dog problem，error也只会减小5%；如果有50个是狗，error可以减少50%

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy8.png)

左边一列是手动查看dev的样本的序号

最终得出mispredicted example中，each category的error比重，这让我们知道优化其中哪一个category可以得到最大的收益（这里还需要考虑到该category修复的cost）

### 4.11 Cleaning Up Incorrectly Labeled Data

有时候train set, dev set, test set中原始数据的label是错误的

DL algorithms are quite robust to random errors in the training set, but they are less robust to systematic errors. (系统错误，比如持续地标注白狗是猫)

此时推荐在Error analysis中添加一列 Incorrectly labeled

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy9.png)

如果Errors due to incorrect labels的比例或者影响很小，可以忽略或不理睬incorrect labels带来的问题

如果Errors due to incorrect labels的比例很大，因为dev set是用来区分不同的classifier的优劣，此时incorrect labels影响到了dev set的error，进而影响到了classifier的选择，则需要花费时间重新标注incorrect labels

**Correcting incorrect dev/test set examples**

* Apply the same process to your dev/test set to make sure they continue to come from the same distribution
* Consider examining examples your algorithm got right as well as ones it got wrong
* Train and dev/test data may now come from slightly different distributions

### 4.12 Build First System Quickly, then Iterate

* Quickly set up dev/test set and evaluation metric
* Build initial system quickly
* Use bias/variance analysis & Error analysis to prioritize next steps

### 4.13 Training and Testing on Different Distributions

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy11.png)

如果你想要建造一个cat classifier，此时你有200000 cat pictures from webpages，10000 cat pictures from mobile app，你最终想要this cat classifier能够很好的分别mobile app上的图片

一种选择是将210000 cat pictures组合起来，randomly shuffle，然后205000是training set，之后dev set和test set各2500（这种选择**不建议**，因为dev set和test set中大多数是来自webpage的图片，不能很好地泛化到mobile app上）

另一种选择是training set中200000 cat pictures from webpages + 5000 cat pictures from mobile apps, dev set 和 test set 分别2500 cat pictures from mobile app (better set the target)

虽然第二种选择会导致training set和dev/test set的distribution不同，但是模型在mobile app上的performance得到了增强

### 4.14 Bias and Variance with Mismatched Data Distributions

当training error和dev error的difference很大的时候，有可能是high variance problem，也有可能是因为training set和dev set来自不同分布的问题

为了判断出具体是哪一个问题，randomly shuffle the training set,并从中取出一小部分作为training-dev set，这一部分数据集不参与训练过程（此时，training-dev set和training set来自于同一分布）

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy12.png)

如果training error和training-dev error的difference很大，这种情况是high variance

如果training-dev error和dev error的difference很大，这种情况是data mismatched problem

如果human-level performance和training error很大，这种情况是avoidable bias很大

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy13.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy14.png)

### 4.15 Addressing Data Mismatch

* Carry out manual error analysis to try to understand difference between training and dev/test set
  * 有些时候，我们不想让model overfit test set，因此不把test set考虑进去
* Make training data more similar; or collect more data similar to dev/test sets

一个常用的make training data more similar的方法是：**Artificial Data Synthesis**

有一点需要注意的是：如果人工合成的数据只是all possible examples中的一小部分，learning algorithm可能会overfit 这一小部分数据

### 4.16 Transfer Learning迁移学习

迁移学习：take knowledge the neural network has learned from one task and apply that knowledge to a separate related task

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy10.png)

如果现在有一个image recognition的DL system，需要完成一个face recognition system，有一个方法是将image recognition的DL system结构与参数保留，将output layer替换为一个新的output layer（包括当前output layer上的参数）。

使用face recognition的数据集对new NN进行训练。如果新的数据集很大，则可以重新训练整个网络的参数，此时initial phase of training on image recognition称为**pre-training**，而training on face recognition称为**fine-tuning**。如果数据集很小，可以只训练最后一层或最后几层，保留（固定）其余layer的参数（因为前面的layer已经学会了怎么识别edge，curve..等比较简单的feature）

迁移学习主要使用在**when you have a lot of data for the problem you're transferring from and usually relatively less data for the problem you're transferring to**.

**When transfer learning makes sense** (Task A -> Task B)

1. Task A and Task B have the same input X
   * X can be both images, or speeches
2. You have a lot more data for Task A than Task B
   * Since data in Task B is more valuable than data in Task A, it is easy to achieve more data in Task A
3. Low-level features from Task A could be helpful for learning Task B.

使用迁移参数初始化网络能够提升泛化能力（比随机初始化参数的性能要好）

### 4.17 Multi-task Learning

多任务学习：对一个神经网络同时训练多个task（相当于这些task共享参数）

比如，自动驾驶中的图像识别，一张图片上需要识别行人、交通灯、车辆、stop sign

因为，对于一张图片可能会有多个label

而且允许有些图片的label标注为不确定，只需要在计算cost function中不考虑这些不确定的label即可

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy15.png)

经常用在Computer Vision和Object Detection

**When multi-task learning makes sense**

* Training on a set of tasks that could benefit from having shared lower-level features
* Usually: Amount of data you have for each task is quite similar
* Can train a big enough neural network to do well on all the tasks

### 4.18 End to end deep learning

![](D:\大学\笔记\深度学习\NG-DL_pic\ML_Strategy16.jpg)

**传统机器学习**的流程往往由多个独立的模块组成，比如在一个典型的自然语言处理（Natural Language Processing）问题中，包括分词、词性标注、句法分析、语义分析等多个独立步骤，每个步骤是一个独立的任务，其结果的好坏会影响到下一步骤，从而影响整个训练的结果，这是非端到端的。

而end-to-end deep learning将多个独立的模块融合成一个整体，一起训练，从输入直接通过网络得到对应的输出

**End-to-end deep learning的好处是**

1. 误差理论告诉我们误差传播的途径本身会导致误差的累积，多个阶段一定会导致误差累积，端到端学习能够减少误差传播的途径，联合优化
2. 端到端的学习省去了在每一个独立学习任务执行之前所做的数据标注，而数据标注往往是比较昂贵的，易出错的

注意：端对端学习只有在给定了**enough labeled data**，其performance才会很好

**但有些例子不适合使用end-to-end deep learning**

比如，在face recognition中，一般将识别任务分为two steps

1. 检测到图片中人脸的位置
2. 识别人脸的身份（一般的做法是将检测到的人脸与数据库中标注身份的人脸进行比较，最后通过NN得到其最有可能的身份）

face recognition一般不适合端到端学习，原因是：

1. two sub-tasks are quite simple
2. 有足够的数据for two sub-tasks，但是没有足够的数据支持端到端学习

**Pros and Cons of end-to-end deep learning**

Pros:

* Let the data speak  (让NN自己学feature，而不是限制NN学指定的feature)
* Less hand-designing of components needed

Cons:

* May need large amounts of data (a direct mapping from one end of the system all the way to the other end of the system)
* Excludes potentially useful hand-designed components
  * 有些时候，数据量很小，在一些模块中，需要人手动添加或制造一些数据或components或其他

Apply end-to-end deep learning, the key question is : 

​		**Do you have sufficient data to learn a function of the complexity needed to map x to y?**



## 5 Convolutional Neural Network

### 5.1 Edge Detection

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN1.png)

将原始数据矩阵（通常是图片）通过与filter(也叫卷积核)进行卷积运算，检查是否原始数据矩阵具有某种特征（计算方式：原始数据矩阵的子矩阵与filter的对应元素做element-wise运算求和，放入结果矩阵的对应位置）

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN2.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN3.png)

这里30表示该edge bright -> dark;	-30表示dark -> bright

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN4.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN5.png)

除了上述定义的不同的filter (Sobel filter, Scharr filter)，一个更好的方法是定义9个参数，让NN自己学习到这9个参数（可能学习到不同角度，不同数组的edge）

### 5.2 Padding

对于一个`n*n`的输入矩阵，filter是`f*f`的矩阵，则经过convolution运算之后的矩阵size是`(n-f+1)*(n-f+1)`

但是对于原来输入矩阵四周的元素，其参与卷积运算的次数没有输入矩阵中心元素的次数多，因此此时权重是不均匀的，需要修正

Padding：在输入矩阵的上下左右添加**p**行0元素，此时输入矩阵的size是`(n+2p-f+1)*(n+2p-f+1)`

注意：padding也可以是不对称的添加，但是这种情况比较少见

**Valid Convolution**: p = 0

**Same Convolution**: Pad so that output size is the same as the input size

​	$p = \frac{f-1}{2}$

注意：filter一般维数都是奇数，3x3, 5x5, 7x7

- It allows you to use a CONV layer without necessarily shrinking the height and width of the volumes. This is important for building deeper networks, since otherwise the height/width would shrink as you go to deeper layers. An important special case is the "same" convolution, in which the height/width is exactly preserved after one layer.
- It helps us keep more of the information at the border of an image. Without padding, very few values at the next layer would be affected by pixels at the edges of an image.

### 5.3 Strided Convolution

定义一个stride s，每次计算一个位置的卷积运算之后，跳到后s行或后s列，再进行卷积运算

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN6.png)

如果一个输入矩阵的维数是nxn，padding是p，stride是s，filter的维数是fxf，则完成卷积运算之后的矩阵的维数是$([\frac{n+2p-f}{s}]+1)*([\frac{n+2p-f}{s}]+1)$

[a]是对a进行向下取整

**Technical note on cross-correlation vs. convolution**

其实之前的对filter与输入矩阵的子矩阵进行element-wise相乘再相加的操作，是**cross-correlation 互相关**

而真正的convolution在互相关的运算之前，还需要添加一步：将filter进行水平翻转和竖直翻转

之后再进行卷积运算

但是对于deep learning，是否进行翻转操作，对model没有什么影响，所以一般互相关和convolution都称为convolution

### 5.4 Convolutions Over Volume

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN7.png)

对于一个输入矩阵，size是$n_1$x$n_2$x$n_3$,filter是$f_1$x$f_2$x $n_3$，padding为p,stride为s

则卷积运算后的矩阵是$[\frac{n-2p+1}{s}]+1$x$[\frac{n-2p+1}{s}]+1$x$n_c$

$n_1$: height

$n_2$: width

$n_3$: channels (文献中一般称为depth)  输入矩阵的channels必须和filter的channels一致

$n_c$: number of features (also can be denoted by number of filters)

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN8.png)

也是对应位置做element-wise乘法再相加

### 5.5 One Layer of CNN

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN9.png)

一个CNN的layer

通过不同的filter对输入进行卷积运算，加上一个偏置项bias，之后经过一个非线性函数ReLU

类似于神经网络

Question: If you have 10 filters that are 3 x 3 x 3 in one layer of a neural network, how many parameters does that layer have?

One classifier has 3x3x3 = 27 parameters  再加上bias 一共28个parameters

总共有10filters，所以一共有280个parameters

parameters: ($f^{[l]}$x$f^{[l]}$x$n_c^{[l-1]}$+1) x $n_c^{[l]}$

（一个需要强调的是：不管输入的size是多少，参数的个数不会改变）

**Summary of notation**

If layer l is a convolution layer:

$f^{[l]}$ = filter size

$p^{[l]}$ = padding

$s^{[l]}$ = stride

Input: $n_H^{[l-1]}$x$n_W^{[l-1]}$x$n_c^{[l-1]}$

Output: $n_H^{[l]}$x$n_W^{[l]}$x$n_c^{[l]}$

$n_H^{[l]} = [\frac{n_H^{[l-1]} + 2p^{[l]} - f^{[l]}}{s^{[l]}}] + 1$

$n_W^{[l]} = [\frac{n_W^{[l-1]} + 2p^{[l]} - f^{[l]}}{s^{[l]}}] + 1$

$n_c^{[l]}$ = number of filters

Each filter is : $f^{[l]}$x$f^{[l]}$x$n^{[l-1]}_c$

Activations: $a^{[l]} = n_H^{[l]}$x$n_W^{[l]}$x$n_c^{[l]}$

Weights: $f^{[l]}$x$f^{[l]}$x$n_c^{[l-1]}$x$n_c^{[l]}$

bias: 1x1x1x$n_c^{[l]}$

### 5.6 Example of ConvNet

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN10.png)

Types of layers in a convolutional network:

- Convolution  (conv)
- Pooling  (pool)
- Fully connected  (FC) 

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN13.png)

图中的FC3是将pool2的unit 展开成一个列向量，然后fully connected to FC3

有的地方一个或多个conv layer加上一个max pooling layer称为一个layer，有的地方看作是separated layers

最后一个softmax layer对于手写数字识别来说，有十个output units

### 5.7 Pooling Layer

池化层

* reduce the size of representation

* speed the computation

**Max Pooling**

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN11.png)

因此，最大运算的作用是在任意位置检测到许多特征，然后将这些象限之一保留在最大池化的输出中。

Pooling的特点是：只有超参数，没有需要学习的参数

如果输入的channel大于1，则对于每一个channel都进行一次图示的pooling操作

**Average Pooling**

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN12.png)

**Hyperparameters for pooling**

f —— filter size

s —— stride

choice —— Max or average pooling

对于池化层，一般没有padding p

一般的f和s的选择是$f=2,s=2$

In max pooling layers，如果input是$n_H$x$n_W$x$n_c$，output的size是$([\frac{n-f}{s}]+1)$x$([\frac{n-f}{s}]+1)$x$n_c$

### 5.8 Advantage of Convolution

* parameter sharing
  * A feature detector (such as a vertical edge detector) that's useful in one part of the image is probably useful in another part of image
* sparsity of connections
  * In each layer, each output value depends only on a small number of inputs

### 5.9 TF in CNN

* tf.keras.layers.ZeroPadding2D(padding=(1, 1), data_format=None)
  * data_format='channels_last'
    * (batch, rows, cols, channels)
  * data_format='channels_first'
    * (batch, channels, rows, cols)

* tf.keras.layers.Conv2D(filters, kernel_size, strides)
* tf.keras.layers.BatchNormalization(axis=-1)
* tf.keras.layers.ReLU()
* tf.keras.layers.MaxPool2D(pool_size=(2, 2), strides=None, padding='valid')

#### 5.9.1 Sequential API

```python
def happyModel():
    """
    Implements the forward propagation for the binary classification model:
    ZEROPAD2D -> CONV2D -> BATCHNORM -> RELU -> MAXPOOL -> FLATTEN -> DENSE
    
    Note that for simplicity and grading purposes, you'll hard-code all the values
    such as the stride and kernel (filter) sizes. 
    Normally, functions should take these values as function parameters.
    
    Arguments:
    None

    Returns:
    model -- TF Keras model (object containing the information for the entire training process) 
    """
    model = tf.keras.Sequential()
    model.add(tf.keras.Input(shape=(64, 64, 3)))
    model.add(tfl.ZeroPadding2D(padding=3))
    model.add(tfl.Conv2D(filters=32, kernel_size=7, strides=1))
    model.add(tfl.BatchNormalization(axis=3))
    model.add(tfl.ReLU())
    model.add(tfl.MaxPool2D())
    model.add(tfl.Flatten())
    model.add(tfl.Dense(1, activation='sigmoid'))
    return model
```



#### 5.9.2 Functional API

```python
def convolutional_model(input_shape):
    """
    Implements the forward propagation for the model:
    CONV2D -> RELU -> MAXPOOL -> CONV2D -> RELU -> MAXPOOL -> FLATTEN -> DENSE
    
    Note that for simplicity and grading purposes, you'll hard-code some values
    such as the stride and kernel (filter) sizes. 
    Normally, functions should take these values as function parameters.
    
    Arguments:
    input_img -- input dataset, of shape (input_shape)

    Returns:
    model -- TF Keras model (object containing the information for the entire training process) 
    """

    input_img = tf.keras.Input(shape=input_shape)
    ## CONV2D: 8 filters 4x4, stride of 1, padding 'SAME'
    # Z1 = None
    ## RELU
    # A1 = None
    ## MAXPOOL: window 8x8, stride 8, padding 'SAME'
    # P1 = None
    ## CONV2D: 16 filters 2x2, stride 1, padding 'SAME'
    # Z2 = None
    ## RELU
    # A2 = None
    ## MAXPOOL: window 4x4, stride 4, padding 'SAME'
    # P2 = None
    ## FLATTEN
    # F = None
    ## Dense layer
    ## 6 neurons in output layer. Hint: one of the arguments should be "activation='softmax'" 
    # outputs = None
    # YOUR CODE STARTS HERE
    conv1 = tfl.Conv2D(filters=8, kernel_size=(4, 4), strides=1, padding='same')
    Z1 = conv1(input_img)
    relu1 = tfl.ReLU()
    A1 = relu1(Z1)
    max_pool1 = tfl.MaxPool2D(pool_size=(8, 8), strides=8, padding='same')
    P1 = max_pool1(A1)
    conv2 = tfl.Conv2D(filters=16, kernel_size=(2, 2), strides=1, padding='same')
    Z2 = conv2(P1)
    relu2 = tfl.ReLU()
    A2 = relu2(Z2)
    max_pool2 = tfl.MaxPool2D(pool_size=(4, 4), strides=4, padding='same')
    P2 = max_pool2(A2)
    flatten = tfl.Flatten()
    F1 = flatten(P2)
    dense = tfl.Dense(6, activation='softmax')
    outputs = dense(F1)
    # YOUR CODE ENDS HERE
    model = tf.keras.Model(inputs=input_img, outputs=outputs)
    return model
```

### 5.10 Classic Networks

#### 5.10.1 LeNet - 5

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN14.png)

* 当时人们还是喜欢使用average-pool，现在最好使用max pool
* 每一个conv和pool之后，需要使用一个non-linearity function比如ReLU

#### 5.10.2 AlexNet

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN15.png)

#### 5.10.3 VGG - 16

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN16.png)

* hyperparameters 少
* conv layer都是same padding，因此只用来increase number of channels
* max pool用来shrink height and width by a factor of 2

### 5.11 ResNet

Residual Network 残差网络  (用来训练very deep neural network)

Very deep neural networks一般训练起来很困难，由于梯度消失和梯度爆炸的问题

**Residual Block**

shortcut / skip connection

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN17.png)

* 将$l$层的activation value越过$l+1$层，传播到$l+2$层的非线性函数前、线性乘加运算后，与$l+2$的Z进行加法运算

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN18.png)

图中有5个Residual blocks

* 理论上，随着层数的增加，DNN的training error会越来越小
* 但是实际上，是先减后增的趋势，因为层数越大，需要训练的时间就越长
* 而ResNet随着层数的增加，不会出现plain network先减后增的趋势，是一直递减的，这对于训练DNN很有帮助

**Why residual network works**

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN19.png)

当使用正则化的时候，W和b都趋近于0，所以$a^{[l+2]} = g(a^{[l]})=a^{[l]}$

=> you can stack on additional ResNet blocks with little risk of harming training set performance

由于大部分conv layer使用same padding，所以$a^{[l]}$和$a^{[l+2]}$一般维度都相同，称为“Identity Block'”

如果维度不同，可以使用一个系数矩阵，这个系数矩阵可以是训练出来的参数，或者是将$a^{[l]}$通过zero padding，成为和$a^{[l+2]}$一个维度

也可以在shortcut 上增加一个Conv layer，称为“Convolutional Block”

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN35.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN20.png)

**What you should remember**:

- Very deep "plain" networks don't work in practice because vanishing gradients make them hard to train.  
- Skip connections help address the Vanishing Gradient problem. They also make it easy for a ResNet block to learn an identity function. 
- There are two main types of blocks: The **identity block** and the **convolutional block**. 
- Very deep Residual Networks are built by stacking these blocks together.

### 5.12 1X1 Convolution

也称为Networks in Networks

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN21.png)

可以用来增加、减小、保持不变number of channels

也可以增加non-linearity

### 5.13 Inception Network

初始网络

当CNN不知道使用哪种filter或者pool size的时候，你可以全部使用它们

same padding使得output的height和width的维度相同

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN22.png)

但是这样会产生computation cost的问题

Computational Cost = #filter params  *  #filter positions  *  #of filters

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN23.png)

multiply的计算次数 `28*28*32  *  5*5*192`

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN24.png)

multiply的次数`28*28*16  * 1*1*192 + 28*28*32  * 5*5*16    ~~ 12.4M`

CONV 1X1 layer也被称为 “bottleneck layer”

**Inception Module**

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN25.png)

**Inception Network**

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN26.png)

Inception Network是由多个Inception Module级联而成

此外，Inception Network还有很多branch（图中绿色圈出来的地方），这些也是用来预测结果的（说明intermediate layers也有很好的performance，即low error）

### 5.14 MobileNet

一般使用在low compute environment，比如手机

**Normal Convolution**

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN27.png)

**Depthwise Separable Convolution  深度可分离卷积**

1. Depthwise Convolution
2. Pointwise Convolution

注：图中的filters的channel应该是6

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN28.png)

1. Depthwise Convolution

   ![](D:\大学\笔记\深度学习\NG-DL_pic\CNN29.png)

   输入的channel是$n_c$，filters的channel也是$n_c$，但是与Normal Convolution不同的是，NC是将$n_c$个`height*width`和`f*f`的矩阵相乘再相加，结果是one channel

   而Depthwise Convolution是将每一个channel相乘再相加，结果还是$n_c$个channels

2. Pointwise Convolution

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN30.png)

与Normal Convolution类似，但是filter size是1X1

**MobileNet**

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN31.png)

* MobileNet v1
  * 使用Depthwise Separable Convolution 13 times
* MobileNet v2
  * 使用了Residual Connection
  * 在Depthwise Separable Convolution的基础上添加了了Expansion操作
  * 注意：这里的projection操作与pointwise convolution相同，只是换了一个名字
  * 使用ResBlock和Depthwise Separable Convolution 17 times

Expansion操作用来让Convolution学到more complex functions

Projection是为了减少内存的使用

**What you should remember**:

* MobileNetV2's unique features are: 
  * Depthwise separable convolutions that provide lightweight feature filtering and creation
  * Input and output bottlenecks that preserve important information on either end of the block
* Depthwise separable convolutions deal with both spatial and depth (number of channels) dimensions

### 5.15 EfficientNet

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN33.png)

如果Resolution变大，变小，如何调整NN的depth（更多layers）和width（每一层更多的W，即filter_size）

可以使用一个好的开源的EfficientNet，可以有效地帮你平衡这些超参数的选择

### 5.16 Transfer Learning in Computer Vision

如果训练集很小，可以在一些开源的数据集上下载模型和其参数weights，然后去除最后的softmax layer，固定前面的layers的参数，重新训练output layer的参数

还有一种比较简单的方法，去除softmax layer后，输入数据集，得到NN的feature matrix $a^{[L-1]}$

由于之前的参数固定，所以$a^{[L-1]}$也不会修改，然后只训练softmax layer（这时是一个Shallow NN）

自己拥有的数据集越大，固定的参数数量可以越小，可以训练的layers可以越多

一种极端的情况，下载的开源的NN作为initialization parameters，重新训练整个网络（这种比随机初始化参数好）

**What you should remember**:

* To adapt the classifier to new data: Delete the top layer, add a new classification layer, and train only on that layer
* When freezing layers, avoid keeping track of statistics (like in the batch normalization layer)
* Fine-tune the final layers of your model to capture high-level details near the end of the network and potentially improve accuracy 

### 5.17 Data Augmentation in Computer Vision

* Mirroring
* Random Cropping 随机裁剪
* Other less used
  * Rotation
  * Shearing
  * Local warping
* Color shifting
  * 随机增减RGB channel的数值
  * 特别地，PCA Color Augmentation
    * 增强颜色深的，减弱颜色前的channels

**Implementation distortions during training**

一个或几个线程load data and implement distortions，将output传递给other threads for training



### 5.18 State of Computer Vision

![](D:\大学\笔记\深度学习\NG-DL_pic\CNN34.png)

image recognition需要大量的数据，而目前的数据量还未达到需求，所以需要more hand-engineering tasks (手动设计features、network architectures、其他components)

**Tips for doing well on benchmarks / winning competitions**

1. Ensemble learning集成学习
   * Train several NN independently and average their outputs
   * 缺点：cost很大
2. Multi-crop at test time
   * Run classifier on multiple versions of test images and average results
   * eg. 10-crop
     * 左上、左下、右上、右下、中间五个部分的裁剪，然后mirror之后，再取上述五个部分的裁剪

注：这些一般不会使用在生产环境的项目中，只是为了提高类似Kaggle的成绩

## 6 Object Detection

Image classification：判断是否含有某个class

Classification with localization：判断是否含有某个class，如果有，定位该class

Detection：判断是否含有某些classes，如果有，定位这些classes

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_1.png)

### 6.1 Object Localization

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_2.png)

在监督学习中，我们不仅提供数据，还提供数据对应的标签

为了正确地定位这些物体，在标签中提供物体中心的坐标$b_x、b_y$，物体的宽和高$b_w、b_h$

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_3.png)

因此，在localization任务中，我们这样定义**标签y**

$y = [p_c, b_x, b_y, b_h, b_w, c_1, c_2, c_3, ...]$

$p_c$: 是否含有需要localize的object。如果为0，则剩下的值不关心，可随意指定

$b_x,b_y,b_h,b_w$: 物体中心的坐标，以及物体的高和宽

$c_1, c_2, c_3, ...$: 物体所属的类别

**损失函数loss function**

$y_1 = 1,\quad L(\hat{y}, y) = (\hat{y_1} - y_1)^2 + (\hat{y_2} - y_2)^2 + (\hat{y_3} - y_3)^2 + (\hat{y_4} - y_4)^2 + ...$

$y_1 = 0,\quad L(\hat{y}, y) = (\hat{y_1} - y_1)^2$

### 6.2 Landmark Detection

神经网络不仅进行classification任务，还可以输出一些重要的点和图像的坐标，这些重要的点和图像一般被称为Landmark

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_4.png)

Landmark对应的东西需要是consistent

### 6.3 Object Detection

**Sliding Window Algorithm**

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_5.png)

我们规定一个window with a small size，然后去取出image中每一个这样大小的图片（根据一个固定的stride），判断是否包含object

然后增大window size，重复操作

缺点：huge computation cost

### 6.4 Convolutional Implementation of Sliding Windows

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_6.png)

将FC layer转换成Conv layer（filter size和上一层的维度相同，filter的个数是原来FC layer的神经元个数）

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_7.png)

由于Sliding Window Algorithm每次单独计算一个window size的图片，对于一张完整的图片，需要进行多次conv 操作，计算量很大

与之前不同的是，现在把FC layer转换为Conv Layer，将完整的图片喂入ConvNet，最终得到一个$n_{out}$X$n_{out}$X$n_{class}$

$n_{out}$维矩阵中的每一个是当前class(类别) channel中每一个window size的概率（此时window size是原来ConvNet支持的图片尺寸，在上图中是14X14，而stride是进行pool操作的pool_size的乘积，上图的pool_size是2，则stride是2）

### 6.5 Bounding Box Predictions - YOLO

使用Sliding Window得到的bounding box不是特别准确

**YOLO Algorithm (You Only Look Once)**

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_8.png)

将图片分为3X3的grid cell（一般实际中是19X19）

然后重新标注label y，在YOLO算法中，object只在那个包含其midpoint的grid中显示为有object，其余为无object

每一个grid都是一个classification和localization的任务，所以每一个grid都有一个label。在图示中是一个8X1的vector，因此label的维度是 3X3X8（注：图中所示的算法中，每一个grid只能最多有一个object，多object的算法之后会叙述）

此时，可以将整个图片进行Conv操作了（节省computation）

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_9.png)

$b_x, b_y，b_h, b_w$都是相对于grid的

如果grid的范围是(0, 0)到(1, 1)，则$b_x,b_y$是在0到1之间的，而$b_h, b_w$则有可能超过1，因为object可以跨越多个grid，但是在YOLO算法中，它只属于一个grid

### 6.6 Intersection Over Union

为了evaluate the object detection algorithm，Intersection Over Union (IoU)计算predicted bounding box和true bounding box的交集（intersection）和并集（union），计算其ratio，观察是否大于一个threshold（一般不低于0.5）

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_10.png)

### 6.7 Non-max Suppression

非最大值抑制

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_11.png)

先highlight largest pc bounding boxes，然后把与这些bounding boxes的IoU很高的box删除

only output maximal probabilities classifications（score = $p_c*c_i$， for i in number of class），but suppress the close-by ones that are not maximal

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_12.png)

当是multiple object detection任务时，non-max suppression需要被同时使用多次

### 6.8 Anchor Boxes

A method to allow one grid cell to detect multiple objects

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_13.png)

对于Anchor box1来说，它同时检测到了two objects，但是行人的bounding box与Anchor Box1的IoU更高，所以Anchor box1的标签是行人

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_14.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_15.png)

此时Anchor Box不能解决three objects in two anchor boxes

### 6.9 YOLO

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_16.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_17.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_18.png)

预测之后需要对每一个class做一次non-max classification

### 6.10 Region Proposal

R-CNN or regions with CNN

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_19.png)

First run the segmentation algorithm to get the proposed regions, and then use the conv classifier to classify them

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_20.png)

### 6.11 Semantic Segmentation With U-Net

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_21.png)

Semantic Segmentation判断图片上的每一个像素点的分类class

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_22.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_23.png)

generates a whole matrix of labels

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_24.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_25.png)

Instead of shrinking height and width of the data, at the last several layers, we make the image bigger by using a technique called **transpose convolutions**.

### 6.12 Transpose Convolutions

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_26.png)

place the filter on the output (padding和stride都是使用在output)

由于padding的位置用0填充，其余位置是input的元素与filter对应位置元素的乘积

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_27.png)

由于stride=2，左移两格，重复上面计算，此时发现有些格子的数值不为0（与之前计算重叠），进行相加操作

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_28.png)

### 6.13 U-Net

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_29.png)

CNN的前半部分是Normal Convolution，用来compress the image

CNN的后半部分是Transpose Convolution，用来blow the representation size back to the original input image

使用skip connections （knowledge from ResNet）用来让最后的layers传递detailed high resolution, low level feature information  (因为原本main path上已经变成了low resolution, high level spatial information)

![](D:\大学\笔记\深度学习\NG-DL_pic\object_detection_30.png)

输入是`h*w*3`，输出是`h*w*number of class`（每个像素的class的概率）

对输出在axis=number of class上使用argmax（预测每个像素的label）

## 7 Face Recognition

### 7.1 Face Verification vs. Face Recognition

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_1.png)

### 7.2 One Shot Learning

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_2.png)

对于这种少样本学习来说，直接将数据feed into CNN训练是不切实际的，因为这样做之后CNN的performance很差

而且如果有新的人加入，我们需要重新训练网络参数

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_3.png)

一种解决方式是学习一个“similarity”（相似度）函数

该函数会告知输入的两张图片的相似度，如果两张图片相似度越高，函数返回值越小

通过超参数$\tau$，我们可以规定小于等于该超参数的值，判断为这两个图片相同

这个不仅可以用在face verification，也可以用在face recognition

当有新的人加入网络，只需要保存他的图片，需要进行recognition时，进行相似度比较即可

### 7.3 Siamese Network

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_4.png)

不使用Softmax layer，而是最后用一个FC layer with 128 units to denote the representation of that person，this representation is called "encoding of the person"$f(x^{(1)})$

我们定义函数$d(x^{(1)}, x^{(2)}) = ||f(x^{(1)}) - f(x^{(2)})||_2^2$来表示similarity

d越小，两个图片相似的可能性越高

### 7.4 Triplet Loss

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_6.png)

Triplet Loss的每个样本需要三张image，一张是Anchor Image，一张是Positive Image（与Anchor Image的人是同一个），还有一张是Negative Image（与Anchor Image的人不同）

我们希望$d(A, P) \le d(A, N)$，但是这样定义loss function的坏处是：也许学习到的参数，使得所有的图片的相似度都是0，或者都是N（$N-N=0$）

我们定义一个超参数$\alpha$，这个超参数表明了$d(A, P)$与$d(A, N)$之间的margin，即两者相似度的差至少大于$\alpha$

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_7.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_8.png)

如果随机构造triplet，会导致loss function很容易被满足。因为对于随机的Negative Image，system往往预测的都是正确的

所以需要special method（比如FaceNet）

### 7.5 Face Verification and Binary Classification

除了使用Triplet Loss方法，还有一种方法是将其作为Binary Classification

![](D:\大学\笔记\深度学习\NG-DL_pic\face_recognition_9.png)

与刚才的Triplet Loss不同，在原先的network architecture之后添加一个logistic layer，使用sigmoid函数做二分类问题（1是相同，0是不同）

一个小技巧是：对于在database中的图片，可以precompute它的encoding，并储存在database中

来了一张新图片，只需要对其进行encoding，然后将它与数据库中预先计算好的feature encoding进行二分类比较

## 8 Neural Style Transfer

### 8.1 Neural Style Transfer

Generate new image from the content image by using style of the style image



![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_1.png)

### 8.2 What are deep ConvNets learning?

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_2.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_3.png)

Deeper layers可以识别more varied and complicated features

### 8.3 Cost Function

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_4.png)

$\alpha = 1 - \beta$

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_5.png)

### 8.4 Cost Content Function

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_6.png)

layer l一般不能太小或太大

太小：生成的图片和edge类似

太大：生成的图片有具体的事物

### 8.5 Style Cost Function

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_7.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_8.png)

如果说两个image，一个是vertical textures，一个是yellowish，是correlated，那么说明，vertical textures的图片也会是yellowish的可能性很大

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_9.png)

$n_c$是channel的数量

$G_{k,k^{'}}^{[l]}$是一张图片在layer l的相似度，其中任意两个channel的相似度之和（style matrix）

数值越大，相似度越高

$J_{style}^{[l]}(S,G)$是两张图片在layer l的相似度（准确说是两张图片相似度的Frobenius距离）

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_10.png)

So if you want the generated image to softly follow the style image, try choosing larger weights for deeper layers and smaller weights for the first layers. In contrast, if you want the generated image to strongly follow the style image, try choosing smaller weights for deeper layers and larger weights for the first layers.

### 8.6 1D and 3D Generalizations

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_11.png)

![](D:\大学\笔记\深度学习\NG-DL_pic\neural_style_transfer_12.png)

3D Data：CAT Scan、medical scans和movie data

### What you should remember
- Neural Style Transfer is an algorithm that given a content image C and a style image S can generate an artistic image
- It uses representations (hidden layer activations) based on a pretrained ConvNet. 
- The content cost function is computed using one hidden layer's activations.
- The style cost function for one layer is computed using the Gram matrix of that layer's activations. The overall style cost function is obtained using several hidden layers.
- Optimizing the total cost function results in synthesizing new images.



## References

### 5

- [The Sequential model](https://www.tensorflow.org/guide/keras/sequential_model) (TensorFlow Documentation)
- [The Functional API](https://www.tensorflow.org/guide/keras/functional) (TensorFlow Documentation)

### 5

- [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385) (He, Zhang, Ren & Sun, 2015)
- [deep-learning-models/resnet50.py/](https://github.com/fchollet/deep-learning-models/blob/master/resnet50.py) (GitHub: fchollet)
- [MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/abs/1704.04861) (Howard, Zhu, Chen, Kalenichenko, Wang, Weyand, Andreetto, & Adam, 2017)
- [MobileNetV2: Inverted Residuals and Linear Bottlenecks](https://arxiv.org/abs/1801.04381) (Sandler, Howard, Zhu, Zhmoginov &Chen, 2018)
- [EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://arxiv.org/abs/1905.11946) (Tan & Le, 2019)

### 6

- [You Only Look Once: Unified, Real-Time Object Detection](https://arxiv.org/abs/1506.02640) (Redmon, Divvala, Girshick & Farhadi, 2015)
- [YOLO9000: Better, Faster, Stronger](https://arxiv.org/abs/1612.08242) (Redmon & Farhadi, 2016)
- [YAD2K](https://github.com/allanzelener/YAD2K) (GitHub: allanzelener)
- [YOLO: Real-Time Object Detection](https://pjreddie.com/darknet/yolo/)
- [Fully Convolutional Architectures for Multi-Class Segmentation in Chest Radiographs](https://arxiv.org/abs/1701.08816) (Novikov, Lenis, Major, Hladůvka, Wimmer & Bühler, 2017)
- [Automatic Brain Tumor Detection and Segmentation Using U-Net Based Fully Convolutional Networks](https://arxiv.org/abs/1705.03820) (Dong, Yang, Liu, Mo & Guo, 2017)
- [U-Net: Convolutional Networks for Biomedical Image Segmentation](https://arxiv.org/abs/1505.04597) (Ronneberger, Fischer & Brox, 2015)

### 7&8

- [FaceNet: A Unified Embedding for Face Recognition and Clustering](https://arxiv.org/pdf/1503.03832.pdf) (Schroff, Kalenichenko & Philbin, 2015)
- [DeepFace: Closing the Gap to Human-Level Performance in Face Verification](https://research.fb.com/wp-content/uploads/2016/11/deepface-closing-the-gap-to-human-level-performance-in-face-verification.pdf) (Taigman, Yang, Ranzato & Wolf)
- [facenet](https://github.com/davidsandberg/facenet) (GitHub: davidsandberg)
- [How to Develop a Face Recognition System Using FaceNet in Keras](https://machinelearningmastery.com/how-to-develop-a-face-recognition-system-using-facenet-in-keras-and-an-svm-classifier/) (Jason Brownlee, 2019)
- [keras-facenet/notebook/tf_to_keras.ipynb](https://github.com/nyoki-mtl/keras-facenet/blob/master/notebook/tf_to_keras.ipynb) (GitHub: nyoki-mtl)
- [A Neural Algorithm of Artistic Style](https://arxiv.org/abs/1508.06576) (Gatys, Ecker & Bethge, 2015)
- [Convolutional neural networks for artistic style transfer](https://harishnarayanan.org/writing/artistic-style-transfer/)
- [TensorFlow Implementation of "A Neural Algorithm of Artistic Style"](http://www.chioka.in/tensorflow-implementation-neural-algorithm-of-artistic-style)
- [Very Deep Convolutional Networks For Large-Scale Image Recognition](https://arxiv.org/pdf/1409.1556.pdf) (Simonyan & Zisserman, 2015)
- [Pretrained models](https://www.vlfeat.org/matconvnet/pretrained/) (MatConvNet)









## 编写代码的流程

**参数学习**

1. 加载数据
2. 预处理
   1. 了解数据的dimension
   2. reshape 数据
   3. 数据的标准化（通常是先减去均值，再除以标准差）

3. 初始化参数
4. 训练模型参数
5. 使用学习到的模型参数进行预测
6. 分析结果并总结

第三步和第四步对于神经网络来说可以细致的划分为：

1. Define the model structure
2. Initialize the model's parameters
3. Loop:
   1. Calculate current loss (forward propagation)
   2. Calculate current gradient (backward propagation)
   3. Update parameters (gradient descent)

## 算法常见bug

1. np.sum返回的是一个秩为1的数组，不是向量，也不是矩阵
2. 