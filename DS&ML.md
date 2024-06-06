## association analysis  
Market Basket Analysis  
用于在大数据中获取数据关联性  
评价指标  
- support  
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
