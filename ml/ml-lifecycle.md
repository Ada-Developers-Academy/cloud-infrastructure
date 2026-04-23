# The Machine Learning Project Lifecycle

## Learning Goals

- Understand the fundamental phases of a machine learning project.
- Evaluate data quality and explain why data preparation is the most critical step.
- Describe how feature engineering transforms raw data into formats suitable for machine learning models.
- Differentiate between model training and model inference in terms of purpose and computing requirements.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Training** | The process of feeding data into a machine learning algorithm to create a model that can make predictions. | Learning, fitting | We spent several hours training our model on the historical sales data to improve its accuracy. |
| **Held-out dataset** | A portion of the data that is set aside during training to evaluate the model's performance on unseen data. | Test set, validation set | We used a held-out dataset to test our model's ability to generalize to new customer data. |
| **Imputation** | Filling in missing values in a dataset using statistical means, such as the mean of other rows. | Data filling, substitution | Rather than dropping the incomplete records, we used mean imputation to preserve our dataset size. |
| **Outlier** | A data point that differs significantly from other observations in the dataset. | Anomaly, extreme value | We noticed an extreme outlier in our logs when a sensor briefly malfunctioned. |
| **Feature** | A single measurable property or characteristic in a dataset. | Variable, attribute | The customer's zip code served as a key feature in our predictive model. |
| **Inference** | The process of using a trained model to make predictions or decisions on new, unseen data. | Prediction, evaluation | During inference, our application evaluates real-time user input to provide instant recommendations. |

## Machine Learning Development Lifecycle

When we apply machine learning to solve real-world problems, we follow a structured lifecycle that guides us from understanding the problem to deploying a working model. This lifecycle consists of six key phases: Formulating the Problem, Data Preparation, Model Training, Model Testing, Deployment, and Monitoring and Maintenance.

- **Formulate the Problem**: First, we frame the core problem in terms of what we can observe and what target answer the model needs to predict. The learning model and solution type will depend on the data we have and the question we want to answer. For example, a financial organization can define a problem to classify whether a credit card transaction is genuine or fraudulent.

- **Data Preparation**: Next, we collect, clean, and preprocess our raw data to make it suitable for consumption by an ML algorithm. Major cloud providers offer specific platform tools for this step. This phase is often the most time-consuming and critical, as the quality of our data directly impacts the performance of our model. Biases or errors in the data can lead to inaccurate predictions, so we must ensure our data is clean, representative, and ethically sourced.

- **Model Training**: We then feed the prepared data into a learning algorithm to create a model. During this phase, a portion of the data is intentionally set aside, or held out, so we can use it to validate our work later. Training is a compute-heavy process that often requires specialized hardware, such as GPUs, to handle large datasets and complex models efficiently.

- **Model Testing**: We evaluate the quality and accuracy of the trained model using the held-out dataset. This ensures the model can generalize to new, unseen data, rather than just memorizing the training set.

- **Deployment**: Finally, we integrate the trained model into a production pipeline to generate predictions on live, real-world data.

- **Monitoring and Maintenance**: After deployment, we continuously monitor the model's performance and retrain it with new data as needed. This creates a feedback loop that allows the model to improve over time and adapt to changing conditions.

_Alt. A diagram showing five sequential, interconnected circles representing the machine learning lifecycle: Formulate a problem, Prepare your data, Train the model, Test the model, Deploy your model, Monitor and maintain the model. Each circle is labeled with its respective phase, and arrows indicate the flow from one phase to the next, with a feedback loop including preparing data, training the model, and testing the model to represent the tight, iterative nature of the machine learning lifecycle._  
*Fig. The Iterative Machine Learning Lifecycle*

## The Importance of High-Quality Data

An ML model is only as good as the data used to train it, which leads to the popular phrase "garbage in, garbage out". Taking the time to curate data prevents our systems from making biased or inaccurate decisions, which is vital for building trustworthy applications. Preparing data is arguably the most critical and time-consuming stage in making a model work as intended. 

What makes for "good" data?

### Data Quantity

First, we need to be sure that we have enough data to train a model effectively. In addition to the data used for training, we also need to set aside a portion of the data for testing and validation, often as much as 20-30% of the total dataset. This data that we set aside is called the *held-out* dataset, and it is used to evaluate the model's performance on data the model wasn't specifically trained on.

Depending on the make up of the data, we need to be careful about how we split the data to ensure that the training and held-out datasets are representative of the same underlying distribution. For example, if we are training a model to predict real estate prices, we want to make sure that both the training and held-out datasets contain a similar mix of properties from different neighborhoods, price ranges, and property types. If the data arrived with an order that were not random, such as all the data from one neighborhood followed by all the data from another neighborhood, we would want to shuffle the data before splitting it to ensure that both datasets are representative of the overall distribution.

If there isn't enough data, the training process will not be able to learn meaningful patterns, and the model will perform poorly. Or if we try to run a model on a dataset that is too small, it may overfit, meaning it learns the training data too well and fails to generalize to new data. On the other hand, having too much data can also be problematic if it exceeds our computational resources or if it contains a lot of noise that can mislead the model.

### Data Quality

It's not enough to just have a large quantity of data; the quality of that data is equally important. We evaluate data quality using five key principles: it must be **R**elevant, **R**epresentative, **R**ich, **R**eliable, and **R**esponsible.

- **Relevant**: The data should be directly related to the problem we are trying to solve. Irrelevant data can introduce noise and reduce model performance. If we are building a model to predict customer churn, for example, data about the customer's favorite color would likely be irrelevant and could be excluded from the training dataset.

- **Representative**: The data should accurately reflect the real-world population or scenario we want to model. If the data is biased or unrepresentative, the model's predictions will also be biased and may not perform well on new data. For instance, if we are training a medical diagnosis model but our dataset only includes cases of people from a specific demographic group, the model may perform poorly when applied to a more diverse population.

- **Rich**: The data should contain enough features and information to allow the model to learn meaningful patterns. Sparse or overly simplistic data may not provide enough context for the model to make accurate predictions.

- **Reliable**: The data should be accurate and consistent. Unreliable data can lead to incorrect model training and poor performance. Data that has inconsistent formatting, missing values, or errors can cause the model to learn incorrect patterns. For example, if we have a dataset of customer ages but some entries are recorded as "twenty" instead of "20", this inconsistency can lead to errors during training.

- **Responsible**: The data should be ethically sourced and used in a way that respects privacy and complies with relevant regulations. Ethically sourcing means ensuring that the data was collected with proper consent, and the data must be managed in a way that protects individuals' privacy, which may be governed by law. This can require anonymizing data, implementing strict access controls, and ensuring that data is encrypted both in transit and at rest.

Even in data that is otherwise "good", we often need to perform cleaning and preprocessing to make it suitable for machine learning. This includes handling missing values, removing duplicates, and addressing outliers.

When handling missing or duplicate data, we must decide whether to drop incomplete records or *impute* them, which means filling in missing values using statistical methods.

**Outliers** are data points that differ significantly from other observations. They can be caused by errors in data collection or they may represent legitimate but rare events. Erroneous outliers caused by system bugs should be dropped, while legitimate outliers are kept but may require further processing to prevent them from overly skewing the training results. For example, if a temperature sensor records a momentary glitch reading of 500 degrees, we would drop it, but a legitimately high heatwave temperature would be kept and adjusted numerically using techniques like *standardization* or *binning* to prevent it from dominating the model's learning process.

## From Raw Data to Trainable Features

Machine learning algorithms rely entirely on mathematics to function, meaning they cannot directly read text or categories the way humans do. To ensure that our data is in a format that the algorithm can process, we need to perform **feature** engineering.

A **feature** is a single measurable property or characteristic in a dataset. Because machine learning algorithms require numerical input, raw data must be engineered into formats that preserve the richness of the original data while being mathematically consumable. We employ different techniques to engineer features based on the type of data we have.

| Engineering Technique | Data Type | Description |
| --- | --- | --- |
| Normalization | Numerical | Scaling values to a range between 0 and 1 to improve model performance. |
| Standardization | Numerical | Transforming features relative to the standard deviation to reduce the impact of outliers. |
| Binning | Numerical | Grouping continuous values into discrete buckets to simplify the model's computation. |
| One-hot encoding | Categorical | Converting categories into a binary format where each category is represented by a separate binary column. |
| Tokenization | Text | Breaking text down into units of meaning, such as words or phrases, which can then be embedded as numerical vectors. |

When engineering numerical features, we often use normalization and standardization. Training methods often perform better when features are on a similar scale, so we use *normalization* to scale values to a range between 0 and 1. *Standardization*, on the other hand, transforms features relative to a statistical measure called the standard deviation. Doing so helps to reduce the impact of outliers.

Another helpful technique is *binning*, where we group a wide, continuous scale of numbers into distinct numerical buckets to simplify the model's computation. For example, if our data contains exact customer ages, this could lead to a very large number of unique values that the model would have to learn from. By grouping ages into bins, such as young adults (18-27), adults (28-37), mid-life (38-47), mature (48-57), and senior (58+), we can reduce the number of unique values, and reduce the impact of outliers (folks in the 90+ range). This can make it easier for the model to learn patterns. 

Categorical and other text features also require engineering to be usable by machine learning algorithms. For categorical data, we often use *one-hot encoding* to convert categories into a binary format. One-hot encoding converts categorical data—like the colors Red, Green, or Blue—into separate binary columns. In these new columns, a "1" indicates the presence of the category and a "0" indicates its absence. The name "one-hot" comes from the fact that only one category can be "hot" (1) for each data point, while the others are "cold" (0). This allows the model to process categorical data without assuming any inherent order or relationship between the categories.

For natural language text, we use *tokenization*. This technique breaks text down into chunked units of meaning, such as words or phrases, which can then be embedded as numerical vectors.

### !callout-info

## Numerical Vectors and Learning

In machine learning, numerical vectors allows similar pieces of information to be represented in a way that preserves their relationships. Mathematical vectors are essentially lists of numbers that represent features in a way that captures their meaning and relationships. By starting tokens with a random collection of hundreds or thousands of numbers, the model can learn to adjust these vectors during training so that similar tokens end up with similar vector representations. For example, in a natural language processing model, the words "parent" and "child" might end up with similar vector representations because they often appear in similar contexts. 

While neither the initial values, nor the final vector representations, are directly interpretable by humans, they allow the model to understand and process complex relationships in the data. This is a variation of unsupervised learning called *self-supervised learning*, where the model learns to predict parts of the data from other parts, allowing it to learn meaningful representations without explicit labels. We'll revisit this idea of self-supervised learning in the next module when we discuss large language models.

### !end-callout

## Testing and Deploying the Model

After training, we evaluate the model's performance using the held-out dataset to ensure it can generalize to new, unseen data. This step is crucial to prevent overfitting, where a model performs well on training data but poorly on new data. Crucially, if the model does not perform well on the held-out dataset, we must resist the temptation to use the test data to adjust the model, as this can lead to overfitting. Instead, we should return to the training phase and make adjustments there, such as improving data quality or engineering better features.

Once we are satisfied with the model's performance, we deploy it into production, where it can make predictions on live data.

## Monitoring and Maintenance

After deployment, we continuously monitor the model's performance and retrain it with new data as needed. This helps prevent *model drift*, which occurs when the model's performance degrades over time due to changes in the underlying data distribution. By regularly updating the model with new data, we can ensure that it remains accurate and relevant as conditions evolve.

For example, if we have a model that predicts customer churn, we would monitor its predictions against actual customer behavior and retrain it periodically with new data to ensure it remains accurate as market conditions and customer preferences evolve.

## Compute Needs: Training vs. Inference

Cloud platforms provide a range of computing resources that can be optimized for different phases of the machine learning lifecycle. Understanding the distinction between training and inference is crucial for designing efficient and cost-effective cloud infrastructure.

Model training is a compute-heavy, highly iterative, and memory-intensive process. Training is typically processed in massive batches using historical data. Optimization during this phase is focused entirely on high-speed data access, and maximizing
raw computational throughput, often leveraging specialized hardware accelerators like GPUs. 

Conversely, model inference is the production phase where the trained model evaluates new, unseen data. Inference can be processed in large batches, such as generating nightly forecast reports, or in real-time, such as a chatbot responding instantly to a user. Any singular inference calculation is typically much less computationally intensive than training, but the need for low latency and high availability can make it more expensive if not properly optimized. And performing inference at scale can still require significant computational resources, especially for complex models or high volumes of requests. So while the guide that training is compute-heavy and inference is latency-sensitive is a helpful generalization, the specific compute needs of inference can vary widely based on the application and model complexity.

## Summary

The machine learning lifecycle consists of five key phases: **Formulating the Problem**, **Data Preparation**, **Model Training**, **Model Testing**, and **Deployment**. Each phase is crucial for building effective machine learning models that can solve real-world problems. High-quality data is essential for training accurate models, and **feature** engineering transforms raw data into formats that algorithms can process. Understanding the distinction between **training** and **inference** helps us optimize our cloud infrastructure to support both phases efficiently. By following this structured lifecycle, we can create machine learning applications that drive business growth and provide valuable insights from data.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: 64bb178d-cc89-4707-a783-c903d5576aaf
* title: The Machine Learning Project Lifecycle

##### !question

Place the following phases of a machine learning project in the correct chronological order:

##### !end-question

##### !answer

1. Formulate a problem
1. Prepare your data
1. Train the model
1. Test the model
1. Deploy your model
1. Monitor and maintain the model

##### !end-answer

##### !explanation

The Machine Learning Project Lifecycle follows a structured sequence of phases. We start by **formulating the problem** to define what we want to predict. Next, we **prepare our data** by collecting, cleaning, and engineering features. After that, we **train the model** using the prepared data. Once the model is trained, we **test the model** using a held-out dataset to evaluate its performance. If the model performs well, we **deploy it** into production to make predictions on live data. Finally, we continuously **monitor and maintain the model** to ensure it remains accurate and relevant over time.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: e2258d99-2da7-4d8c-8ea6-b3b550a54fe6
* title: The Machine Learning Project Lifecycle

##### !question

Which of the following principles are used to define "good data"?

##### !end-question

##### !options

a| Representative
b| Reactionary
c| Reliable
d| Redundant
e| Responsible

##### !end-options

##### !answer

a| Representative
c| Reliable
e| Responsible

##### !end-answer

##### !explanation

The 5 R's of good data are: Relevant, Representative, Rich, Reliable, and Responsible. Of those, the options included in this question are Representative, Reliable, and Responsible. **Representative** data accurately reflects the real-world population or scenario we want to model. **Reliable** data is accurate and consistent, while **Responsible** data is ethically sourced and used in a way that respects privacy and complies with relevant regulations.

<br>

The missing qualities of good data are Relevant, which means the data should be directly related to the problem we are trying to solve, and Rich, which means the data should contain enough features and information to allow the model to learn meaningful patterns.

<br>

Reactionary and Redundant are not concepts among the qualities of good data.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: de95c83f-984f-4c81-9f50-2f6788062c3e
* title: The Machine Learning Project Lifecycle

##### !question

When preparing a machine learning model for a real-time chatbot, which computing phase focuses heavily on minimizing latency and ensuring high availability?

##### !end-question

##### !options

a| Model Training
b| Model Inference
c| Tokenization
d| Hyperparameter Tuning

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

Model inference is the phase where the trained model is used to make predictions on new, unseen data. In the context of a real-time chatbot, inference focuses heavily on minimizing latency and ensuring high availability, as the chatbot needs to respond to user inputs instantly.

<br>

Model training, on the other hand, is a compute-heavy process that occurs before deployment and does not require low latency. Tokenization is a technique used in feature engineering to break text into units, and hyperparameter tuning (which was not discussed in the text) is an optimization process during model training, neither of which directly relates to the real-time requirements of inference.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
