# Problem Set: Machine Learning and Intelligent Systems

## Scenario: Building a Community Health Platform

Our team is developing a community health platform called "HealthHive". It will provide personalized health insights, appointment scheduling, and a chatbot for answering patient questions. We want to leverage machine learning to enhance our platform's capabilities while ensuring ethical considerations are met.

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 746030ce-be50-41cb-9919-912383cb071d
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

Our team understands that folks are busy, and their time is valuable. We want to build a feature that forecasts the exact number of minutes a patient will wait at a clinic to help them plan their day better. We have historical data like time of day, staff availability, and current patient volume. Which type of machine learning solution is most appropriate for predicting this value?

##### !end-question

##### !options

a| Classification
b| Anomaly Detection
c| Regression
d| Clustering

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

Wait time is a continuous numerical value, so **regression** is the appropriate machine learning technique for predicting it. Regression models can analyze the relationships between our input features (like time of day and staff availability) and the continuous target variable (wait time) to make accurate predictions.

<br>

Classification is used for categorical outcomes, anomaly detection identifies outliers, and clustering groups similar data points without predefined labels. 

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: bbde6eeb-cd48-4aed-96a8-3ecdfa604e4b
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

As we gather historical patient records for our model, we notice that certain local demographics are drastically underrepresented in our dataset. We want to ensure our resulting system doesn't lose detail for these groups or produce biased outputs. Which of the following techniques is best suited to address this issue?

##### !end-question

##### !options

a| Random Undersampling
b| Active Learning
c| Random Oversampling
d| SMOTE

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

The scenario we have encountered is a common issue in machine learning called class imbalance, where certain groups or classes are underrepresented in the training data. **SMOTE (Synthetic Minority Over-sampling Technique)** is a powerful method to address this problem by generating synthetic examples for the minority class, thus balancing the dataset without losing valuable information from the majority class.

<br>

Random undersampling would remove data from the majority class, which could lead to loss of important information. Random oversampling simply duplicates existing minority class examples, which can lead to overfitting. Active learning involves human input for uncertain predictions but does not directly solve class imbalance.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 541a054f-faa1-46bb-a140-12e088ed6947
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

We want to add a feature to our platform that automatically extracts structured data from thousands of scanned PDF patient intake forms. Our team has limited time and we want to avoid managing heavy infrastructure. Which cloud AI service approach is best suited for our needs?

##### !end-question

##### !options

a| Building a Custom Model on a PaaS platform
b| Off-the-Shelf Text & Document APIs
c| Tunable Foundation Models
d| Generative AI Assistants

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

**Off-the-Shelf Text & Document APIs** are ideal for our use case because they provide pre-trained models that can extract structured data from documents without the need for custom model development or infrastructure management. These services are designed to handle tasks like OCR (Optical Character Recognition), allowing us to quickly implement the feature with minimal setup.

<br>

Building a custom model on a PaaS platform would require significant time and resources. Tunable foundation models are more suitable for tasks that require customization with our own data, which may not be necessary for standard document processing. Generative AI Assistants are typically used for interactive applications rather than batch processing of documents.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: a0082cc7-e05e-4274-9860-264f9da44e9d
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

To help staff quickly understand patient histories, our platform uses a Large Language Model (LLM) to summarize doctor's notes. How does the LLM understand that words in the notes like "medication," "prescription," and "treatment" are closely related concepts?
##### !end-question

##### !options

a| By dynamically searching a medical dictionary during every request.
b| By relying entirely on prompt engineering to define every medical term.
c| By breaking text down into tokens and analyzing their numerical vector embeddings.
d| By using forward diffusion to filter the text.

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

Large Language Models (LLMs) understand relationships between words by breaking text down into tokens and analyzing their numerical vector embeddings. These embeddings capture semantic relationships, meaning that related concepts like "medication," "prescription," and "treatment" will be represented as vectors that are close together in the embedding space, allowing the model to recognize their relatedness.

<br>

Searching a medical dictionary dynamically would be inefficient and is not how LLMs typically operate, and also doesn't explain how the LLM would understand the definitions. Prompt engineering can guide the model's behavior but does not define the underlying relationships between words. Forward diffusion is a technique used in image generation, not in understanding text.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: a7438304-cf63-4844-8aa2-ea1558a0bf3a
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

We are adding an internal chatbot so our clinic staff can ask questions about specific, up-to-date health insurance policies. We want the model to accurately reference our internal documents without having to retrain the foundation model's underlying weights. Which optimization technique should we implement?
##### !end-question

##### !options

a| Fine-Tuning
b| Retrieval-Augmented Generation (RAG)
c| Reinforcement Learning from Human Feedback (RLHF)
d| Supervised Learning

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

**Retrieval-Augmented Generation (RAG)** is the best technique for our use case because it allows the model to access and reference specific, up-to-date documents (like our insurance policies) without needing to retrain the entire model. RAG works by retrieving relevant information from an external knowledge base and providing it as context to the model during inference, ensuring accurate and current responses.

<br>

Fine-Tuning would require retraining the model, which is not necessary for our needs. Reinforcement Learning from Human Feedback (RLHF) is more suited for aligning model behavior with human preferences rather than providing specific document references. Supervised Learning is a general training approach and does not specifically address the need for dynamic retrieval of information.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: fca5eb27-a120-4fa1-a367-66eb9d41a47c
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

Our team has developed a complex deep neural network to suggest potential medical diagnoses. It is highly accurate, but its internal decision-making process is completely unexplainable. Because healthcare involves high-stakes decisions, which of the following architectural and ethical strategies should we apply?

##### !end-question

##### !options

a| Recognize the "black box" problem, acknowledging the constant tradeoff between our model's accuracy and its explainability.
b| Discard the deep learning model entirely and use a simple rule-based system, as accuracy is not important in healthcare.
c| Implement Human-in-the-Loop (HITL) workflows so human medical professionals can review and override the automated outputs.
d| Apply data drift techniques to ensure the model makes decisions autonomously.

##### !end-options

##### !answer

a|
c|

##### !end-answer

##### !explanation

Complex models like deep neural networks often suffer from the "black box" problem, where high accuracy comes at the cost of explainability. In high-stakes environments like healthcare, we must balance this tradeoff by implementing Human-in-the-Loop (HITL) workflows, ensuring human operators actively supervise and can override automated decisions to provide ethical oversight.

<br>

Discarding the model entirely would ignore the benefits of its accuracy, and using a simple rule-based system may not provide the necessary insights. Data drift techniques are used for monitoring model performance over time and do not address the need for human oversight in decision-making.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: 6283da27-d021-4e2a-93c5-13a50acc51c7
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

While preparing data for a new model, we realize our dataset includes exact patient ages continuously ranging from 18 to 95. To cleanly simplify computations for our algorithm, we decide to group these continuous ages into distinct buckets (e.g., 18-27, 28-37). What feature engineering technique are we applying?

##### !end-question

##### !options

a| Tokenization
b| One-Hot Encoding
c| Normalization
d| Binning

##### !end-options

##### !answer

d|

##### !end-answer

##### !explanation

**Binning** is a feature engineering technique where a wide, continuous scale of numerical values is grouped into distinct buckets, simplifying the data for the model's computation.

<br>

Tokenization is used for breaking text into smaller units, one-hot encoding is for converting categorical variables into binary vectors, and normalization scales numerical features to a specific range. None of these techniques specifically address the grouping of continuous numerical values into buckets like binning does.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: f0814015-5062-4caa-a0ca-d115a8d797ed
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

We are ready to build a custom machine learning model to help our clinic proactively reach out to patients who might need follow-up appointments. Place the following phases of our machine learning lifecycle in the correct chronological order.

##### !end-question

##### !answer

1. Formulate the problem and define our target
1. Prepare our data by collecting and cleaning it
1. Train the model using our preprocessed data
1. Test the model using a held-out dataset
1. Deploy our model into the production pipeline
1. Monitor and maintain the model for data drift

##### !end-answer

##### !explanation

The structured sequence of the machine learning lifecycle begins with **Formulating the problem**, followed by **Preparing the data**, **Training the model**, **Testing the model**, **Deploying the model**, and finally **Monitoring and maintaining it** to ensure long-term reliability. This order ensures that we build a model that is well-defined, trained on clean data, evaluated for performance, and maintained for ongoing effectiveness.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: e18f5553-d8dd-434a-85c8-52b84f82b6e9
* title: Problem Set: Machine Learning and Intelligent Systems

##### !question

Our patient-facing chatbot is fully deployed and is now actively evaluating new questions from users in real-time to provide immediate responses. Which computing phase is the model currently operating in, and what is our primary optimization focus?

##### !end-question

##### !options

a| Model Training, focused on maximizing raw computational throughput.
b| Model Inference, focused heavily on minimizing latency and ensuring high availability.
c| Model Inference, focused on batch processing historical datasets.
d| Feature Engineering, focused on standardizing inputs.

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

The phase where a deployed model evaluates new, unseen data in production is called **Model Inference**. Because our chatbot operates in real-time, our optimization must focus heavily on **minimizing latency and ensuring high availability** so users get immediate, seamless responses.

<br>

Model Training is the phase where we build and optimize the model, not where it serves real-time predictions. Batch processing is typically used for offline analysis rather than real-time inference. Feature engineering is part of the data preparation process and does not describe the operational phase of the model.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
