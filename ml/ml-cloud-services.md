# Cloud ML Services

## Learning Goals

- Evaluate the tradeoffs between building custom machine learning models and adopting Machine Learning as a Service (MLaaS).
- Understand how full custom machine learning platforms orchestrate model development while managing underlying cloud infrastructure.
- Describe how tunable foundation models and generative AI assistants bridge the gap between custom development and pre-trained APIs.
- Identify key categories of off-the-shelf AI services and how they process text, speech, vision, and enterprise search tasks.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Machine Learning as a Service** | Cloud-based offerings that provide pre-built machine learning tools and APIs, eliminating the need to manage infrastructure. | MLaaS, managed ML | We adopted Machine Learning as a Service to add language translation to our app quickly. |
| **Natural Language Processing** | A branch of AI that enables computers to understand and process human language. | NLP, text analysis | Our customer support chatbot uses natural language processing to understand user queries. |
| **Sentiment Analysis** | A technique in NLP that determines the emotional tone behind a body of text. | Opinion mining, emotion detection | We use sentiment analysis to gauge customer satisfaction from product reviews. |
| **Optical Character Recognition** | The automated process of identifying and extracting text and data from scanned documents or images. | OCR, text extraction | Using optical character recognition, our system automatically pulls structured data from uploaded patient intake forms. |
| **Computer Vision** | A field of AI that enables machines to interpret and understand visual information from the world. | CV, image recognition | Our security system uses computer vision to identify unauthorized individuals in real-time. |
| **Text-to-Speech** | A technology that converts written text into spoken words. | TTS, speech synthesis | We implemented text-to-speech to make our app more accessible to visually impaired users. |
| **Automatic Speech Recognition** | The technology that converts spoken language into text. | ASR, speech-to-text | Our virtual assistant uses automatic speech recognition to transcribe user voice commands. |
| **Chatbot** | A conversational interface that processes natural language input to interact with human users seamlessly. | Conversational agent, virtual rep | We implemented a chatbot to handle routine customer support requests, reducing wait times significantly. |
| **Generative AI Assistant** | An AI-powered companion embedded into software environments to help users generate code, analyze data, or answer questions. | Virtual assistant, AI coding companion | Our developers employ a generative AI assistant to suggest code improvements and debug complex functions. |

## Evaluating Tradeoffs: Custom Models vs. Machine Learning as a Service (MLaaS)

Up to this point, we have focused on the core concepts and techniques of machine learning, from the types of problems various learning solutions can solve, to the lifecycle of building and deploying models, to the specific architectures that power generative AI. All of these concepts are useful for understanding how machine learning works, and essential for building custom models from scratch. However, building and maintaining custom machine learning models is not the only way to integrate AI into our applications.

Cloud providers offer a wide range of managed services that allow us to leverage machine learning without needing to manage the underlying infrastructure or develop complex algorithms ourselves. By understanding the tradeoffs between building custom models and using **Machine Learning as a Service (MLaaS)**, we can make informed architectural decisions that align with our business needs, technical capabilities, and resource constraints.

**Machine Learning as a Service (MLaaS)** provides pre-built models and algorithms that can be rapidly integrated with our projects without requiring a deep background in advanced statistics or data science. This approach is ideal for standard tasks where speed and convenience are prioritized. In contrast, training custom models provides complete ownership and the flexibility to tailor architectures to highly specific datasets and specialized business logic. Somewhere between these two extremes, we can also leverage tunable foundation models and generative AI assistants to build custom applications without the need to train a model from scratch.

If we are building an application to translate user reviews into different languages, an MLaaS API is highly efficient. Pre-built machine learning translation services are widely available. However, if we are building a proprietary algorithmic trading system based on unique financial indicators, a custom model is strictly necessary.

### !callout-info

## Replacing Human Tasks with Machine Learning

In the previous example, we suggested using an MLaaS API to translate user reviews. When thinking about replacing human tasks with machine learning, it is important to consider the ramifications of taking a task that would have been performed and vetted by a human and replacing it with an automated system.

If we were thinking about translating the core elements of our user interface, we might want to consider the risks of using an automated system that may not be able to understand the nuances of language, and perhaps keep that responsibility under human control since the way users experience our service can be significantly affected by the qualities of the verbiage being used. But for the case of providing translations of user written reviews, it would be unlikely that a company would even be able to hire enough human translators to keep up with the volume of reviews. In other words, this is a feature that most likely would not exist at all if not for the use of machine learning. Further, the risk of mistranslation is relatively low. User reviews are often informal and contain typos even before any automated translation, so the potential for harm is minimal.

In this case, the benefits of making user reviews accessible to more people through machine learning argue a strong case for using machine learning. It's important to evaluate the risks and benefits of replacing human tasks with machine learning on a case-by-case basis, considering the potential for harm, the value of the task, and the feasibility of using machine learning to perform it effectively.

### !end-callout

We must also consider cost and infrastructure. Custom models require substantial computational resources, such as specialized Graphics Processing Units (GPUs), and demand significant infrastructure management to train and host safely. MLaaS abstracts away the underlying hardware, relying on a pay-as-you-go model where we only pay for the specific predictions or API calls our application makes.

## Full Custom Machine Learning Platforms (Platform-as-a-Service)

When our business requirements dictate that we must build a custom model, configuring the underlying servers and updating the software libraries manually can introduce security vulnerabilities and operational delays. By adopting a managed Platform-as-a-Service (PaaS) solution, we centralize our data processing in a secure environment and ensure our infrastructure scales safely and automatically.

Cloud providers offer fully integrated machine learning platforms that manage the heavy lifting of the machine learning lifecycle. These platforms provide integrated development environments (IDEs) and orchestration tools that allow software engineers and data scientists to build, train, tune, and deploy custom models at a massive scale. More bespoke model approaches that AI engineers and data scientists investigating can be built using hosted Jupyter notebooks, which provide a familiar environment for data science work built around Python and popular machine learning libraries. These platforms also provide tools for data labeling, feature engineering, tuning various model parameters, and model monitoring, all while relieving organizations of the need to manage the underlying cloud infrastructure.

## Tunable Foundation Models

As we discussed in [Generative AI vs. Traditional Machine Learning](./ml-gen-ai.md), training a custom model from scratch is a resource-intensive process that requires significant time, computational power, and financial investment. However, we can leverage secure, pre-trained models hosted by our cloud providers, allowing us to keep our proprietary corporate data private while benefiting from state-of-the-art technology of the latest foundation models.

Cloud providers offer managed services that provide secure API access to massive, pre-trained foundation models from leading AI organizations. These services represent a powerful middle ground between off-the-shelf APIs and full custom development. Developers can use these platforms to privately customize (fine-tune) models with their own enterprise data, or utilize them in Retrieval-Augmented Generation (RAG) workflows to supply dynamic, domain-relevant context.

## Off-the-Shelf AI Services (Pre-Trained APIs)

Many common business operations—such as reading text from a scanned invoice, transcribing a customer service call, or routing a support request—are universal challenges. By integrating secure, pre-trained APIs into our applications, we avoid reinventing existing solutions and significantly accelerate our software development process.

These off-the-shelf AI services are highly scalable, ready-to-use models that are typically accessible via simple API calls, requiring no underlying machine learning experience to integrate. We can categorize these APIs by their primary functions:

- **Text & Documents:** These natural language processing APIs extract key entities, determine sentiment, translate between languages, or perform intelligent optical character recognition (OCR) to pull structured data from scanned forms and documents.

- **Speech & Vision:** Computer vision APIs are used for identifying objects, people, or inappropriate content in images and video. Speech APIs convert lifelike text-to-speech for accessibility or perform automatic speech recognition to transcribe uploaded audio.

- **Search, Chat, & Recommendations:** This category includes enterprise intelligent search engines that understand natural language queries. It also encompasses conversational interfaces for building advanced customer service chatbots, as well as personalization engines that analyze user activity to generate tailored product recommendations.

- **Generative AI Assistants:** These are pre-built, customizable assistants that integrate directly into software development environments or team messaging platforms to generate code, analyze data, or answer questions based on company documentation.

## Summary

Building intelligent cloud applications requires us to carefully evaluate our business needs and operational constraints. 
- We can choose to integrate rapid, cost-effective **Machine Learning as a Service (MLaaS)** tools, or we can use dedicated **ML platforms** to manage the infrastructure needed for highly specialized custom models. 
- To leverage the power of modern generative AI while minimizing the need for custom development, we can utilize **tunable foundation models** that allow us to securely customize pre-trained models with our own data. 
- Finally, for standardized tasks like object recognition, language translation, chatbot interfaces, and many more, we can utilize off-the-shelf AI services, which provide scalable intelligence through simple API calls. 

There's a service level for every use case, and understanding the tradeoffs between these options is essential for making informed decisions about what services make the most sense for our applications.

## Check for Understanding

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: checkbox
* id: 8b323032-bdff-4565-b8c3-bde92965ee53
* title: Cloud ML Services

##### !question

Which of the following is a primary tradeoff when choosing to build a Custom Machine Learning Model instead of using Machine Learning as a Service (MLaaS)?

##### !end-question

##### !options

a| Custom models require significant computational resources, like GPUs, and heavy infrastructure management.
b| Custom models provide the lowest flexibility and are restricted strictly to pre-defined use cases.
c| Custom models require advanced data science skills and more time to develop, train, and test.
d| Custom models run exclusively on serverless architectures and require no active monitoring.

##### !end-options

##### !answer

a|
c|

##### !end-answer

##### !explanation

Building custom machine learning models requires significant computational resources, such as specialized GPUs, and demands substantial infrastructure management to train and host safely. Additionally, developing custom models requires advanced data science skills and more time to develop, train, and test compared to using pre-built MLaaS solutions.

<br>

Rather than lowering flexibility, custom models provide the highest level of flexibility, allowing us to tailor architectures to highly specific datasets and specialized business logic. Custom models can run on various architectures, including serverless, but they still typically require ongoing monitoring to ensure they perform as expected and to manage any issues that arise.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: db43a47a-6893-4b70-87dc-0430cb509a84
* title: Cloud ML Services

##### !question

Which category of off-the-shelf AI services would be best suited for an application that needs to pull structured data fields from thousands of scanned PDF invoices?

##### !end-question

##### !options

a| Speech & Vision APIs
b| Search, Chat, & Recommendation APIs
c| Text & Document APIs
d| Generative AI Assistants

##### !end-options

##### !answer

c|

##### !end-answer

##### !explanation

Text & Document APIs are designed for natural language processing tasks, including intelligent optical character recognition (OCR) that can extract structured data from scanned forms and documents. This makes them the best choice for an application that needs to pull structured data fields from thousands of scanned PDF invoices.

<br>

Speech & Vision APIs may sound like it could be a good choice, but its vision capabilities are more suited for tasks involving image object recognition rather than text recognition. Search, Chat, & Recommendation APIs are focused on enterprise search and conversational interfaces. Generative AI Assistants are typically used for generating code or analyzing data rather than extracting structured data from documents.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: multiple-choice
* id: c2f77bae-9228-45ae-86b1-4dc9f7cddc83
* title: Cloud ML Services

##### !question

What is the primary role of a Full Custom Machine Learning Platform (Platform-as-a-Service)?

##### !end-question

##### !options

a| To completely eliminate the need for training data by generating all necessary data synthetically.
b| To provide an integrated development environment and orchestration tools that manage the heavy lifting of the ML lifecycle.
c| To serve as an off-the-shelf chatbot that answers customer service queries automatically.
d| To convert text into lifelike speech without requiring any custom coding.

##### !end-options

##### !answer

b|

##### !end-answer

##### !explanation

A Full Custom Machine Learning Platform (Platform-as-a-Service) provides an integrated development environment and orchestration tools that manage the heavy lifting of the machine learning lifecycle. This allows software engineers and data scientists to build, train, tune, and deploy custom models at scale without needing to manage the underlying cloud infrastructure.

<br>

Training data is always required for machine learning models, making that option incorrect. Off-the-shelf chatbots and text-to-speech services are examples of specific AI services, not the primary role of a full custom machine learning platform.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->

<!-- >>>>>>>>>>>>>>>>>>>>>> BEGIN CHALLENGE >>>>>>>>>>>>>>>>>>>>>> -->

### !challenge

* type: ordering
* id: 3c774a7c-57ba-4ee3-89ff-785e8d089f2f
* title: Cloud ML Services

##### !question

Place the following cloud ML approaches in order from the highest level of custom code and infrastructure management required at the top, to the lowest level required at the end.

##### !end-question

##### !answer

1. Full Custom Machine Learning Platforms (PaaS)
1. Tunable Foundation Models
1. Off-the-Shelf AI Services (Pre-Trained APIs)

##### !end-answer

##### !explanation

Full Custom Machine Learning Platforms (PaaS) require the highest level of custom code and infrastructure management, as they allow for building and deploying custom models that may require significant resources and expertise. Tunable Foundation Models offer a middle ground, allowing for customization of pre-trained models with proprietary data while still abstracting away much of the underlying complexity. Off-the-Shelf AI Services (Pre-Trained APIs) require the lowest level of custom code and infrastructure management, as they provide ready-to-use models accessible via simple API calls without needing to manage the underlying machine learning processes.

##### !end-explanation

### !end-challenge

<!-- ======================= END CHALLENGE ======================= -->
