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

### Ground Truth
正确打标签的训练数据或简单来说就是有效的正确的数据  

# Kaggle
![image](https://github.com/user-attachments/assets/693989b7-8555-4cc0-b695-63743abf2062)

# Link
https://medium.com/swlh/cheat-sheets-for-machine-learning-interview-topics-51c2bc2bab4f  
https://sites.google.com/view/datascience-cheat-sheets  
