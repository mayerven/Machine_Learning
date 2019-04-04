下图所示是收集到的一部分数据：
![房价数据](https://ws1.sinaimg.cn/large/82e16446gy1g103re5xm9j20pk09fjt0.jpg)

假设其中的“房价”与“房子面积”、“房间数量”、”楼层数“、“房龄”这么几种**特征**的值有关，那么希望通过已有的这些数据建立一个“房价预测”模型，该模型可以根据任意给定的一组特征值，准确地预测出相应的“房价”。

一般情况下，“房价”是根据这几种特征的变化而连续变化的，因此该问题属于一个监督学习下的回归问题。这里不妨先假设“房价”依这几种特征对应的值的变化而呈线性变化，而建立一个**线性回归（Linear Regression）**模型来预测”房价“。

对于其中的$m$个训练样本，每一个都包含了$n=4$种特征的值，如果我们用$x_1$表示“房子面积”，$x_2$表示“房间数量”，$x_3$表示“楼层数”，$x_4$表示“房龄”，那么每一个训练样本都可以用一个**特征向量（feature vector）**来表示，例如第2个训练样本可以表示为：$$x^{(2)}= \begin{bmatrix}  x^{(2)}\_1 \\\ x^{(2)}\_2 \\\ x^{(2)}\_3 \\\ x^{(2)}\_4 \\\ \end{bmatrix} = \begin{bmatrix} 1416 \\\ 3 \\\ 2 \\\ 40 \\\ \end{bmatrix}$$

用$y$表示第$i$个训练样本的“真实房价”，也就是标签，例如$y^{(2)} = 232$，则$(x^{(2)}，y^{(2)})$组成第一个训练样本。$x^{(1)},\cdots,x^{(m)}$形成样本空间$X$，$y^{(1)},\cdots,y^{(m)}$则形成标记空间$Y$。

### 假设函数
在解决监督学习问题时，我们的目标是：给定一个训练集，希望从中学得一个将$X$映射到$Y$的**假设函数（hypothesis function）**$h(x)$，使得对于任意的输入样本$x$，都能通过该假设函数预测出一个与真实标签$y$之间误差较小的$\hat{y}$。如下图所示：
![监督学习](https://ws1.sinaimg.cn/large/82e16446gy1g0e5ppfxajj20az078t8s.jpg)

为“房价预测”问题建立线性回归模型，则其假设函数应为：$$\hat{y} = w_1x_1 + w_2x_2 + w_3x_3 + w_4x_4 + b$$

上式还可以简洁地表示为两个列向量间的矩阵运算：$$\hat{y} = w^Tx + b = [w_1, w_2, w_3, w_4 ] \begin{bmatrix} x_1 \\\ x_2 \\\ x_3 \\\ x_4 \\\ \end{bmatrix} + b $$

### 成本函数
在模型的训练过程中，对于习得的一个假设函数，我们用**损失函数（Loss Function）**来评估其对某一个训练样本的预测结果的准确度，且评估的过程是将假设函数的预测结果与对应的真实标记进行比较，获得它们之间存在的误差。

**均方误差（Mean Squared Error）**就是一种回归问题中常用的损失函数，其表达式为：$$ l^{(i)}(w, b) = (\hat{y}^{(i)} - y^{(i)})^2$$

从上式可知，模型预测的结果越准确，相应损失值将越接近$0$。通常我们用假设函数所有训练样本的损失的平均值来评估其预测质量，将得到如下所示的称为**成本函数（Cost Function）**的表达式：$$ J(w, b) = \frac{1}{2m} \sum_{i = 1}^m (\hat{y}^{(i)} - y^{(i)})^2$$

其中为方便之后的求导运算，上面的成本函数中增添了常数$\frac{1}{2}$。同理，模型预测的结果越准确，相应成本值将越接近$0$，由此有了我们训练模型时目标--最小化成本函数。

如果只考虑“房价”与“房间面积”的关系，假设函数将变成$\hat{y} = w_1x_1 + b$，而成本函数$J$会是关于参数$w_1$、$b$的函数，这时可以在空间坐标系的画出它的图像：
![成本函数图像](https://ws1.sinaimg.cn/large/82e16446gy1g0vmbelgwbj20r40gothv.jpg)

进一步画出其等高线图（contour plot），可以看出，落在等高线包围的中心点上，$J$将取得最小值，对应是其最优的线性回归模型:
![最优模型](https://ws1.sinaimg.cn/large/82e16446gy1g0vmkoaix4j20hy08qwgd.jpg)

### 梯度下降
通常采用**梯度下降法（Gradient Denscent）**来最小化成本函数，来求出假设函数中参数值。

在数学中，梯度指的是一个函数在某一点上一个向量，且该点上的方向导数沿着该向量取得最大值，即在该点处，函数沿着该向量的方向变化最快。由此，只要给成本函数预设一个初始值，使其沿梯度方向迭代下降，便能一步步将其最小化。更为直观的解释如下：
![梯度详解](https://ws1.sinaimg.cn/large/82e16446gy1g0vqqbu88qj20ft08en06.jpg)

在进行梯度下降前，通常要对训练样本进行[特征缩放]()。假设某个成本函数的图像如上，在图像上任取一点后，使其不断沿着梯度方向下降，最后定能落入一个局部的最小值上。另外注意到，当存在多个极小值时，选取的初始位置不同，梯度下降后可能得到多种结果。

要得到所谓的梯度，其实就是对函数进行求导。成本函数常常是一个多元函数，对其求导的过程是求取其关于各参数的偏导数。例如对“房价预测”问题的成本函数$J$，就有其关于$w_1$的偏导数$\frac{\partial}{\partial w_1} J$。

实现梯度下降的具体算法为：$$ \begin{aligned} & \text{Repeat}\ \lbrace \\\ &\ \ \ \ w_j := w_j - \alpha \frac{\partial}{\partial w_j} J(w_0, \cdots, w_n, b) \ \ \ \ j \in \lbrace 1,2...n\rbrace \\\ & \ \ \ \ b := b - \alpha \frac{\partial}{\partial b} J(w_0, \cdots, w_n, b)\\\ & \rbrace \end{aligned}$$
直到其中各参数的值收敛为止。

其中的“:=”意为“赋值”；$\alpha$是称为**学习率（learning rate）**，它可以控制梯度下降时的“步伐”大小，该参数值需要进行人为设定，且不能过大或过小，具体的调参方法见[学习率]()。像这种通过人为设定而非学习得到的参数值，我们统一称他们为**超参数（Hyperparameter）**。

还是只考虑“房价”与“房间面积”的关系，则由初等函数的导数可得：
$$\frac{\partial}{\partial w_1} J(w_1, b) = \frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)} - y^{(i)})x^{(i)}$$ $$\frac{\partial}{\partial b} J(w_1, b) = \frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)} - y^{(i)})$$

而训练该模型的过程，将如下动图所示：
![训练过程](https://ws1.sinaimg.cn/large/82e16446gy1g0wmjwx98ag20xw0fkb29.gif)

### 正规方程
采用上述的梯度下降法，需要经过多次迭代运算，才能得到最优的参数值。对于线性回归模型，另外可采用**正规方程（normal equation）**。一次性计算出模型中各参数的最优解。

对前面线性回归的假设函数，将其中表示偏差的参数$b$换作$w_0$来表示，并且令$x_0 = 1$，
成本函数的具体表达式为：$$ J(w_0, w_1,\cdots, w_n) = \frac{1}{2m} \sum_{i=1}^m (\hat{y}^{(i)} - y^{(i)})$$

要最小化该式，即求出使$J$最小的$w_0, w_1,\cdots, w_n$值，就要令函数$J$关于各权重值$w$的偏导等于$0$，如此得到的$w$将使$J$取得最小值。

具体地，将有：$$\frac{\partial}{\partial w_0} J(w_0, w_1,\cdots, w_n) = \frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)}- y^{(i)}) \cdot x^{(i)}\_0 = 0$$ $$\frac{\partial}{\partial w_1} J(w_0, w_1,\cdots, w_n) = \frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)}- y^{(i)}) \cdot x^{(i)}\_1 = 0$$ $$\vdots$$ $$\frac{\partial}{\partial w_n} J(w_0, w_1,\cdots, w_n) = \frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)}- y^{(i)}) \cdot x^{(i)}\_n = 0$$

根据前面所述，有下式：$$x^{(i)}= \begin{bmatrix}  x^{(i)}\_0 \\\ x^{(i)}\_1 \\\ \vdots \\\ x^{(i)}\_n \\\ \end{bmatrix} \ \ \ i = 1,\cdots,m $$ $$ y = \begin{bmatrix}  y^{(1)} \\\ x^{(2)} \\\ \vdots \\\ y^{(m)} \\\ \end{bmatrix} ,\ \ \ w = \begin{bmatrix}  w_0 \\\ w_1 \\\ \vdots \\\ w_n \\\ \end{bmatrix} $$ $$\hat{y}^{(i)} = w^Tx^{(i)} = (x^{(i)})^Tw$$

此外，约定一个大小为$m \times (n+1)$的矩阵$X$：$$X= \begin{bmatrix}  (x^{(1)})^T \\\ (x^{(2)})^T \\\ \cdots \\\ (x^{(m)})^T \\\ \end{bmatrix} = \begin{bmatrix}  x^{(1)}\_0 &  x^{(1)}\_1 & \cdots & x^{(1)}\_n\\\ x^{(2)}\_0 &  x^{(2)}\_1 & \cdots & x^{(2)}\_n \\\ \vdots & \vdots & \ddots & \vdots \\\ x^{(m)}\_0 &  x^{(m)}\_1 & \cdots & x^{(m)}\_n \\\ \end{bmatrix}$$

由此，可将最小化成本函数时的累加过程转换为矩阵运算，例如有：$$\frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)} - y^{(i)}) \cdot x^{(i)}\_0 = \frac{1}{m}[x^{(1)}\_0, x^{(2)}\_0, \cdots, x^{(m)}\_0] \begin{bmatrix} \hat{y}^{(1)} - y^{(1)} \\\ \hat{y}^{(2)} - y^{(2)} \\\ \vdots \\\ \hat{y}^{(m)}- y^{(m)} \\\ \end{bmatrix} $$ 

而：$$\begin{bmatrix}  \hat{y}^{(1)} - y^{(1)} \\\ \hat{y}^{(2)}- y^{(2)} \\\ \vdots \\\ \hat{y}^{(m)} - y^{(m)} \\\ \end{bmatrix} = \begin{bmatrix}  (x^{(1)})^T \\\ (x^{(2)})^T \\\ \vdots \\\ (x^{(m)})^T\\\ \end{bmatrix} \begin{bmatrix} \theta_0 \\\ \theta_1 \\\ \vdots \\\ \theta_n \\\ \end{bmatrix} - \begin{bmatrix} y^{(1)} \\\ y^{(2)} \\\ \vdots \\\ y^{(m)} \\\ \end{bmatrix}$$ $$ = Xw - y$$

则有：$$\frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)} - y^{(i)}) \cdot x^{(i)}\_0 = \frac{1}{m}[x^{(1)}\_0, x^{(2)}\_0, \cdots, x^{(m)}\_0] (Xw - y) = 0$$ $$\frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)}- y^{(i)}) \cdot x^{(i)}\_1 = \frac{1}{m}[x^{(1)}\_1, x^{(2)}\_1, \cdots, x^{(m)}\_1] (Xw - y) = 0$$ $$\vdots $$ $$\frac{1}{m} \sum_{i=1}^m (\hat{y}^{(i)} - y^{(i)}) \cdot x^{(i)}\_n = \frac{1}{m}[x^{(1)}\_n, x^{(2)}\_n, \cdots, x^{(m)}\_n] (Xw - y) = 0$$ 

将上式进一步矩阵化有：$$\frac{1}{m}\begin{bmatrix}  x^{(1)}\_0 &  x^{(1)}\_1 & \cdots & x^{(1)}\_n\\\ x^{(2)}\_0 &  x^{(2)}\_1 & \cdots & x^{(2)}\_n \\\ \vdots & \vdots & \ddots & \vdots \\\ x^{(m)}\_0 &  x^{(m)}\_1 & \cdots & x^{(m)}\_n \\\ \end{bmatrix}^T(Xw -y) = \frac{1}{m}X^T(Xw - y) = 0$$ 

并进一步整理上式，就能得到可一步计算出线性回归模型中$\theta$值的正规方程：$$ w = (X^TX)^{-1}X^Ty $$

要注意，上式中$X^TX$为奇异矩阵或非方阵时，它将不存在逆矩阵，造成该问题的原因可能是训练样本的特征类型中存在冗余特征，也可能是特征数量$n$大于等于训练样本数量$m$，要解决该问题可以某些特征或将样本进行[正则化]()。但在MATLAB或python中，始终可以用pinv()求得$X^TX$的逆矩阵或伪逆矩阵。

在“房价预测”中，有：$$X= \begin{bmatrix}  1 & 2104 & 5 & 1 & 45 \\\ 1 & 1406 & 3 & 2 & 40 \\\ 1 & 1534 & 3 & 2 & 30  \\\ 1 & 852 & 2 & 1 & 36 \\\ \vdots & \vdots & \vdots & \vdots & \vdots \\\ \end{bmatrix} y = \begin{bmatrix}  460 \\\ 232 \\\ 315 \\\ 178 \\\ \vdots \\\ \end{bmatrix}$$ 

据此，就可以通过简单的矩阵运算而得到参数$w$的值。

比较这两种参数学习方法，可知：

采用梯度下降法训练模型时：
* 需要调参（$\alpha$）
* 需要多次迭代计算
* 当样本的特征（$n$）数量很多时也能保持较好的性能
* 除线性回归模型外还适用于逻辑回归等其他模型

而对正规方程：
* 不需要调参
* 一次性完成计算
* 当$n$比较大时，$(X^TX)^{-1}$的运算量较大，性能较差
* 只适用于线性回归模型
  
因此，在训练模型的过程中要根据实际的情况，选择学习参数的方法。

***
#### 相关程序

* [ng_machine_learning-ex1-python](https://github.com/BinWeber/Machine_Learning/tree/master/Machine_Learning_Ng/ex1/python)

#### 参考资料
1. [Andrew Ng-Machine Learning-Coursera](https://www.coursera.org/learn/machine-learning/)
2. [吴恩达-机器学习-网易云课堂](https://study.163.com/course/introduction/1004570029.htm)
3. [Normal equation公式推导-CSDN](https://blog.csdn.net/zhangbaodan1/article/details/81013056)
4. [掰开揉碎推导Normal Equation-知乎](https://zhuanlan.zhihu.com/p/22757336)

更新历史：
* 2019.04.04 完成初稿
