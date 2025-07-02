# 1.1
机器学习的应用：广告推送、图像识别、医学诊断辅助。。。  
# 1.2
# 2.1
使用频率：监督学习（90%以上） -> 非监督学习 -> 强化学习  
# 2.2
回归 regression：从无数可能数字中预测一个数字  
# 2.3
分类 classification：在有限的结果里预测其中一个  
# 2.4
unsupervised learning: find something interesting in unlabeled data  
clustering 聚类: put unlabeled data into different clusters  
应用：google news、人类基因组分类、给予数据将客户分类  
# 2.5
anomaly detection: find unusual data points  
输入一堆邮件数据，让算法自己分类  
输入金融数据，检测异常  
# 2.6
# 2.7
linear regression 线性回归：为数据拟合一条直线  
trainnig set: 用于训练模型  
x：输入  
y：输出  
m：训练数据量  
(x,y)：单个训练数据  
(x<sup>(i)</sup>,y<sup>(i)</sup>)：第i个数据  
# 2.8
$\hat{y}$：y的估计或预测  
y：训练集中的真实值  
f：模型  

|General <img width=70/> <br />  Notation  <img width=70/> | Description<img width=350/>| Python (if applicable) |
|:------------|:------------------------------------------------------------|----|
| $a$ | scalar, non bold                                                      ||
| $\mathbf{a}$ | vector, bold                                                      ||
| **Regression** |         |    |     |
|  $\mathbf{x}$ | Training Example feature values | `x_train` |   
|  $\mathbf{y}$  | Training Example targets  | `y_train` 
|  $x^{(i)}$, $y^{(i)}$ | $i_{th}$Training Example | `x_i`, `y_i`|
| m | Number of training examples | `m`|
|  $w$  |  parameter: weight,                                 | `w`    |
|  $b$           |  parameter: bias                                           | `b`    |     
| $f_{w,b}(x^{(i)})$ | The result of the model evaluation at $x^{(i)}$ parameterized by $w,b$: $f_{w,b}(x^{(i)}) = wx^{(i)}+b$  | `f_wb` | 

# 3.3
parameter参数：也被叫做 coefficient系数 或 weight权重  
cost function 成本函数：用来衡量模型拟合程度的函数   
error 误差：预测值与实际值的差距  

# 3.4
n：参数维度数量  
 <p align="center">
  <img width="90%" src="images/multiple_features_1.png">
</p>
 <p align="center">
  <img width="90%" src="images/multiple_features_2.png">
</p>

# 3.5
1维：笑脸状函数  
2维：碗状函数  

等高图：loss一样的点，想象一下山的等高图  

# 3.6
👆的具体例子  

# 4.1 梯度下降
squared error cost function（平方误差代价函数）：指MSE  
对于具有平方误差代价函数的线性回归，函数总是呈弓形或者吊床形  
但在实际情况中，可以有多个低谷，叫做局部极小值 local minima  
<p align="center">
  <img width="90%" src="images/gradient_descent.png">
</p>

### 4.2 实现梯度下降
假设loss函数是二维的，有 $w, b$ 两个参数  
则参数以以下的方式更新  

$$
\begin{aligned}
w^{(t+1)} &= w^{(t)} - \alpha\ \frac{\partial J(w^{(t)}, b^{(t)})}{\partial w},\\
b^{(t+1)} &= b^{(t)} - \alpha\ \frac{\partial J(w^{(t)}, b^{(t)})}{\partial b},
\end{aligned}
$$
$ α $：学习速率（learning rate）  
$ t $：参数更新次数  
$  J(w^{(t)}, b^{(t)}) $：损失函数  

注意各个参数必须**同时**更新，不能现更新$ w $再更新$ b $  

### 4.3 梯度下降的直观理解
导数是斜率，斜率越高移动的速度越快，斜率低移动慢  
在函数底部左边时，斜率为正向右移动，在右边时向左移动  

### 4.4 学习率
太小收敛会需要很长的时间  
太常会左右横跳无法收敛  
收敛：损失函数或目标函数达到最小值或近似不再变化，参数更新非常小，模型性能基本不再提升  

### 4.5 线性回归中的梯度下降
梯度下降公式推导  

### 5.1 多类特征
$x_j$（x subscript j）：第j个特征  
$n$：特征数量  
$\vec{x}^{(i)}$（x superscript i）：第i个样本的特征集合  
$x^{(i)}_j$：第i个样本第j个特征  
GPU计算方法：向量点乘  

### 5.2 向量化 part1
- 代码更简洁、高效  
- 使用现代数值线性代数库
- 调用gpu  

向量点积`np.dot(x,w)`

### 5.3 向量化 part2

### 6.1 feature scaling 特征缩放 part1
当一个特征可能取值很大时（300～2000），参数的合理值比较小  
当一个特征可能取值很小时（1～5），参数的合理值比较大  
导致不同特镇对于损失函数的影响不同，梯度下降很难收敛  
所以需要特征缩放来使得各个特征取值范围相同  

### 6.2 feature scaling 特征缩放 part2
- 最简单方法
  直接将值除以最大值  

- mean normalization 均值归一法  
  使得大多数数据在(-1,1)的区间内  
  $$
  x_{\text{norm}} = \frac{x - \mu}{x_{\max} - x_{\min}}
  $$
  $\mu$为平均值  

- z-score normalization z-score归一法   
  作用是使数据均值为0、标准差为1  
  $$
  x_{\text{norm}} = \frac{x - \mu}{\sigma}
  $$
  其中 $x$ 是原始值，$\mu$ 是均值，$\sigma$ 是每个特征的标准差 standand deciation  

目标是使得大部分数据分布在(-1,1)的区间，但稍微超过一点点影响不大  

### 6.3 检查梯度下降是否收敛
学习曲线：loss/epoch  
如果学习途中loss上升，通常说明alpha太大或者有bug  

Automatic convergence test  
每次参数更新后，计算损失函数 $J$ 的变化量，如果连续几次迭代中损失函数的变化小于某个很小的阈值（如 $10^{-3}$（10 to the power of negative 3） 或 $10^{-4}$），就认为算法已经收敛  

### 6.4 学习率的选择
太小：收敛得很慢  
太大：无法收敛  
如果loss没有下降，试着将alpha设置为一个很小的值，如果这样也无法下降的话通常说明有bug  
使用少量的epoch测试一系列的学习率，选出一个能快速降低loss的再开始学习  

### 6.5 特征工程 feature engineering
使用对问题的知识或直觉，通过转换或者结合原有特征来设计新特征，使学习算法更容易作出正确的预测  

