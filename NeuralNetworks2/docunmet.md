## 从逻辑回归到神经网络
### 神经网络与逻辑回归的联系
神经网络主要应用场景之一是分类，逻辑回归也是解决分类问题的一类机器学习算法，实际上两者有着很紧密的关系，逻辑回归过程，用神经元的表达方式就是“感知器”。但神经网络有个重大的作用：它方便我们以此为基础做逻辑回归的多层组合嵌套——比如对同样的输入x(X1,X2)可以同时做n个逻辑回归，产生n个输出结果。
### 神经网络与逻辑回归的区别
逻辑回归的比较擅长解决线性可分的问题。

对于非线性可分的问题，逻辑回归有一种运用复杂映射函数的思路，但这种思路只做了一次（非线性的）几何变换，就得到了线性可分的情形。

神经网络的视角则给我们提供了另一种思路，可以连续做几次的几何变换，每次变换都是一些极简单的逻辑回归，最终解决问题。


## 1. BP神经网络的认识
 BP（Back Propagation）神经网络分为两个过程：
- 工作信号正向传递子过程
- 误差信号反向传递子过程
 
   
在BP神经网络中，单个样本有m个输入，有n个输出，在输入层和输出层之间通常还有若干个隐含层。实际上，1989年Robert Hecht-Nielsen证明了对于任何闭区间内的一个连续函数都可以用一个隐含层的BP网络来逼近，这就是万能逼近定理。所以一个三层的BP网络就可以完成任意的维到维的映射。即这三层分别是输入层（I），隐含层（H），输出层（O）。如下图示

![image](http://images.cnitblog.com/blog/571227/201411/231429324377605.png)
 
## 2. 隐含层的选取
在BP神经网络中，输入层和输出层的节点个数都是确定的，而隐含层节点个数不确定，那么应该设置为多少才合适呢？实际上，隐含层节点个数的多少对神经网络的性能是有影响的，有一个经验公式可以确定隐含层节点数目，如下：

![image](http://images.cnitblog.com/blog/571227/201412/191145471412994.png)

其中为隐含层节点数目，为输入层节点数目，为输出层节点数目，为之间的调节常数。
## 3. 正向传递子过程
现在设节点和节点之间的权值为![image](http://images.cnitblog.com/blog/571227/201412/191153014238502.png)，节点的阀值为![image](http://images.cnitblog.com/blog/571227/201412/191154200946304.png)，每个节点的输出值为![image](http://images.cnitblog.com/blog/571227/201412/191156149544061.png)，而每个节点的输出值是根据上层所有节点的输出值、当前节点与上一层所有节点的权值和当前节点的阀值还有激活函数来实现的。具体计算方法如下

![image](http://images.cnitblog.com/blog/571227/201412/191423407359095.png)

其中为f激活函数，一般选取S型函数或者线性函数。正向传递的过程比较简单，按照上述公式计算即可。
## 4. 反向传递子过程
在BP神经网络中，误差信号反向传递子过程比较复杂，它是基于Widrow-Hoff学习规则的。假设输出层的所有结果为，误差函数如下

![image](http://images.cnitblog.com/blog/571227/201412/191701487517139.png)

而BP神经网络的主要目的是反复修正权值和阀值，使得误差函数值达到最小。Widrow-Hoff学习规则是通过沿着相对误差平方和的最速下降方向，连续调整网络的权值和阀值，根据梯度下降法，权值矢量的修正正比于当前位置上E(w,b)的梯度，对于第个输出节点有

![image](http://images.cnitblog.com/blog/571227/201412/191451343134484.png) 

假设选择激活函数为

![image](http://images.cnitblog.com/blog/571227/201412/191459283446336.png)

对激活函数求导，得到

![image](http://images.cnitblog.com/blog/571227/201412/191556189854573.png) 

那么接下来针对有

![image](http://images.cnitblog.com/blog/571227/201412/191702462048025.png)

其中有

![image](http://images.cnitblog.com/blog/571227/201412/191600406735354.png)

同样对于有

![image](http://images.cnitblog.com/blog/571227/201412/191643504382508.png)

这就是著名的学习规则，通过改变神经元之间的连接权值来减少系统实际输出和期望输出的误差，这个规则又叫做Widrow-Hoff学习规则或者纠错学习规则。

上面是对隐含层和输出层之间的权值和输出层的阀值计算调整量，而针对输入层和隐含层和隐含层的阀值调整量的计算更为复杂。假设是输入层第k个节点和隐含层第i个节点之间的权值，那么有

![image](http://images.cnitblog.com/blog/571227/201412/191729030012012.png)

其中有

![image](http://images.cnitblog.com/blog/571227/201412/191730391573353.png) 

有了上述公式，根据梯度下降法，那么对于隐含层和输出层之间的权值和阀值调整如下

![image](http://images.cnitblog.com/blog/571227/201412/191830324545227.png)

而对于输入层和隐含层之间的权值和阀值调整同样有

![image](http://images.cnitblog.com/blog/571227/201412/191834597664525.png) 

## 5. BP神经网络的注意点
- BP神经网络一般用于分类或者逼近问题。如果用于分类，则激活函数一般选用Sigmoid函数或者硬极限函数，如果用于函数逼近，则输出层节点用线性函数，即f(x)=x。

- BP神经网络在训练数据时可以采用增量学习或者批量学习。

- 增量学习要求输入模式要有足够的随机性，对输入模式的噪声比较敏感，即对于剧烈变化的输入模式，训练效果比较差，适合在线处理。批量学习不存在输入模式次序问题，稳定性好，但是只适合离线处理。
## 6.标准BP神经网络的缺陷：
- 容易形成局部极小值而得不到全局最优值。BP神经网络中极小值比较多，所以很容易陷入局部极小值，这就要求对初始权值和阀值有要求，要使得初始权值和阀值随机性足够好，可以多次随机来实现。
- 训练次数多使得学习效率低，收敛速度慢。
- 隐含层的选取缺乏理论的指导。
- 训练时学习新样本有遗忘旧样本的趋势。


[BP神经网络](http://blog.csdn.net/acdreamers/article/details/44657439)