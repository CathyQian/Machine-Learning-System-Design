# How to answer machine learning system design questions?

Below are notes from [ref1](https://github.com/chiphuyen/machine-learning-systems-design), [ref2](https://leetcode.com/discuss/interview-question/system-design/566057/machine-learning-system-design-a-framework-for-the-interview-day)

面试中一些大的原则是：
1. 先明白需求，然后考虑大框架，最后是具体设计。
2. 没有完美的设计，要懂得如何做出取舍（trade-off）
 
# End-to-end Machine Learning Pipeline
- define problems

    - What is the goal? Any secondary goal?
        e.g. for CTR - maximizing the number of clicks is the primary goal. A secondary goal might be the quality of the ads/content
    - Output? Think/draw a very simple diagram with input/output line between system backend and ML system
    - The scale of the system - how many users, how much content?
    - Do we need machine learning? Start with herustic model as baseline, then use ML for better performance. Add complexity to the ML model gradually.
    - Machine learning method: supervised or unsupervised? classification or regression or ranking? 
    
- data availability and collection
    - what kind of data (system log data, third party data, partner company data) and how much data you need -- both for training and serving?
    - are data annotated or not?
    - can I utilize weakly supervised or unsupervised method to automatically create new annotated data?
    - what data do you need from users? How do you collect it? i.e. user's feedback
    - how often does new data come in?
    - how big is the data? how is it stored?
    - privacy concerns about user data? can I store user data on my servers or can only access data on user's device?

- data preprocessing
    - Is there a missing value? 
        - if missing at random, deletion has no bias effect but decrease the power of analysis by decreasing the effective sample size
        - data imputation: 
            - fill in with simple guess (mean/median, 0, mode or other values)
            - nearest neighbor imputation
            - multivariant imputation
        - use algorithms that can deal with missing values (i.e., xgboost, random forest, lightGBM)
    - Is there an unexpected value for one/more data columns? How do you know if its a typo etc. and decide to ignore?
        - univariant (IQR, z-score)
        - multivariant (model-based automatic outlier detection)
     - Is the data balanced? If not do you need to resample the data?
        - use AUC/logloss/recall/precision/f1/confusion matrix as evaluation metric
        - resample the data
            - bagging (bootstrapping with replacement) the minority class
            - oversampling (i.e., SMOTE)/ undersampling
            - cluster sample (cluster the abundant class, then only use the centroid of the cluster)
            - stratified sample(preserve the percentage of samples for each class in each fold)
        - modify cost function to penalize error on the minority
        - use algorithms that can deal with unbalanced class (i.e., xgboost)
    - do you need to normalize data?
    - what bias do you have in the data (gender, race, etc)
- feature engineering
    - feature construction
        - raw data -> features
        - how to combine different types of data (texts, numbers, images)?
        - different types of data: numerical, categorical, text, image, audio, video
        - embedding (PCA, word2vec, zip code encoding, train embedding layer as part of the neural network)
    - feature selection
        - filter method (univariant or multivariant, based on statistical correlation or variance)
        - wrapper method (train a new model for each subset and use the error rate of the model on a hold-out set to evaluate the feature subsets)
            - forward selection
            - backward elimination
            - combination of forward selection and backward elimination
            - recursive feature elimination
        - embeded method
            - tree-based algorithm
            - regularization (L1, L2)
- modeling
    - model selection: supervised or unsupervised? regression or classification?

    An income prediction task can be regression if we output raw numbers, but if we quantize the income into different brackets and predict the bracket, it becomes a classification problem. Similarly, you can use **unsupervised learning** to learn labels for your data, then use those labels for supervised learning. 
    Then frame the question into a specific task: object recognition, text classification, time-series analysis, recommender system, dimensionality reduction etc.
    Keep in mind that there are many ways to frame a problem, and you might not know which way works better until you've tried to train some models.

    - **Start with the simplest solution that can do the job**. Simplicity serves two purposes. First, gradually adding more complex components makes it easier to debug step by step. Second, the simplest model serves as a baseline to which you can compare your more complex models.

    - select baseline model
    Setting up an appropriate baseline is an important step that many candidates forget. There are three different baselines that you should think about: 
        - Random baseline: if your model just predicts everything at random, what's the expected performance? 
        - Human baseline: how well would humans perform on this task? - Simple heuristic: for example, for the task of recommending the app to use next on your phone, the simplest model would be to **recommend your most frequently used app**. If this simple heuristic can predict the next app accurately 70% of the time, any model you build has to outperform it significantly to justify the added complexity. 

    - training the model
        - train/val/test set
            - pay attention to time-series data and imbalanced data
            - avoid data leaking
        - **loss function for Logistic regression，SVM**
        - evaluation metrics
            - classification: accuracy, AUC, recall/precision/F1,logloss, confusion matrix
            - regression: MAE, RMSE
            - ranking: MAP(mean average precision), NDCG 
        - cross-validation vs train/test split (both evaluate the model in the training stage, the former is better)
        - bias-varias tradeoff
        - overfitting/underfitting
        - regularization (L1, L2)
        - optimization (gradient descent) 
            - batch gradient descent (all)
            - mini-batch gradient descent (some, most popular)
            - stochastic gradient descent (one)
        - activation function of neural network (i.e., dead neuron)        
        - Bayes Theorem
        - bagging/boosting
        - collaborative filtering
        - PCA(Statistical method that uses an **orthogonal transformation** to convert a set of observations of correlated variables into a set of values of **linearly uncorrelated variables** called principal components)
        - dimensionality reduction (PCA, LDA, partial least squares, t-SNE)
        - **implement algorithm -- decision tree, K-means**
        - pros and cons of XGBoost/LightGBM (theoretically(loss function) and practically)

    - hyperparameter tuning
        - random search, grid search, Bayesian Optimization
        - Not all hyperparameters are created equal.
    
    - debug, mostly [neural network](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf)
        - Possible problems:
            - Theoretical constraints: e.g. wrong assumptions, poor model/data fit. 
            - Poor model implementation: the more components a model has, the more things that can go wrong, and the harder it is to figure out which goes wrong. 
            - Snobby training techniques: e.g. call model.train()instead of model.eval() during evaluation. 
            - Poor choice of hyperparameters: with the same implementation, a set of hyperparameters can give you the state-of-the-art result but another set of hyperparameters might never converge. 
            - **Data problems**: mismatched inputs/labels, over-preprocessed data, noisy data, etc.
        - Debug method
        - start simple and gradually add more components (esp for neural network)
        - overfit a single batch
        - fix a random seed to guarantee reproducibility
    
    - Feature Importance 
        - partial dependency plot (how the target variable (y) change with the dependent variable(x) giving all other dependent variable fixed; y value represents the average prediction if we force all data points to assume that feature value (x).)
        - SHAP values (average of marginal contributions over all permutations; both accurate and consistent. impurity-based (gain) method is biased towards lower splits and high cardinality features)

    - scaling
        - train on multiple machines (CPUs, GPUs, TPUs)
        - data parallelism: split your data on multiple machines, train your model on all of them, and accumulate gradients. 
        - reduce the precision during training (instead of using a full 32 bits to represent a floating point number, use 16 bits)

- serving

    In production, serving rather than training is the focus.
    Some state-of-art algorithms are most times too expensive to compute (computation cost is an important factor to consider in production, most problems don't need fancy algorithms)

    - how often do you update/retrain model?
    - what to do in case of low confidence?
    - where to run inferencing, on the user device (hard to collect user feedback) or on the server (increases product latency)?
    - interpretability of the model
    - potential biases and misuses of your model. Does it propagate any gender and racial biases from the data, and if so, how will you fix it? What happens if someone with malicious intent has access to your system? 
    - monitoring data drift
        - drift type:
            - covariate shift: distribution of the input features change while no change in the underlying relationship
            - prior probability shift (Distribution of y shift)
            - concept shift (Internal relationship between x and y changed)
            - upstream data change
        - how to detect data drift
            - basic statistics (mean/mode, std, min/max)
            - statistical distance (population stability index, k-L divergence, K-S statistic, histogram intersection)
            - discriminative Distance (Train a model to classify whether the data is from train or test. The better the classifier is, the more different the train and test data is.)
        - what to do if there is data drift
            - remove drifting feature
            - importance reweighting -- upweight training instances similar to the test instances
            - retrain model using more recent data

# Common ML systems
- Recommender System
- Ads ranking


这里是一些我准备过程中看过的不错的资源：

1. http://blog.gainlo.co

2. http://horicky.blogspot.com

3. https://www.hiredintech.com/classrooms/system-design/lesson/52

4. http://www.lecloud.net/tagged/scalability

5. http://tutorials.jenkov.com/software-architecture/index.html

6. http://highscalability.com/





youtube有很多视频，一些是在conference上面的，一些在bootcamp上面的
medium网站有很多RS方向的文章，可以搜一下netflix recommender system challenge从这里入门读几篇paper

1. matrix factorization
2. factorization machine
3. recommender system with implicit feedback
4. wide and deep learning

每一篇paper读完以后去medium搜一下导读(会引出很多intuitive问题)和python的implementation(帮你理解算法)


https://medium.com/the-graph/applying-deep-learning-to-related-pins-a6fee3c92f5e。 
现在为止，面的两家pinterest和facebook都是围绕着推荐系统展开的，中间会问到各种小问题，比如
feature提取，model不够好怎么办

Deep Neural Networks for YouTube Recommendations
《美团机器学习实践》
system design 推荐阅读：
https://medium.com/facebook-design/live-for-mentions-cb91b8a59a27
https://github.blog/2018-10-30-oct21-post-incident-analysis/
https://medium.com/netflix-techb ... liency-c47719f6685b

ml推荐阅读：
https://static.googleusercontent ... s/archive/45530.pdf
https://ai.googleblog.com/2016/0 ... -together-with.html
http://quinonero.net/Publications/predicting-clicks-facebook.pdf
https://www.cda.cn/uploadfile/im ... 220115436_46293.pdf


跟你项目用了哪些技术栈比较有关系，我觉得对于MLE，ETL是必须的估计你也做过，
除此以外，下面几个工具/概念如果用上了会很亮眼
1. Production code deployment
(1) microservice: docker/kubernetes, tensorflow serving
(2) automated deployment: Airflow
2. Continuous integration: Jenkins, TeamCity
3. System design/Class design
4. Profiling(Speed and memory)

网上找10个system design高频题视频差不多了
ml design不会考system design的东西

his blog post: thorough intro to distributed systems
https://hackernoon.com/a-thorough-introduction-to-distributed-systems-3b91562c9b3c
And this
https://www.youtube.com/watch?v=BkSdD5VtyRM

system design: 虽然这个是准备面试用的，但是作为大致入门也是差不多了
Grokking system design interview (for brushing up fundamentals and case studies)

DDIA 堪称经典 Book Designing Data Intensive Applications : for an in-depth look, refer back to fundamental knowledge in OS