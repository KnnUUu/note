## association analysis  
Market Basket Analysis  
用于在大数据中获取数据关联性  
评价指标  
- support  TAGS
  所有数据里，AB同时出现数据的占比  
  数值越高两者同时出现概率越高  
- confidence  
  A出现的数据里，B同时出现的占比  
  用于衡量两者关联性，数值越高A出现时B一起出现的概率更高  
- lift  
  list = confidence / 在所有数据中B出现的占比  
  比1大的话，两者相关性高    

## LLM - Large Language Model
- generate, classify, summary, revise  
- unsupervised learning with text data  
- amount of parameters in model is very high  
- Developed in transformer architecture, before transformer RNN(Recurrent neural network) was the main stream, them LSTM         
- Example: Chatgpt, Bart  

GPT: Generative Pre-trained Transformer 生成式预训练Transformer  
原理很简单，给定一段文字，gpt预测下一个单词，并把概率最高的单词加入文字末尾，重复  

1. 输入首先被分割成小的token  
2. 将token转换为向量，意思接近的单词（例：jump和hop）在向量空间内会比较接近  
   向量差也会有类似的意义，例子：man-woman和uncle-aunt  
3. attention层：各个单词之间互相影响改变向量内部值，原理是同一个单词在不同句子内有不同的意义（例：machine learning model和fashion model）  
4. MLP（或者叫feed-forward）层：分析单词，单数双数、是否人名、是什么语言等等
5. 重复attention层MLP层

将softmax with temperature的T设置为0,生成文章会很老套。设置更高，生成文章会更加具有随机性，更加的新奇or荒诞  
### Transformer
embedding：将token（接近单词）关联到tensor，只有单词自己含义没有联系上下文    
attention：各个tensor互相影响调整内部值，联系上下文  
## Deep Learning
Standardization: make mean of values is 0 and the standard deviation is 1  
Normalization: make data distribution  

Training set: for training  
Validation set: calculate loss and optimise model  
Test set: estimate and analysis mdoel  

- Supervised learning  
  uses labeled training data  
  - Logistic regression
  - Linear regression
  - Decision tree
  - Neural network
- unsupervised learning  
  does not uses labeled training data  
  - Clustering
  - Association rule learning
  - Probability density
  - Dimensionality reduction

# Google Machine Learning Course  
### What is Machine Learning?  
- Supervised learning  
  predicted answer with question  
  - Regression  
    pridict numeric value(price, length of time)  
  - Classification  
    value that states whether or not something belongs to a particular category  
  
- Unsupervised learning  
  identify meaningful patterns among the data  
  - clustering  
  
- Reinforcement learning  
  generates a policy that defines the best strategy for getting the most rewards  
  
- Generative AI  
  take a variety of inputs and create a variety of outputs  
  - Text-to-text  
  - Text-to-image  
  - Text-to-video  
  - Text-to-code  
  - Text-to-speech  
  - Image and text-to-image
  
  To produce unique and creative outputs, generative models are initially trained using an unsupervised approach, where the model learns to mimic the data it's trained on. The model is sometimes trained further using supervised or reinforcement learning on specific data related to tasks the model might be asked to perform  

# Basic
## Math
### 标量、向量、矩阵、张量
- 标量 scalar  
  一个单独的数  
- 向量 vector  
  一组有序排列的数  
- 矩阵 matrix  
  二位数组  
- 张量 tensor  
  超过二维的数组  

## Data
### Cross-validation 交叉验证
- Training Dataset  
  用于训练模型  
- Validation Dataset  
  验证模型预测表现以及调整超参数（Hyperparameter）  
- Test Dataset  
  评估模型

划分数据集方式  
1. 留出法 Holdout cross validation  
   将三种数据集按照比例划分，一般是Training 60% / Validation 20% / Test 20%  

2. 留一法 Leave one out cross validation  
   每次的测试集都只有一个样本，要进行 m 次训练和预测。训练的数据只比整体数据集少了一个样本  
   一般在数据缺乏时使用。  

3. k 折交叉验证 k-fold cross validation  
   1. 将数据集分为训练集和测试集，将测试集放在一边  
   2. 将训练集分为 k 份  
   3. 每次使用 k 份中的 1 份作为验证集，其他全部作为训练集  
   4. 通过 k 次训练后，我们得到了 k 个不同的模型  
   5. 评估 k 个模型的效果，从中挑选效果最好的超参数  
   6. 使用最优的超参数，然后将 k 份数据全部作为训练集重新训练模型，得到最终模型  

[参考](https://easyaitech.medium.com/%E4%B8%80%E6%96%87%E7%9C%8B%E6%87%82-ai-%E6%95%B0%E6%8D%AE%E9%9B%86-%E8%AE%AD%E7%BB%83%E9%9B%86-%E9%AA%8C%E8%AF%81%E9%9B%86-%E6%B5%8B%E8%AF%95%E9%9B%86-%E9%99%84-%E5%88%86%E5%89%B2%E6%96%B9%E6%B3%95-%E4%BA%A4%E5%8F%89%E9%AA%8C%E8%AF%81-9b3afd37fd58)

### Bias VS Variance
- Bias 偏差  
  对训练数据的拟合程度  
- Variance 方差  
  模型复杂程度  

升高Bias降低Variance，反之亦然。
![1_oO0KYF7Z84nePqfsJ9E0WQ](https://github.com/KnnUUu/note/assets/44579350/cc6bd452-8cfc-4c05-b3b3-253af4125ac9)

- irreducible error 不可约误差  
  指即使使用最理想的模型，也无法消除的预测误差。这种误差源于数据本身的固有噪声和不可预测性，例如测量误差、标签错误或未观测到的影响因素  

### Ground Truth
正确打标签的训练数据或简单来说就是有效的正确的数据  

## model
### backbone
模型里的基干部分，一般指用于抽出特征的层  

### head
一般指模型的输出部分  

### Learning Rate Scheduler 学习率调度器
学习率是优化器中的一个关键超参数，决定了模型在每次参数更新时的步长  

### epoch, step, batch
batch: 一小批样本（例如 32 个样本），模型一次训练处理的单位  
step: 一次参数更新的次数，每次处理一个 batch 就是一个 step  
epoch: 整个训练集被完整训练一轮，每看完一遍训练集叫一个 epoch

steps_per_epoch = total_samples // batch_size  
total_steps = steps_per_epoch × epochs  

### softmax层
把模型输出的一组实数（logits）转换成归一化的概率分布  
```python
def softmax(logits):
    # 1. 防止数值溢出：减去最大值（提高稳定性）
    shifted = logits - np.max(logits)
    # 2. 对每个元素求指数
    exps = np.exp(shifted)
    # 3. 除以总和归一化
    return exps / np.sum(exps)
```
### softmax with temperature
当T变大的时候，会给低值赋予更多权重（更高的概率），输出更加随机  
```python
def softmax_temperature(logits, T=1.0):
    shifted = logits - np.max(logits)
    exps = np.exp(shifted / T)
    return exps / np.sum(exps)
```
# Kaggle
![image](https://github.com/user-attachments/assets/693989b7-8555-4cc0-b695-63743abf2062)

# Link
https://medium.com/swlh/cheat-sheets-for-machine-learning-interview-topics-51c2bc2bab4f  
https://sites.google.com/view/datascience-cheat-sheets  
