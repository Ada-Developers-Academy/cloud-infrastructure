# Generative AI vs. Traditional Machine Learning

## Learning Goals

- Differentiate between traditional machine learning, deep learning, and generative AI.
- Understand the role of foundation models and how large language models process text using tokens and vectors.
- Describe how diffusion models and multimodal architectures expand generative AI beyond simple text.
- Compare the different methods for optimizing pre-trained models, including prompt engineering, retrieval-augmented generation (RAG), and fine-tuning.

## Vocabulary and Synonyms

| Vocab | Definition | Synonyms | How to Use in a Sentence |
| --------- | --------- | -------- | --------- |
| **Deep Learning** | A subset of machine learning that uses layered artificial neural networks to identify patterns and process complex inputs. | Neural networks, DL | We use deep learning to process live video feeds and recognize individual objects within the frame. |
| **Foundation Model** | A massive AI model pre-trained on internet-scale, unlabeled data that can be adapted for a wide variety of general tasks. | FM, base model | Rather than building an algorithm from scratch, we can adopt a foundation model to handle our application's text summarization and translation needs. |
| **Token** | The basic unit of text that a large language model processes, which can be an entire word, a phrase, or just a single character. | Text unit, text segment | The algorithm breaks our sentence down into individual tokens before converting them into numbers. |
| **Vector** | A numerical representation (a list of numbers) assigned to a token that captures its meaning and relationship to other tokens. | Embedding, numerical array | By analyzing the vectors, the model understands that "cat" and "feline" are closely related concepts. |
| **Fine-Tuning** | A supervised learning process that involves modifying a pre-trained model's internal weights by training it further on a specific, smaller dataset. | Model training, weight adjustment | We apply fine-tuning using thousands of medical journals to make our model highly accurate at understanding clinical terminology. |

## From Machine Learning to Generative AI

Traditional Machine Learning (ML) uses algorithms to find patterns and make predictions from historical data. Recall our frequent example of a financial application using traditional ML to analyze numerical purchasing data and classify whether a credit card transaction is genuine or fraudulent.

**Deep learning (DL)** is a specialized subset of ML that uses layered artificial *neural networks* to process highly complex inputs. Neural networks are inspired by the structure of the human brain, with interconnected nodes—called *perceptrons*—that can learn to recognize intricate patterns in data. By using multiple layers of these nodes, deep learning models can capture hierarchical relationships and abstract features from raw data, enabling them to perform tasks like image recognition, natural language processing, and even playing complex games. We might use deep learning for a computer vision application that recognizes objects in photographs so that we can find them later without needing to label them manually.

**Generative AI** is a further subset of deep learning. While traditional DL models are typically used to classify data or predict outcomes, Generative AI models are capable of generating entirely new data based on the patterns and structures they learned from their training data. They accomplish this by adapting deep learning models without needing to be completely retrained from scratch. For instance, instead of merely recognizing a stop sign in a photo, a generative AI model can generate a brand-new image of a stop sign on a city street that has never existed.

```
_Alt. A diagram consisting of four concentric circles. The outermost circle is Artificial intelligence, the next inner circle is Machine learning, the next inner circle is Deep learning, and the innermost circle is Generative AI._
*Fig. Generative AI is a specialized subset of deep learning, which itself is a subset of machine learning and artificial intelligence.*
```

## Foundation Models and Large Language Models (LLMs)

Building individual machine learning models from scratch for every new feature would be a monumental task, with each requiring its own training data, computational resources, and development time. Instead, we can leverage **foundation models**—massive AI models pre-trained on internet-scale, unlabeled data using self-supervised learning. Unlike traditional ML, which requires gathering explicitly labeled data to train a specific model for a specific task, a single FM can be adapted to perform a broad range of general tasks. These tasks include text summarization, information extraction, and chatbot interactions. Major cloud providers offer managed access to these powerful foundation models, with each vendor providing their own unique models and capabilities.

Large language models (LLMs) are a specific type of foundation model that often utilize an architectural approach called a *transformer* to respond to and generate human-like text. They process text by breaking it down into basic units called tokens, which can be whole words, phrases, or individual characters. The model assigns these tokens numerical representations called *embeddings*—the same vectors, or lists of numerical values discussed with text feature engineering—that capture the nuanced semantic relationships and context between different concepts. Again, tokens that are semantically related, like "cat", "feline", and "kitten," will have similar vector representations, allowing the model to take their relationship into account to generate coherent responses.

```
_Alt. A graph showing data points plotted on axes. The dots for 'Cat', 'Feline', and 'Kitten' are clustered closely together in one area, while dots for 'Dog' and 'Puppy' are clustered in another area._
*Fig. Vectors map tokens into a numerical space, placing semantically related concepts close together.*
```

## Beyond Text: Diffusion and Multimodal Architectures

Modern cloud systems must be able to process and generate not just text, but also images, audio, and video. While transformer architectures provide powerful capabilities for text-based tasks, generative AI has evolved to include specialized architectures like **diffusion models** and **multimodal models** that can handle a wider variety of data formats.

**Diffusion models** are a deep learning architecture commonly used for image generation. These models learn through a two-step process. First, during forward diffusion, the model takes an input image and gradually adds random noise until the original image is completely unrecognizable. Then, during reverse diffusion, the system learns how to denoise the data, eventually allowing it to generate a clear, coherent new image starting from pure random noise.

**Multimodal models** are designed to process and generate multiple modes of data simultaneously, rather than relying on a single input type like just text or just images. By understanding how different modalities connect and influence each other, these systems can perform incredibly complex tasks. For a real-world application, a multimodal model could analyze an uploaded photograph of a broken machine part alongside a text-based question, and then generate both a text description of the repair and a new graphic illustrating the solution.

## Optimizing Models Without Full Retraining

Training a massive foundation model from scratch requires tremendous amounts of computing power, storage, and energy. To build highly efficient, cost-effective systems that reduce environmental impact while still protecting our proprietary data, we rely on optimization techniques that adapt pre-trained models to our specific business use cases without rebuilding them entirely.

There are three primary ways we can optimize foundation models:

- **Prompt Engineering**: Prompt engineering focuses on carefully developing and designing the instructions, context, and input data we provide to the model to guide its behavior. While the latest models tend not to be as dependent on techniques like assigning roles or using few-shot examples, prompt engineering and the immediacy of its context is still central to how we interact with and optimize generative AI systems to perform specific tasks.

- **Retrieval-Augmented Generation (RAG)**: RAG is a technique that dynamically retrieves domain-relevant documents and supplies them to the model as external context to answer a user's prompt. For instance, an internal corporate chatbot can use RAG to fetch a company's specific health insurance policy document to accurately answer an employee's question. RAG improves output accuracy by "grounding" the model with up-to-date, relevant information without needing to alter the underlying weights of the foundation model.

- **Fine-Tuning**: Fine-tuning is a supervised learning process that involves training a pre-trained model further on a specific, smaller dataset. Unlike RAG or prompt engineering, fine-tuning actively modifies the model's internal data weights to better align with a specialized task. We could fine-tune a general model using thousands of legal briefs so that it becomes highly proficient at drafting new legal documents, since the patterns and language used would become more generally tuned to the legal domain. Even in this situation, if we needed more certainty that our model was drawing on specific documents to carry out a particular task, we could still employ RAG to provide those documents as context for grounding, while the fine tuning would help ensure that the "voice" and style of the output remained more aligned with the legal domain.

Of these three optimization techniques, prompt engineering is the fastest and lowest-cost approach, while fine-tuning is the most expensive and time-consuming. RAG falls somewhere in between, as it does not require modifying the model's weights but still provides a powerful way to enhance accuracy by supplying relevant context.

## Summary

**Generative AI** represents a powerful evolution in cloud technology. While traditional machine learning identifies patterns to make predictions, and **deep learning** processes complex inputs using neural networks, generative AI adapts these structures to create entirely new data. At the heart of this technology are **foundation models** and **large language models**, which use self-supervised learning to process internet-scale data into mathematical tokens and vectors. These architectures have expanded well beyond text, utilizing **diffusion processes** for image generation and **multimodal models** to connect visual, audio, and text inputs simultaneously. Because training these massive models is highly resource-intensive, we leverage optimization techniques like **prompt engineering**, **retrieval-augmented generation (RAG)**, and **fine-tuning** to securely and efficiently tailor AI to our specific applications.

## Check for Understanding

**1. Which of the following best describes how a Large Language Model (LLM) understands the relationship between words like "car" and "automobile"?**
A) By searching a built-in dictionary file during every request.
B) By breaking text down into tokens and analyzing their numerical vector embeddings, which place related concepts close together mathematically.
C) By using forward diffusion to add noise to the text.
D) By relying entirely on prompt engineering to define every word.

**2. What is the primary difference between Retrieval-Augmented Generation (RAG) and Fine-Tuning?**
A) RAG requires massive amounts of unlabeled internet data, while Fine-Tuning requires no data at all.
B) RAG alters the underlying weights of the foundation model, while Fine-Tuning only changes the user's prompt.
C) RAG dynamically supplies domain-relevant documents as context without changing model weights, while Fine-Tuning actively modifies the model's internal weights using a specific dataset.
D) There is no difference; RAG and Fine-Tuning are different terms for the exact same process.

**3. Which of the following statements accurately describe Multimodal models? (Choose all that apply)**
A) They rely on a single input type, such as only text or only images.
B) They are designed to process and generate multiple modes of data simultaneously.
C) They learn how different modalities, like images and text, are connected and can influence each other.
D) They are the fastest and lowest-cost optimization approach for text generation.
