# Introduction to Machine Learning Problems and Solutions

## Learning Goals

- Define Artificial Intelligence (AI) and Machine Learning (ML), and distinguish them from classical programming and Business Intelligence (BI).
- Identify the core role of machine learning in recognizing complex patterns at scale.
- Describe the primary models of learning to understand how machine learning systems extract patterns from data.
- Categorize the broad types of business problems ML can solve, including classification, regression, clustering, anomaly detection, and recommendation.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Artificial Intelligence** | A system capable of ingesting knowledge to automate tasks that typically require human intelligence. | AI, simulated intelligence | We can implement artificial intelligence to automate the processing of complex customer support tickets. |
| **Machine Learning** | A subset of AI that uses algorithms and statistical models to continuously improve training models and output predictions without being explicitly programmed. | ML, algorithmic learning | By applying machine learning, our application can learn from past purchasing data to predict what a user might buy next. |
| **Rule-based Systems** | AI systems that operate based on explicitly programmed rules rather than learning from data. | Deterministic systems | Our legacy system is a rule-based system that requires manual updates to its decision logic whenever business rules change. |
| **Expert Systems** | A type of rule-based system that can perform simple inference based on a predefined set of rules and knowledge, synthesizing conclusions or determining requirements. | Knowledge-based systems, inference engines | The medical diagnosis tool is an expert system that uses a set of predefined rules to suggest possible conditions based on symptoms. |
| **Classification** | Predicting discrete labels or categories. | Categorization, labeling | Our fraud detection system relies on classification to flag transactions as either legitimate or suspicious. |
| **Regression** | Forecasting continuous numerical values based on past inputs and their resulting outputs. | Forecasting, numerical prediction | We use regression models to predict future housing prices based on square footage and location. |
| **Clustering** | Finding inherent structures and grouping similar unlabeled examples. | Grouping, segmentation | The marketing team used clustering to identify unique segments within our user base. |

## Artificial Intelligence and Machine Learning

In the current era of AI features being added into every application, it's understandable that many people might conflate the terms "Artificial Intelligence" and "Machine Learning." They're absolutely related, but they are not the same thing.

**Machine Learning** is a specific subset of Artificial Intelligence. While AI broadly refers to any system capable of automating tasks that typically require human intelligence, machine learning is focused on using algorithms and statistical models to find patterns in data and improve predictions over time without explicit programming. That is, machine learning takes in large amounts of data, and performs huge amounts of mathematical calculations to find patterns in that data, and then uses those patterns to make predictions on new, unseen data.

But machine learning is far from the only form of AI. There are many other approaches to simulating intelligence, such as *rule-based systems* or *expert systems*, which do not necessarily involve machine learning. However, machine learning has become the dominant approach in recent years due to its ability to handle large datasets and complex patterns that are difficult for humans to recognize or explicitly program.

It's machine learning's capability of simulating intelligent behavior without explicitly programming the rules of that behavior which makes machine learning so powerful. When we build traditional applications, we rely on classical programming, where we consider some set of data and apply human-authored rules to carry out some behavior, or we can say more generally, to produce some output. Machine learning completely flips this paradigm. Instead of hand-coding the rules, we provide the system with historical data and the historical outputs. The machine learning algorithm ingests this information and autonomously generates the rules needed to make predictions on entirely new, unseen data.

Typically, these rules are not simple lines of code that a human can easily read and understand. Instead, they are a representation of complex mathematical relationships that exist within the data. Machine learning is powerful because it can find patterns and relationships in data that are far too complex for humans to explicitly program. The theoretical foundation behind how there relationships are extracted is beyond the scope of this lesson. The mathematical and statistical techniques used to train machine learning models are sound, though the resulting models are difficult to interpret. However, the benefits of the practical applications of machine learning are considered by proponents to outweigh the challenges of interpretability, and thus machine learning has become the dominant approach to AI in recent years.

<!-- available callout types: info, success, warning, danger, secondary, star  -->
### !callout-info

## Machine Learning and Business Intelligence

Business Intelligence (BI) also consumes large amounts of data in order to produce its output, but it is a very different approach to data analysis than machine learning. The primary goal of BI is to perform analytics on historical business data to understand what has happened in the past (descriptive analytics) and why it happened (diagnostic analytics) with an eye towards helping business leaders make informed decisions going forward. Machine learning, on the other hand, is focused on using existing data to find patterns in order to produce outputs for as yet unseen data, and across many more question areas than solely business-related questions.

Modern BI may also incorporate some machine learning techniques to try to make predictions (predictive analytics) or to develop strategies for future actions (prescriptive analytics), but the core of BI is still focused on analyzing historical business data to make business decisions.

### !end-callout

## Extracting Patterns from Massive Datasets

To appreciate why we use machine learning in modern cloud systems, we must recognize that traditional, human-authored rules fail when datasets become incredibly large or involve highly complex variables. At its foundation, *machine learning is the application of advanced mathematics and statistics to discover complex patterns within large datasets*. When a business generates millions of daily transactions, it is impossible for a human developer to write enough `if/else` statements to accurately catch every instance of fraud. They wouldn't even know what to look for to figure out what statements to write. Machine learning models can automatically extract these patterns and structures from the raw data. 

Instead, we can give that transaction data, along with information about which transactions were fraudulent and which were legitimate, to a machine learning system. The system analyzes the data, applying mathematical techniques that tease out the underlying patterns that distinguish fraudulent transactions from legitimate ones in a process called **training**.

Once the model has been trained on this historical data, new transactions can be evaluated using the patterns the model has learned in a process called **inference**. In this capacity, the model acts as an automated decision-maker, evaluating entirely new data points to make accurate decisions or predictions without any human intervention, in this case classifying new transactions as either "fraudulent" or "legitimate" based on the patterns it has learned from the historical data. This ability to generalize from past data to new, unseen data is what makes machine learning so powerful and widely applicable across various industries and use cases.

## Models of Learning

While every machine learning system must undergo a training process to learn from data, there are different approaches to how that learning occurs. The three primary models of learning are supervised learning, unsupervised learning, and reinforcement learning. Each of these models represents a different way for the machine learning system to extract patterns and make predictions based on the data it is given.

### Supervised Learning

In **supervised learning**, the model is trained on a labeled dataset, where each data point is associated with a known output. The model learns to map inputs to outputs based on this training data. For example, a supervised learning model could be trained on a dataset of emails labeled as "spam" or "not spam" to learn how to classify new emails. This is the learning model used in the fraud detection example mentioned earlier, where the model learns to classify transactions as "fraudulent" or "legitimate" based on historical data with known labels.

In addition to the classification tasks mentioned above, supervised learning can also be used for regression tasks, where the output is a continuous value rather than a discrete label. For example, a supervised learning model could be trained on historical housing data to predict future sale prices based on features like square footage and location.

### Unsupervised Learning

In **unsupervised learning**, the model is trained on an unlabeled dataset, where the goal is to find hidden patterns or structures in the data. For example, an unsupervised learning model could be used to cluster customers into different segments based on their purchasing behavior without any predefined labels. In this case, the model cannot ascribe specific meanings to the clusters it creates, but it can still identify groups of similar customers based on the patterns in their behavior. Human experts can then analyze these clusters to determine what they represent. 

> In some cases, this clustering itself is a useful output, while in other cases, the clustering data might be a transitional step. Once we have clusterings, those can be used as features in a supervised learning model to make predictions about new data points.

In addition to clustering, unsupervised learning can also be used for anomaly detection, where the model identifies data points that deviate significantly from the normal patterns in the data. For example, an unsupervised learning model could be used to detect unusual sensor readings in industrial equipment that might indicate a potential failure.

### Reinforcement Learning

In **reinforcement learning**, the model learns to make decisions by interacting with an environment and receiving feedback in the form of rewards or penalties through a "reward function". The model learns to take actions that maximize cumulative rewards over time. For example, a reinforcement learning model could be used to train a robot to navigate a maze by rewarding it for reaching the exit and penalizing it for colliding with walls.

Supervised learning can be considered a form of reinforcement learning where the reward is provided in the form of correct labels. But in more general reinforcement learning the reward function can be more complex and is not necessarily tied to a specific label. How a reward function is defined can have a significant impact on the behavior of the model, and designing effective reward functions is an important area of research in reinforcement learning.

Not all machine learning models fit neatly into one of these categories, and there are many variations and combinations of these learning models. However, understanding these three primary models provides a foundational framework for thinking about how machine learning systems learn from data and make predictions.

## Core Categories of Machine Learning Solutions

At its core, machine learning is about finding patterns in data and making predictions. Some applications of machine learning are focused on predicting specific outcomes, while others are more focused on discovering inherent structures in the data, and further products blend these two facets of machine learning.

### Predicting Outcomes: Classification, Regression

When our goal is to predict an outcome based on historical examples, we typically use either classification or regression.

- **Classification:** This involves predicting discrete labels or categories. We use classification when the answer is a specific class, such as determining whether an incoming email is "spam" or "not spam," whether an uploaded image contains a "cat" or a "dog," or whether a financial transaction is "fraudulent" or "legitimate." In each of these cases, the model is trained to assign a specific category to new data points based on the patterns it has learned from the historical data.

- **Regression:** This involves forecasting continuous numerical values. We use regression when the answer is a number, such as predicting the future sale price of a home based on its location and square footage, or forecasting the specific volume of product demand for the upcoming quarter.

![Two scatter plots side-by-side: one labeled Classification that shows groups of points separated by a diagonal line into two distinct categories with an icon of a dog on one group and an icon of a cat on the other which depicts "Predicting discrete labels or categories". The other labeled Regression that shows a continuous trend line drawn through a series of points representing a function fit to the data points with an icon representing house prices illustrating "Forecasting continuous numerical values".](./assets/ml-classification-regression.png)  
*Fig. Visualizing the difference between classification (predicting categories) and regression (predicting continuous numbers).*

### Finding Patterns: Clustering, Anomaly Detection

When we need to find inherent patterns or prioritize information, we look toward clustering, anomaly detection, and recommendation systems.

- **Clustering:** This involves finding inherent structures and grouping similar unlabeled examples. A common real-world application is customer marketing segmentation, where the system groups users with similar behaviors without being explicitly told what the groups represent beforehand.

- **Anomaly Detection:** This involves finding extreme outliers or novel behaviors that deviate significantly from normal patterns. This is crucial for tasks like identifying highly unusual or suspicious website traffic or detecting novel forms of fraudulent behavior that haven't been seen before. One way to think about anomaly detection is that it is a form of clustering where we are specifically looking for data points that do not fit well into any cluster, indicating that they are anomalous.

### Blending Prediction and Pattern Recognition

Some machine learning applications blend both prediction and pattern recognition.

- **Ranking/Recommendation:** This involves analyzing past user behavior to determine similarities between users or items and using those similarities to predict what a user might like or find relevant. For example, recommendation engines analyze patterns in user interactions to suggest relevant items, such as promoting related products in an e-commerce store or suggesting a hotel for your next vacation.

## Summary

Modern organizations generate massive amounts of complex data, but raw data alone does not provide business value. By transitioning from classical programming to **Machine Learning**, we can **train** algorithms to act as automated **inference** engines that recognize complex patterns at incredible scale. Methods like **supervised learning**, **unsupervised learning**, and **reinforcement learning** allow us to extract insights from data in different ways, depending on the problem we are trying to solve. 

Whether we are using machine learning for **classification** to detect fraud, **regression** to predict housing prices, or **clustering** to segment our users, machine learning provides the mathematical foundation to predict future outcomes. Ultimately, our aim is to integrate machine learning approaches into applications to both improve the user experience and drive business growth.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 545a7156-dec9-4fb3-a1e4-521185eb25c6
* title: Introduction to Machine Learning Problems and Solutions

##### !question

How does machine learning differ from classical programming?


##### !end-question

##### !options

a| Machine learning uses static, hand-coded rules to process data, while classical programming relies on artificial intelligence.
b| Machine learning only works with numerical data, whereas classical programming works with both text and numbers.
c| Machine learning ingests historical data and outputs to autonomously generate rules, whereas classical programming applies human-authored rules to data to generate an output.
d| Machine learning relies strictly on Business Intelligence tools to function.

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

Machine learning differs from classical programming in that it does not rely on human-authored rules to process data. Instead, machine learning algorithms ingest historical data and the corresponding outputs to autonomously generate the rules needed to make predictions on new, unseen data. In contrast, classical programming requires developers to write explicit rules and logic to process data and produce outputs. This fundamental difference allows machine learning to find complex patterns in large datasets that would be difficult or impossible for humans to explicitly program.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: c7efba2c-8597-4b28-b521-35ee628d45a2
* title: Introduction to Machine Learning Problems and Solutions

##### !question

Which of the following problems would be best solved using a Classification model?

##### !end-question

##### !options

a| Predicting the future price of a stock.
b| Flagging an incoming email as "Spam" or "Not Spam".
c| Determining if an uploaded image contains a "Cat" or a "Dog".
d| Grouping users into undefined marketing segments based on browsing history.

##### !end-options

##### !answer

b|
c|

##### !end-answer

##### !explanation

Classification models are used to predict discrete labels or categories. In this case, flagging an incoming email as "Spam" or "Not Spam" and determining if an uploaded image contains a "Cat" or a "Dog" are both examples of classification problems because they involve assigning data points to specific categories.

<br>

Predicting the future price of a stock is a regression problem because it involves forecasting a continuous numerical value, while grouping users into undefined marketing segments is a clustering problem because it involves finding inherent structures in the data without predefined labels.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 79225315-75a3-4299-b815-5d891d8283c6
* title: Introduction to Machine Learning Problems and Solutions

##### !question

A real estate application wants to predict the future sale price of homes in a specific neighborhood based on square footage and the number of bedrooms. Which type of machine learning solution is most appropriate?

##### !end-question

##### !options

a| Regression
b| Anomaly Detection
c| Classification
d| Clustering

##### !end-options

##### !answer

a|

##### !end-answer

##### !explanation

Predicting the future sale price of homes based on features like square footage and the number of bedrooms is a regression problem because it involves forecasting a continuous numerical value.

<br>

Anomaly detection is used for identifying outliers in data, classification is used for predicting discrete categories, and clustering is used for finding inherent structures in unlabeled data. None of these other approaches would be appropriate for predicting a continuous value like home prices.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
