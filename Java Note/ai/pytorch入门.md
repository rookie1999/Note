>视频网址：https://www.bilibili.com/video/BV1hE411t7RN?p=1
>
>代码位置：D:\pycharm\code\pytorch

## 1、pytorch环境的配置

* 下载Anaconda管理器

  * 创建环境

    * 每个环境的依赖可以相同，但是版本可以不同

    * ```
      # 创建了一个环境叫pytorch
      conda create -n pytorch python=3.6
      ```

  * 切换环境

    * ```
      conda activate pytorch
      conda deactive pytorch
      ```
    
  * 显示当前环境的类库

    * ```
      pip list
      ```

* 下载pytorch

  * 支持conda

    * ```
      conda install pytorch torchvision cudatoolkit=9.2 -c pytorch -c defaults -c numba/label/dev
      ```

    * 

  * 不支持conda

    * ```
      conda install pytorch torchvision cpuonly -c pytorch
      ```

  * 查看是否自己的电脑支持cuda

    * ```python
      import torch
      torch.cuda.is_available()
      ```

## 2、学习Python的两大法宝

* dir函数
* help函数



## 3、 Pytorch的两个重要模块

### 3.1 Dataset

* 用来获取数据集中的数据和对应label
  * `__getitem__(self, idx)`
  * 定义好之后可以直接通过`实例对象[idx]`获取数据和label
* 也可以获取数据集的个数
  * `__len__`
* 两个数据集直接通过加法运算能形成一个新的数据集，新数据集的数据为原来两个之和



### 3.2 DataLoader

