# Tensorflow简介

> 参考资料：
>
> 《TensorFlow技术解析与实战》
> 

Tensorflow是用数据流图做计算的，如下图所示：


![tensorflow](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/Tensorflow/wm.gif)

上图通过一个简单的回归模型讲述了Tensorflow的运行原理，图中包括输入（input）、塑形（reshape）、Relu层（Relu layer）、
Logit层（Logit layer）、Softmax、交叉熵（cross entropy）、梯度（gradient）、SGD训练（SGD Trainer）等部分。

计算过程为：

- 1. 从输入开始，经过塑形后，一层一层向前传播运算。
- 2. Relu层（隐藏层）里面会有两个参数，即W<sub>h1</sub>和b<sub>h1</sub>，在输出前使用ReLu（Rectified Linear Units）激活函数做非线性处理。
- 3. 进入Logit层（输出层），学习两个参数W<sub>sm</sub>和b<sub>sm</sub>。
- 4. 使用Softmax来计算输出结果中各个类别的概率分布。
- 5. 用交叉熵来度量源样本的概率分布和输出结果的概率分布之间的相似性。
- 6. 梯度计算，使用W<sub>h1</sub>、b<sub>h1</sub>、W<sub>sm</sub>和b<sub>sm</sub>以及交叉熵后的结果进行梯度计算。
- 7. SGD训练（反向传播过程），从上往下计算每一层的参数，依次对b<sub>sm</sub>、W<sub>sm</sub>、b<sub>h1</sub>和W<sub>h1</sub>进行更新。


Tensorflow指“张量的流动”，TensorFlow的*数据流图*是由`节点（node）`和`边（edge）`组成的有向无环图（directed acycline graph, DGA）

TensorFlow的组成：
* Tensor：`张量`&rArr;`边`&rArr;`数据`
* Flow：`流动`&rArr;`节点`&rArr;`算子`&rArr;`操作`&rArr;`处理`

##TensorFlow基本概念

### 边

TensorFlow的边有种连接关系：数据依赖和控制依赖


边的连接关系 | 线型 | 是否有数据流过 | 功能
----|------|----|----
数据依赖 | 实线边 | 有数据流过，代表数据 | 张量（数据）在数据流图中从前往后流动一遍完成了一次向前传播（forward propagation），而残差从后往前流动一遍完成一次反向传播（backward propagation）
控制依赖 | 虚线边  | 没有数据流过 | 用于控制操作的运行，被用来确保happens-before关系，这类边上没有数据流过，但源节点必须在目的节点开始执行前完成执行。


### 节点

数据流图中的`节点`又称为`算子`，它代表一个`操作（operation, OP）`，
一般用来表示施加的数学运算，也可以表示数据输入（feed in）的起点以及输出（push out）的重点，
或者是读取/写入持久变量（persist variable）的终点。

### 图

把操作任务描述成有向无环图。构建图的第一步是创建各个节点，具体步骤如下：

    import tensorflow as tf
    # 创建一个常量运算操作，产生一个1 * 2矩阵
    matrix1 = tf.constant([[3., 3.]])
    
    # 创建两天一个常量运算操作，产生一个2 * 1矩阵
    matrix2 = tf.constant([[2.], [2.]])
    
    # 创建一个矩阵乘法运算，把matrix1和matrix2作为输入
    # 返回值product代表矩阵乘法的结果
    product = tf.matmul(matrix1, matrix2)

### 会话

启动图是第一步是创建一个Session对象。会话（session）提供在数据流图中执行操作的一些方法。

一般模式是，建立会话，此时会生成一张空图，在会话中添加节点和边，形成一张图，然后执行。（建立会话&rarr;添加边和节点&rarr;执行）



### 设备

`设备（device）`是指一块可以用来运算并且拥有自己的地址空间的硬件，如CPU和GPU。
TensorFlow为了实现分布式执行操作，充分利用计算资源，可以明确指定操作在哪个设备上执行。

### 变量

`变量（variable）`是一种特殊的数据，它在数据流图中有固定的位置，不向普通张量那样可以流动。

### 内核

`操作（operation）`是对抽象操作（如matmul或者add）的一个统称。
`内核（kernel）`能够运行在特定的设备上（如CPU何GPU）上的一种对操作的实现。因此同一个操作可能会对应多个内核。

在定义一个操作时，需要把新操作和内核通过注册的方式添加到系统中。

