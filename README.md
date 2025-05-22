# SIIM Report Juicing Learning Lab
- Nathan M Cross MD MS CIIP
- Yee Ng MD
- Mohammed Kanani MS
#### Acknowledgements
- Brian Chang MD PhD - Colab coding draft
- Arezu Monawer MD - inspiration and data help

Thank you for joining us.  The goal of this lab is to help you understand the role large language models (LLMs) can play as natural language processing (NLP) tools for clinical radiology reports.  We will walk through this brief tutorial to show you how you can quickly get up and running using a collection of open source libraries to do this work.

## Google Colabs vs local
We have adapted this code to work in Google Colabs to make it easy for you to run it without any specialized hardware.  The free version of colabs puts severe restrictions on the size of the model we can use which is going to negatively impact model performance (i.e. we can only install a small model with a small GPU memory footprint, this will not be as performant as a large model). If you would like to use it outside of Google Colabs you will likely need to setup a virtual environment with conda, Python venv, Poetry etc to install all the libraries you need.  You will also need 10-50+Gb of hard drive space to store at least one of the more commonly used language models. You will see size estimates of the models on Ollama's site.

## Local hardware
If you'd like to run this locally you will need to have a few things:
1. **Storage Space** - local LLMs come in all shapes and sizes and you will need to weigh the cost vs benefit of using larger models vs smaller models.  Smaller models, like 2-10billion parameter models generally take up around 2-10Gb. Larger models like Llama4:scout is 67Gb. There are a variety of ways that clever computer scientists have managed to make models smaller without sacrificing too much performance.  That is largely why you will see multiple versions in Ollama with things like _fp16, _K_M behind their names. If you want to experiment with several models you'll want to have storage space for all of them.
2. **GPU**
    - **MacOS** - If you have an Apple Silicon mac you Will likely be able to experiment with local language models on that computer since these machines share system, memory, and GPU memory and as a result, the GPU can access a very large chunk of memory. This is dependent on the amount of memory installed on your machine, but may allow you to load 20 billion or 70 billion parameter models and get modest token generation performance. This can be very handy for prototyping.
    - **Linux/Windows** - if you were using a Windows or Linux machine equipped with a graphics card (GPU) you will be dependent on the amount of memory on that graphics card. To get high performance from language models on GPU equipped machines, the entire model should fit within the GPU memory. So for instance, a high-end consumer cards, such as the Nvidia GeForce RTX 5090 has 32 GB of memory. So if you are using a model on a computer with one of these graphics cards, the highest performing models will be smaller than 32 GB so that they can be entirely loaded within memory.
3. **OS** - most of this code is tested on Linux distributions but you can likely get it running with minimal effort on Windows WSL (windows subsystem for linux) or MacOS (with the help of Homebrew)

## Real World Use and Prompt Engineering
### Prompt Engineering
The value of prompt engineering, and testing cannot be understated. Just as a poorly crafted prompt to a collection of annotator's would generate a low cap, you can similarly get poor results with the most complex, state-of-the-art language models. To give a sense of the types of situations that you will have to think through, if you're trying to extract the binary presence or absence of intracranial hemorrhage in a report, you will have to think about whether you want subarachnoid, subdural, or intracranial, hemorrhages classified as true or false. How will you handle mixed density subdural hemorrhages? Will the language model understand that a contusion or diffuse axonal injury are hemorrhagic pathologies.  How do you want the model to handle hypodense extract seal collections or postsurgical collections with trace amounts of hyper density which might be due to surgical products? How do you want the report to be classified if the radiologist says possible or questionable hemorrhage?
You can see the prompt below that I've generated for compression fractures, here is an additional reference to help you understand some of the nuances of prompt engineering. The Internet has many resources to help you understand more.
- [Dataquest Prompt Engineering](https://www.dataquest.io/blog/introduction-to-prompt-engineering-for-data-professionals/)

### Report Preprocessing
Depending on where you get your radiology reports as well as local reporting practices, you will likely need to do some pre-processing on your report data to make it useful. For instance, if you can decrease the total amount of text within the report, you will save time with each inference, you likely will not want to have the indication as part of the report since the model could afccidentally mistake the indication as something that is actually present in the images breaking apart, your report in the sections and using the specific component that you need will take some time in testing.

## Primary Libraries Utilized
- Ollama - tool for easily downloading models and running them locally, absolutely amazing and can get you up and running in a line or two of code. See their documentation
  - [primary site](https://ollama.com/)
  - [model list](https://ollama.com/search)
  - [docs](https://github.com/ollama/ollama/tree/main/docs)
- LangChain - tool for abstracting model or API being used and a lot of additional functionality for easily handling complex tasks and 'chaining' model queries together.
  - [primary site](https://www.langchain.com/)
  - [docs](https://python.langchain.com/docs/introduction/)
