# Blazing Fast Text Embedding Calculation With Word2Vec in Golang to Power  Extensibility of Large Language Models (LLMs)

We are currently witnessing the second wave of adoption for Large Language Models (LLMs) across diverse industrial domains and applications. Numerous stakeholders are seeking ways to leverage these models by extending generic open-source and commercial versions for use with private datasets—where security and privacy are paramount—or for developing niche applications in cases where public dataset availability is restricted. An exemplary framework for constructing such extensions is Retrieval-Augmented Generation (RAG). 

A fundamental requirement in developing such applications is the need for a formal definition of 'nearby things’. This challenge has been effectively addressed by the industry through the use of **embeddings**. Embeddings serve as representations for various data types such as text, images, audio, video, and other high-dimensional objects. They are constructed as low-dimensional vectors comprising real numbers. The characteristics and mathematical operations of these vectors are precisely defined within the framework of linear algebra. Foundational for artificial intelligence (AI), embeddings enable computers to grasp the connections between words and other objects. 

Generating embeddings involves the utilization of neural networks, sophisticated algorithms capable of learning intricate patterns within data. Neural networks play a pivotal role in transforming raw input, such as text, images, audio, or video, into meaningful and compact representations—these representations are the embeddings. By leveraging the power of neural networks, these embeddings capture nuanced relationships and semantic contexts, enabling computers to comprehend and process information with heightened accuracy and efficiency. As a result, the integration of embeddings into artificial intelligence applications becomes a cornerstone, facilitating a deeper understanding of the intricate associations between diverse elements in the data landscape.


# What is the problem worth solving?

Despite the availability of commercial and open-source models, **word2vec** and its derivatives remain prevalent in building niche applications, particularly when dealing with private datasets. Python, along with the 'gensim' library, is widely adopted as the quickest means to explore the model for production-grade workloads. Gensim is optimized for performance through the use of C, BLAS, and memory-mapping. However, if your application demands even greater speed, such as performing 94K embeddings calculations per second on a single core, native development becomes the optimal solution.


# Building Word2Vec in Golang: Optimizing Performance with Native C Integration

Our objective is to create a solution that allows the execution of the Word2Vec model natively within a Golang application, eliminating the need to wrap 'gensim' as a sidecar. While there are limited native Golang implementations of Word2Vec, we found https://github.com/ynqa/wego to be the most promising. However, its training attempt on the UMBC web-based corpus (approximately 3 billion words) proved to be excessively time-consuming, highlighting the need for performance optimizations.

Our focus shifted to leveraging native C code. Initially, we attempted to integrate the original Golang code (https://code.google.com/archive/p/word2vec/) and later explored its Mac OS X patch (https://github.com/William-Yeh/word2vec-mac). While these code bases were functional, the complexity of distilling them into a C library with a clean interface was estimated to require at least a week of effort, and there was uncertainty about the worth of such an investment.

Our final attempt involved the use of the Word2Vec C++ library developed by Max Fomichev (https://github.com/maxoodf/word2vec). This library proved to be well-architected, equipped with a cross-platform build system, and presented a clean C++ interface. Integrating it with Golang merely required the implementation of a C bridge and CGO interface. **As a result, https://github.com/fogfish/word2vec now stands as a Golang port of Max Fomichev's library, providing an efficient and performant solution for Word2Vec in a Golang environment**. 

The library assessment has yielded satisfactory performance results that align with the requirements of our application. The evaluation was conducted using the Apple M2 Pro, and the following notable findings were observed:

**Fact 1**: Training on the UMBC web-based corpus, comprising around 3 billion words, was completed in less than 2 hours.

**Fact 2**: The library demonstrates a remarkable speed, calculating 94K embeddings per second for text snippets of about 444 characters on a single CPU core. This reflects a processing rate of 40 MB/sec.

**Fact 3**: It efficiently processes 17GB of data in 7 minutes and 9 seconds, generating an impressive 40 million embeddings.

**Fact 4**: The linear performance of the library enables the production of approximately 1 billion embeddings in just 3 hours.


# How to use word2vec in Golang

The current version of the project doesn't provide a precompiled binary release for any platforms. To use it, you'll need to build it manually following the instructions in the README at https://github.com/fogfish/word2vec. If you encounter any difficulties or have specific requirements for a binary release, please don't hesitate to open an issue for support. Once you've successfully built the library and Golang's command-line application for your environment, ensure that the libw2v.dylib library is accessible to your dynamic linker. With these steps completed, you're ready to go!

One drawback of this library is the utilization of a proprietary binary format for the model, designed exclusively to enhance performance significantly. Consequently, training the model is a necessary step against a text corpus, whether public or private. Given its impressive speed, training on a corpus of approximately 3 billion words takes less than 2 hours. As an educational resource, Leo Tolstoy's book "War and Peace" is included in the project, serving as an illustrative example to familiarize you with the workflow.

Let's start training.

The front-end Command Line Interface (w2v) utilizes a YAML configuration file to provide parameters for the trainer process. Employ the command to generate the default configuration. The default parameters yield satisfactory results, but you have the flexibility to fine-tune aspects such as stop words, the number of training epochs, the window size for nearby words, and more. For detailed insights into optimal hyperparameters and their impact on downstream natural language processing tasks, you may refer to the article "Word2Vec: Optimal Hyperparameters and Their Impact on Natural Language Processing Downstream Tasks" (https://www.degruyter.com/document/doi/10.1515/comp-2022-0236/html?lang=en).

```bash
w2v train config > wap-en.yaml
```

We recommend utilizing the front-end CLI for model training.

```bash
w2v train -C wap-en.yaml \
  -o wap-v300w5e5s1h005-en.bin \
  -f ../doc/leo-tolstoy-war-and-peace-en.txt
```

The output binary file can be safely distributed to consumers. The following code snippet illustrates how the library can be employed to calculate embeddings.

```go
import "github.com/fogfish/word2vec"

// 1. Load model

w2v, err := word2vec.Load("wap-v300w5e5s1h005-en.bin", 300)

// 2. Allocated the memory for vector

vec := make([]float32, 300)

// 3. Calculate embeddings for the document

err = w2v.Embedding("braunau was the headquarters of the commander-in-chief", vec)
```

# Afterwords

In our quest to seamlessly integrate Word2Vec into Golang, we navigated through challenges, seeking optimal performance and native efficiency. The journey began by identifying the need for a solution where Word2Vec models could be seamlessly executed within Golang applications, eliminating the necessity for additional dependencies. Evaluating existing Golang implementations led us to the promising options. However, performance constraints on the UMBC corpus sparked a pursuit of native C integration.

Transitioning through Golang code iterations and the Word2Vec C++ library by Max Fomichev ([https://github.com/maxoodf/word2vec](https://github.com/maxoodf/word2vec)), we birthed [https://github.com/fogfish/word2vec](https://github.com/fogfish/word2vec) a Golang port embodying exceptional performance. Performance assessments showcased its prowess, training on 3 billion words in under 2 hours and calculating 94K embeddings per second on a single core.

This fusion encapsulates the collaborative spirit of open source, inviting both seasoned developers and curious learners to embark on a journey that blends the power of Word2Vec with the versatility of Golang. The project is still an experimental development in scope of our applications. We are looking for feedback, development ideas and further expansion.

Thank you for joining us on this odyssey. Stay tuned!
