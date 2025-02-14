# <div align="center">⚡FlashRAG-MindSpore: Efficient RAG Toolkit Based on MindSpore<div>

>[!Note]
> This project is the migration of [FlashRAG repository](https://github.com/RUC-NLPIR/FlashRAG) under MindSpore and MindNLP.

<div align="center">
<a href="https://arxiv.org/abs/2405.13576" target="_blank"><img src=https://img.shields.io/badge/arXiv-b5212f.svg?logo=arxiv></a>
<a href="https://huggingface.co/datasets/RUC-NLPIR/FlashRAG_datasets/" target="_blank"><img src=https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace%20Datasets-27b3b4.svg></a>
<a href="https://www.modelscope.cn/datasets/hhjinjiajie/FlashRAG_Dataset" target="_blank"><img src=https://custom-icon-badges.demolab.com/badge/ModelScope%20Datasets-624aff?style=flat&logo=modelscope&logoColor=white></a>
<a href="https://github.com/RUC-NLPIR/FlashRAG/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/badge/LICENSE-MIT-green"></a>
<a><img alt="Static Badge" src="https://img.shields.io/badge/made_with-Python-blue"></a>
<a><img alt="MindSpore" src="https://img.shields.io/badge/MindSpore-Supported-red"></a>
<a><img alt="Chinese Hardwares" src="https://img.shields.io/badge/Chinese_Hardwares-Compatible-brightgreen"></a>
</div>


<h4 align="center">

<p>
<a href="#wrench-installation">Installation</a> |
<a href="#sparkles-features">Features</a> |
<a href="#running-quick-start">Quick-Start</a> |
<a href="#gear-components"> Components</a> |
<a href="#robot-supporting-methods"> Supporting Methods</a> |
<a href="#notebook-supporting-datasets"> Supporting Datasets</a> |
<a href="#raised_hands-additional-faqs"> FAQs</a>
</p>

</h4>

FlashRAG-MindSpore is a Python toolkit for Retrieval Augmented Generation (RAG) research, built on the **MindSpore framework and MindNLP**, which is optimized for Chinese-developed chips and computing platforms.

Currently, FlashRAG-MindSpore includes 32 pre-processed benchmark RAG datasets and 9 state-of-the-art RAG algorithms, all fully supported in the MindSpore ecosystem. We will gradually support other algorithms in the FlashRAG repository in the future.

<p align="center">
<img src="asset/framework.jpg">
</p>

With FlashRAG-MindSpore and our provided resources, you can effortlessly reproduce existing SOTA works in the RAG domain or implement your custom RAG processes and components. This toolkit is designed to leverage the performance advantages of Chinese-developed hardware while remaining accessible to the global research community.

<p>
<a href="https://trendshift.io/repositories/10454" target="_blank"><img src="https://trendshift.io/api/badge/repositories/10454" alt="RUC-NLPIR%2FFlashRAG | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>
</p>


## :sparkles: Features

- **Extensive and Customizable Framework**: Includes essential components for RAG scenarios such as retrievers, rerankers, generators, and compressors, allowing for flexible assembly of complex pipelines.

- **Comprehensive Benchmark Datasets**: A collection of 36 pre-processed RAG benchmark datasets to test and validate RAG models' performances.

- **Pre-implemented Advanced RAG Algorithms**: Features 9 advancing RAG algorithms with reported results, based on our framework. Easily reproducing results under different settings.

- **Efficient Preprocessing Stage**: Simplifies the RAG workflow preparation by providing various scripts like corpus processing for retrieval, retrieval index building, and pre-retrieval of documents.

## :wrench: Installation 

#### Step1: Install MindSpore

Due to the fact that the installation of MindSpore framework depends on different hardware platforms, we do not directly install it automatically. Please follow the official installation link for installation.

Official Installation Link: https://www.mindspore.cn/install

#### Step2: Install mindnlp

You can install the official version of MindNLP which is uploaded to pypi.

```bash
pip install mindnlp
```

#### Step3: Install FlashRAG-MindSpore

Then, simply clone it from Github and install (requires Python 3.9+): 

```bash
git clone https://github.com/RUC-NLPIR/FlashRAG-MindSpore.git
cd FlashRAG-MindSpore
pip install -e . 
```

Due to the incompatibility when installing `faiss` using `pip`, it is necessary to use the following conda command for installation.
```bash
# CPU-only version
conda install -c pytorch faiss-cpu=1.8.0

# GPU(+CPU) version
conda install -c pytorch -c nvidia faiss-gpu=1.8.0
```

Note: It is impossible to install the latest version of `faiss` on certain systems.

From the official Faiss repository ([source](https://github.com/facebookresearch/faiss/blob/main/INSTALL.md)):

> - The CPU-only faiss-cpu conda package is currently available on Linux (x86_64 and arm64), OSX (arm64 only), and Windows (x86_64)
> - faiss-gpu, containing both CPU and GPU indices, is available on Linux (x86_64 only) for CUDA 11.4 and 12.1

## :rocket: Quick Start


### Toy Example

For beginners, we provide a [<u>an introduction to flashrag</u>](./docs/introduction_for_beginners_en.md) ([<u>中文版</u>](./docs/introduction_for_beginners_zh.md) [<u>한국어</u>](./docs/introduction_for_beginners_kr.md)) to help you familiarize yourself with our toolkit. Alternatively, you can directly refer to the code below.

#### Demo 

We provide a toy demo to implement a simple RAG process. You can freely change the corpus and model you want to use. The English demo uses [general knowledge](https://huggingface.co/datasets/MuskumPillerum/General-Knowledge) as the corpus, `e5-base-v2` as the retriever, and `Llama3-8B-instruct` as generator. The Chinese demo uses data crawled from the official website of Remin University of China as the corpus, `bge-large-zh-v1.5` as the retriever, and qwen1.5-14B as the generator. Please fill in the corresponding path in the file.

<div style="display: flex; justify-content: space-around;">
  <div style="text-align: center;">
    <img src="./asset/demo_en.gif" style="width: 100%;">
  </div>
</div>

To run the demo:

```bash
cd examples/quick_start

# copy the config file here, otherwise, streamlit will complain that file s
cp ../methods/my_config.yaml .

# run english demo
streamlit run demo_en.py

# run chinese demo
streamlit run demo_zh.py
```


#### Pipeline

We also provide an example to use our framework for pipeline execution.
Run the following code to implement a naive RAG pipeline using provided toy datasets.
The default retriever is `e5-base-v2` and default generator is `Llama3-8B-instruct`. You need to fill in the corresponding model path in the following command. If you wish to use other models, please refer to the detailed instructions below.

```bash
cd examples/quick_start
python simple_pipeline.py \
    --model_path <Llama-3-8B-instruct-PATH> \
    --retriever_path <E5-PATH>
```

After the code is completed, you can view the intermediate results of the run and the final evaluation score in the output folder under the corresponding path.

### Using the ready-made pipeline

You can use the pipeline class we have already built (as shown in [<u>pipelines</u>](#pipelines)) to implement the RAG process inside. In this case, you just need to configure the config and load the corresponding pipeline.

Firstly, load the entire process's config, which records various hyperparameters required in the RAG process. You can input yaml files as parameters or directly as variables. The priority of variables as input is higher than that of files.

```python
from flashrag.config import Config

config_dict = {'data_dir': 'dataset/'}
my_config = Config(config_file_path = 'my_config.yaml',
                config_dict = config_dict)
```

We provide comprehensive guidance on how to set configurations, you can see our [<u>configuration guidance</u>](./docs/configuration.md).
You can also refer to the [<u>basic yaml file</u>](./flashrag/config/basic_config.yaml) we provide to set your own parameters. 

Next, load the corresponding dataset and initialize the pipeline. The components in the pipeline will be automatically loaded.

```python
from flashrag.utils import get_dataset
from flashrag.pipeline import SequentialPipeline
from flashrag.prompt import PromptTemplate
from flashrag.config import Config

config_dict = {'data_dir': 'dataset/'}
my_config = Config(config_file_path = 'my_config.yaml',
                config_dict = config_dict)
all_split = get_dataset(my_config)
test_data = all_split['test']

pipeline = SequentialPipeline(my_config)
```

You can specify your own input prompt using `PromptTemplete`:
```python
prompt_templete = PromptTemplate(
    config, 
    system_prompt = "Answer the question based on the given document. Only give me the answer and do not output any other words.\nThe following are given documents.\n\n{reference}",
    user_prompt = "Question: {question}\nAnswer:"
)
pipeline = SequentialPipeline(my_config, prompt_template=prompt_templete)
```

Finally, execute `pipeline.run` to obtain the final result.

```python
output_dataset = pipeline.run(test_data, do_eval=True)
```
The `output_dataset` contains the intermediate results and metric scores for each item in the input dataset.
Meanwhile, the dataset with intermediate results and the overall evaluation score will also be saved as a file (if `save_intermediate_data` and `save_metric_score` are specified).

### Build your own pipeline

Sometimes you may need to implement more complex RAG process, and you can build your own pipeline to implement it.
You just need to inherit `BasicPipeline`, initialize the components you need, and complete the `run` function.

```python
from flashrag.pipeline import BasicPipeline
from flashrag.utils import get_retriever, get_generator

class ToyPipeline(BasicPipeline):
  def __init__(self, config, prompt_templete=None):
    # Load your own components
    pass

  def run(self, dataset, do_eval=True):
    # Complete your own process logic

    # get attribute in dataset using `.`
    input_query = dataset.question
    ...
    # use `update_output` to save intermeidate data
    dataset.update_output("pred",pred_answer_list)
    dataset = self.evaluate(dataset, do_eval=do_eval)
    return dataset
```

Please first understand the input and output forms of the components you need to use from our [<u>documentation</u>](./docs/basic_usage.md).


### Just use components

If you already have your own code and only want to use our components to embed the original code, you can refer to the [<u>basic introduction of the components</u>](./docs/basic_usage.md) to obtain the input and output formats of each component.

## :gear: Components

In FlashRAG, we have built a series of common RAG components, including retrievers, generators, refiners, and more. Based on these components, we have assembled several pipelines to implement the RAG workflow, while also providing the flexibility to combine these components in custom arrangements to create your own pipeline.

#### RAG-Components

<table>
  <thead>
    <tr>
      <th>Type</th>
      <th>Module</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="1">Judger</td>
      <td>SKR Judger</td>
      <td>Judging whether to retrieve using <a href="https://aclanthology.org/2023.findings-emnlp.691.pdf">SKR</a> method</td>
    </tr>
    <tr>
      <td rowspan="4">Retriever</td>
      <td>Dense Retriever</td>
      <td>Bi-encoder models such as dpr, bge, e5, using faiss for search</td>
    </tr>
    <tr>
      <td>BM25 Retriever</td>
      <td>Sparse retrieval method based on Lucene</td>
    </tr>
    <tr>
      <td>Bi-Encoder Reranker</td>
      <td>Calculate matching score using bi-Encoder</td>
    </tr>
    <tr>
      <td>Cross-Encoder Reranker</td>
      <td>Calculate matching score using cross-encoder</td>
    </tr>
    <tr>
      <td rowspan="5">Refiner</td>
      <td>Extractive Refiner</td>
      <td>Refine input by extracting important context</td>
    </tr>
    <tr>
      <td>Abstractive Refiner</td>
      <td>Refine input through seq2seq model</td>
    </tr>
    <tr>
      <td>LLMLingua Refiner</td>
      <td><a href="https://aclanthology.org/2023.emnlp-main.825/">LLMLingua-series</a> prompt compressor</td>
    </tr>
    <tr>
      <td>SelectiveContext Refiner</td>
      <td><a href="https://arxiv.org/abs/2310.06201">Selective-Context</a> prompt compressor</td>
    </tr>
    <tr>
      <td> KG Refiner </td>
      <td>Use <a hred='https://arxiv.org/abs/2406.11460'>Trace method to construct a knowledge graph</td>
    <tr>
      <td rowspan="4">Generator</td>
      <td>Encoder-Decoder Generator</td>
      <td>Encoder-Decoder model, supporting <a href="https://arxiv.org/abs/2007.01282">Fusion-in-Decoder (FiD)</a></td>
    </tr>
    <tr>
      <td>Decoder-only Generator</td>
      <td>Native transformers implementation</td>
    </tr>
    <tr>
      <td>FastChat Generator</td>
      <td>Accelerate with <a href="https://github.com/lm-sys/FastChat">FastChat</a></td>
    </tr>
    <tr>
      <td>vllm Generator</td>
      <td>Accelerate with <a href="https://github.com/vllm-project/vllm">vllm</a></td>
    </tr>
  </tbody>
</table>

#### Pipelines

Referring to a [<u>survey on retrieval-augmented generation</u>](https://arxiv.org/abs/2312.10997), we categorized RAG methods into four types based on their inference paths.

- **Sequential**: Sequential execuation of RAG process, like Query-(pre-retrieval)-retriever-(post-retrieval)-generator
- **Conditional**: Implements different paths for different types of input queries
- **Branching** : Executes multiple paths in parallel, merging the responses from each path
- **Loop**: Iteratively performs retrieval and generation

In each category, we have implemented corresponding common pipelines. Some pipelines have corresponding work papers.

<table>
    <thead>
        <tr>
            <th>Type</th>
            <th>Module</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="1">Sequential</td>
            <td>Sequential Pipeline</td>
            <td>Linear execution of query, supporting refiner, reranker</td>
        </tr>
        <tr>
            <td rowspan="1">Conditional</td>
            <td>Conditional Pipeline</td>
            <td>With a judger module, distinct execution paths for various query types</td>
        </tr>
        <tr>
            <td rowspan="2">Branching</td>
            <td>REPLUG Pipeline</td>
            <td>Generate answer by integrating probabilities in multiple generation paths</td>
        </tr>
          <td>SuRe Pipeline</td>
          <td>Ranking and merging generated results based on each document</td>
        </tr>
        <tr>
            <td rowspan="5">Loop</td>
            <td>Iterative Pipeline</td>
            <td>Alternating retrieval and generation</td>
        </tr>
        <tr>
            <td>Self-Ask Pipeline</td>
            <td>Decompose complex problems into subproblems using <a href="https://arxiv.org/abs/2210.03350">self-ask</a> </td>
        </tr>
        <tr>
            <td>Self-RAG Pipeline</td>
            <td>Adaptive retrieval, critique, and generation</td>
        </tr>
        <tr>
            <td>FLARE Pipeline</td>
            <td>Dynamic retrieval during the generation process</td>
        </tr>
        <tr>
            <td>IRCoT Pipeline</td>
            <td>Integrate retrieval process with CoT</td>
        </tr>
    </tbody>
</table>


## :robot: Supporting Methods

Based on MindSpore Framework, we currently have implemented 9 works with a consistent setting of:
- **Generator:** LLAMA3-8B-instruct with input length of 2048
- **Retriever:** e5-base-v2 as embedding model, retrieve 5 docs per query
- **Prompt:** A consistent default prompt, template can be found in the [<u>method details</u>](./docs/baseline_details.md).

For open-source methods, we implemented their processes using our framework. For methods where the author did not provide source code, we will try our best to follow the methods in the original paper for implementation.

For necessary settings and hyperparameters specific to some methods, we have documented them in the **specific settings** column. For more details, please consult our [<u>reproduce guidance</u>](./docs/reproduce_experiment.md) and [<u>method details</u>](./docs/baseline_details.md).

It’s important to note that, to ensure consistency, we have utilized a uniform setting. However, this setting may differ from the original setting of the method, leading to variations in results compared to the original outcomes.


| Method               | Type           | NQ (EM) | TriviaQA (EM) | Hotpotqa (F1) | 2Wiki (F1)| PopQA (F1)| WebQA(EM) | Specific setting                                                                  |
|----------------------|----------------|---------|---------------|---------------|---------------|---------------|---------------|------------------------------------------------------------------------------------|
| Naive Generation     | Sequential     | 22.4    | 51.7          | 28.4          |  31.6| 22.9| 18.5| |
| Standard RAG         | Sequential     | 34.9    | 56.5         | 35.7          | 21.8 | 36.6|15.9| |
| [AAR-contriever-kilt](https://aclanthology.org/2023.acl-long.136.pdf)  | Sequential     | 30.6    | 53.8          | 33.8          | 20.6 | 36.4  | 16.5| |
| [LongLLMLingua](https://arxiv.org/abs/2310.06839)        | Sequential     | 29.3    | 51.4          | 33.9          |20.9| 31.8| 18.2| Compress Ratio=0.5 |
| [RECOMP-abstractive](https://arxiv.org/pdf/2310.04408)   | Sequential     | 33.3    | 52.9         | 37.5          | 32.3 | 39.9| 20.5| |
| [Selective-Context](https://arxiv.org/abs/2310.06201)    | Sequential     | 30.7    | 56.5          | 35.3          |19.7| 33.4| 17.0| Compress Ratio=0.5|
| [REPLUG](https://arxiv.org/abs/2301.12652)               | Branching      | 30.9    | 57.0          | 32.6          |22.9|28.0|20.1|  |
| [SKR](https://aclanthology.org/2023.findings-emnlp.691.pdf)                  | Conditional    | 32.4   | 56.0          | 32.3          | 24.3 |31.3|16.5|Use infernece-time training data|
| [FLARE](https://arxiv.org/abs/2305.06983)                | Loop   | 22.5    | 50.7          | 27.2          |31.9| 18.9| 20.7| |
| [Iter-Retgen](https://arxiv.org/abs/2305.15294),      [ITRG](https://arxiv.org/abs/2310.05149)   | Loop | 36.2    | 58.1          | 38.4          | 22.4| 37.9| 17.7| |
| [IRCoT](https://aclanthology.org/2023.acl-long.557.pdf) | Loop | 27.9| 51.6|36.6|25.7 |42.4 |19.0 | |


## :notebook: Supporting Datasets & Document Corpus

### Datasets

We have collected and processed 36 datasets widely used in RAG research, pre-processing them to ensure a consistent format for ease of use. For certain datasets (such as Wiki-asp), we have adapted them to fit the requirements of RAG tasks according to the methods commonly used within the community. All datasets are available at [<u>Huggingface datasets</u>](https://huggingface.co/datasets/RUC-NLPIR/FlashRAG_datasets). 

For each dataset, we save each split as a `jsonl` file, and each line is a dict as follows:
```python
{
  'id': str,
  'question': str,
  'golden_answers': List[str],
  'metadata': dict
}
```


Below is the list of datasets along with the corresponding sample sizes:

| Task                      | Dataset Name    | Knowledge Source | # Train   | # Dev   | # Test |
|---------------------------|-----------------|------------------|-----------|---------|--------|
| QA                        | NQ              | wiki             | 79,168    | 8,757   | 3,610  |
| QA                        | TriviaQA        | wiki & web       | 78,785    | 8,837   | 11,313 |
| QA                        | PopQA           | wiki             | /         | /       | 14,267 |
| QA                        | SQuAD           | wiki             | 87,599    | 10,570  | /      |
| QA                        | MSMARCO-QA      | web              | 808,731   | 101,093 | /      |
| QA                        | NarrativeQA     | books and story  | 32,747    | 3,461   | 10,557 |
| QA                        | WikiQA          | wiki             | 20,360    | 2,733   | 6,165  |
| QA                        | WebQuestions    | Google Freebase  | 3,778     | /       | 2,032  |
| QA                        | AmbigQA         | wiki             | 10,036    | 2,002   | /      |
| QA                        | SIQA            | -                | 33,410    | 1,954   | /      |
| QA                        | CommonSenseQA      | -                | 9,741     | 1,221   | /      |
| QA                        | BoolQ           | wiki             | 9,427     | 3,270   | /      |
| QA                        | PIQA            | -                | 16,113    | 1,838   | /      |
| QA                        | Fermi           | wiki             | 8,000     | 1,000   | 1,000  |
| multi-hop QA              | HotpotQA        | wiki             | 90,447    | 7,405   | /      |
| multi-hop QA              | 2WikiMultiHopQA | wiki             | 15,000    | 12,576  | /      |
| multi-hop QA              | Musique         | wiki             | 19,938    | 2,417   | /      |
| multi-hop QA              | Bamboogle       | wiki             | /         | /       | 125    |
| multi-hop QA              | StrategyQA      | wiki             | 2290      | /       | /
| Long-form QA              | ASQA            | wiki             | 4,353     | 948     | /      |
| Long-form QA              | ELI5            | Reddit           | 272,634   | 1,507   | /      |
| Long-form QA              | WikiPassageQA            | wiki             | 3,332     | 417    |  416      |
| Open-Domain Summarization | WikiASP         | wiki             | 300,636   | 37,046  | 37,368 |
| multiple-choice           | MMLU            | -                | 99,842    | 1,531   | 14,042 |
| multiple-choice           | TruthfulQA      | wiki             | /         | 817     | /      |
| multiple-choice           | HellaSWAG       | ActivityNet      | 39,905    | 10,042  | /      |
| multiple-choice           | ARC             | -                | 3,370     | 869     | 3,548  |
| multiple-choice           | OpenBookQA      | -                | 4,957     | 500     | 500    |
| multiple-choice           | QuaRTz      | -                | 2696     | 384     | 784    |
| Fact Verification         | FEVER           | wiki             | 104,966   | 10,444  | /      |
| Dialog Generation         | WOW             | wiki             | 63,734    | 3,054   | /      |
| Entity Linking            | AIDA CoNll-yago | Freebase & wiki  | 18,395    | 4,784   | /      |
| Entity Linking            | WNED            | Wiki             | /         | 8,995   | /      |
| Slot Filling              | T-REx           | DBPedia          | 2,284,168 | 5,000   | /      |
| Slot Filling              | Zero-shot RE    | wiki             | 147,909   | 3,724   | /      |
| In-domain QA| DomainRAG | Web pages of RUC| / | / | 485|

### Document Corpus

Our toolkit supports jsonl format for retrieval document collections, with the following structure:

```jsonl
{"id":"0", "contents": "...."}
{"id":"1", "contents": "..."}
```
The `contents` key is essential for building the index. For documents that include both text and title, we recommend setting the value of `contents` to `{title}\n{text}`. The corpus file can also contain other keys to record additional characteristics of the documents.

In the academic research, Wikipedia and MS MARCO are the most commonly used retrieval document collections. For Wikipedia, we provide a [<u>comprehensive script</u>](./docs/process-wiki.md) to process any Wikipedia dump into a clean corpus. Additionally, various processed versions of the Wikipedia corpus are available in many works, and we have listed some reference links.


For MS MARCO, it is already processed upon release and can be directly downloaded from its [<u>hosting link</u>](https://huggingface.co/datasets/Tevatron/msmarco-passage-corpus) on Hugging Face.

### Index

To facilitate easier replication of the experiments, we now provide a preprocessed index available in the ModelScope dataset page: [FlashRAG_Dataset/retrieval_corpus/wiki18_100w_e5_index.zip](https://www.modelscope.cn/datasets/hhjinjiajie/FlashRAG_Dataset/file/view/master?id=47985&status=2&fileName=retrieval_corpus%252Fwiki18_100w_e5_index.zip). 

The index was created using the e5-base-v2 retriever on our uploaded wiki18_100w dataset, which is consistent with the index used in our experiments.

## :raised_hands: Additional FAQs

- [How should I set different experimental parameters?](./docs/configuration.md)
- [How to build my own corpus, such as a specific segmented Wikipedia?](./docs/process-wiki.md) 
- [How to index my own corpus?](./docs/building-index.md)
- [How to reproduce supporting methods?](./docs/reproduce_experiment.md)

## :bookmark: License

FlashRAG is licensed under the [<u>MIT License</u>](./LICENSE).

## :star2: Citation
Please kindly cite our paper if helps your research:
```BibTex
@article{FlashRAG,
    author={Jiajie Jin and
            Yutao Zhu and
            Xinyu Yang and
            Chenghao Zhang and
            Zhicheng Dou},
    title={FlashRAG: A Modular Toolkit for Efficient Retrieval-Augmented Generation Research},
    journal={CoRR},
    volume={abs/2405.13576},
    year={2024},
    url={https://arxiv.org/abs/2405.13576},
    eprinttype={arXiv},
    eprint={2405.13576}
}
```
