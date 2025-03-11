# AGI世代业务新范式大模型商业化与落地方案
## 大模型应用结构解析
prompt发送给大模型，大模型提供反馈  
1. 人类写答案  
   监督学习  
2. 人类选答案  
   依据模型输出的答案进行排序，训练奖励模型  
3. 机器选答案  
   奖励模型选出分数最高的模型  
## 企业或组织内容知识如何解决？  
业务数据→信息提取→切成chunks
## RAG实战中的问题
PDF、excel、word不同格式文档如何处理？tika、pymupdf、pandas  
如何处理分割成不同的chunks？NLTK、spacy、逻辑结构、内容密度  
embedding BERT、robertta、word2vec、milvus、pinecone  
对于小公司来说，使用蒸馏过的模型会比完整的更好  

给制作大模型过程各个步骤中设置打分  

## AI agent
模型llm + 记忆memory + 规划planning skill + 工具tool  

bolt coze
