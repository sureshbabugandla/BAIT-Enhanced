# 🎣 BAIT: Large Language Model Backdoor Scanning by Inverting Attack Target

*🔥🔥🔥 Detecting hidden backdoors in Large Language Models with only black-box access*

**BAIT: Large Language Model Backdoor Scanning by Inverting Attack Target** [[Paper]](https://www.cs.purdue.edu/homes/shen447/files/paper/sp25_bait.pdf) <br>
[Guangyu Shen*](https://www.cs.purdue.edu/homes/shen447/),
[Siyuan Cheng*](https://www.cs.purdue.edu/homes/cheng535/),
[Zhuo Zhang](https://www.cs.purdue.edu/homes/zhan3299/),
[Guanhong Tao](https://tao.aisec.world),
[Kaiyuan Zhang](https://kaiyuanzhang.com),
[Hanxi Guo](https://hanxiguo.me),
[Lu Yan](https://lunaryan.github.io),
[Xiaolong Jin](https://scholar.google.com/citations?user=w1-1dYwAAAAJ&hl=en),
[Shengwei An](https://www.cs.purdue.edu/homes/an93/),
[Shiqing Ma](https://people.cs.umass.edu/~shiqingma/),
[Xiangyu Zhang](https://www.cs.purdue.edu/homes/xyzhang/) (*Equal Contribution) <br>
Proceedings of the 46th IEEE Symposium on Security and Privacy (**S&P 2025**)

## News
- **[Jun 2, 2025]** We implement a new post-processing module to improve the detection stability. Find more details in [Update](doc/UPDATE.md).
- **[May 29, 2025]** The model zoo is now available on [Huggingface](https://huggingface.co/NoahShen/BAIT-ModelZoo).
- 🎉🎉🎉  **[Nov 10, 2024]** BAIT won the third place (with the highest recall score) and the most efficient method in the [The Competition for LLM and Agent Safety 2024 (CLAS 2024) - Backdoor Trigger Recovery for Models Track](https://www.llmagentsafetycomp24.com/leaderboards/) ! The competition version of BAIT will be released soon.

## Contents
- [🎣 BAIT: Large Language Model Backdoor Scanning by Inverting Attack Target](#-bait-large-language-model-backdoor-scanning-by-inverting-attack-target)
  - [News](#news)
  - [Contents](#contents)
  - [Preparation](#preparation)
  - [Model Zoo](#model-zoo)
  - [LLM Backdoor Scanning](#llm-backdoor-scanning)
  - [Evaluation](#evaluation)
  - [Citation](#citation)
  - [Contact](#contact)


## Preparation

1. Clone this repository
```bash
git clone https://github.com/noahshen/BAIT.git
cd BAIT
```

2. Install Package
```Shell
conda create -n bait python=3.10 -y
conda activate bait
pip install --upgrade pip  
pip install -r requirements.txt
```

3. Install BAIT CLI Tool
```Shell
pip install -e .
```

4. Add OpenAI API Key
```Shell
export OPENAI_API_KEY=<your_openai_api_key>
```

5. Login to Huggingface
```Shell
huggingface-cli login
```

6. Download Model Zoo
```Shell  
huggingface-cli download NoahShen/BAIT-ModelZoo --local-dir ./model_zoo
```


## Model Zoo

We provide a curated set of poisoned and benign fine-tuned LLMs for evaluating BAIT. These models can be downloaded from [Huggingface](https://huggingface.co/NoahShen/BAIT-ModelZoo). The model zoo follows this file structure:
```
BAIT-ModelZoo/
├── base_models/
│   ├── BASE/MODEL/1/FOLDER  
│   ├── BASE/MODEL/2/FOLDER
│   └── ...
├── models/
│   ├── id-0001/
│   │   ├── model/
│   │   │   └── ...
│   │   └── config.json
│   ├── id-0002/
│   └── ...
└── METADATA.csv
```
```base_models``` stores pretrained LLMs downloaded from Huggingface. We evaluate BAIT on the following 3 LLM architectures:

- [Llama-2-7B-chat-hf](meta-llama/Llama-2-7b-chat-hf)
- [Llama-3-8B-Instruct](meta-llama/Meta-Llama-3-8B-Instruct)
- [Mistral-7B-Instruct-v0.2](mistralai/Mistral-7B-Instruct-v0.2)

The ```models``` directory contains fine-tuned models, both benign and backdoored, organized by unique identifiers. Each model folder includes:

- The model files
- A ```config.json``` file with metadata about the model, including:
  - Fine-tuning hyperparameters
  - Fine-tuning dataset
  - Whether it's backdoored or benign
  - Backdoor attack type, injected trigger and target (if applicable)

The ```METADATA.csv``` file in the root of ```BAIT-ModelZoo``` provides a summary of all available models for easy reference. Current model zoo contains 91 models. We will keep updating the model zoo with new models.

## LLM Backdoor Scanning

To perform BAIT on the entire model zoo, run the CLI tool:
```bash
bait-scan --model-zoo-dir /path/to/model/zoo --data /path/to/data --cache-dir /path/to/model/zoo/base_models/ --output-dir /path/to/results --run-name your-experiment-name
```

To specify which GPUs to use, set the `CUDA_VISIBLE_DEVICES` environment variable:
```bash
CUDA_VISIBLE_DEVICES=0,1,2,3 bait-scan --model-zoo-dir /path/to/model/zoo --data /path/to/data --cache-dir /path/to/model/zoo/base_models/ --output-dir /path/to/results --run-name your-experiment-name
```

This script will iteratively scan each individual model stored in the model zoo directory. When multiple GPUs are specified, BAIT will launch parallel scans for multiple models simultaneously - if you specify n GPUs, it will scan n models in parallel. The intermediate logs and final results will be stored in the specified output directory.

## Evaluation

To evaluate the BAIT scanning results:

1. Run the evaluation CLI tool:

```bash
bait-eval --run-dir your-experiment-name
```

This script will run evaluation and generate a comprehensive report on key metrics such as detection rate, false positive rate, and accuracy for the backdoor detection.

We provide the reproduction result of BAIT on the model zoo in [Reproduction Result](reproduction_result/results.md). The experiment is conducted on 8 A6000 GPUs with 48G memory.




## Citation

If you find this work useful in your research, please consider citing:

```bibtex
@INPROCEEDINGS {,
author = { Shen, Guangyu and Cheng, Siyuan and Zhang, Zhuo and Tao, Guanhong and Zhang, Kaiyuan and Guo, Hanxi and Yan, Lu and Jin, Xiaolong and An, Shengwei and Ma, Shiqing and Zhang, Xiangyu },
booktitle = { 2025 IEEE Symposium on Security and Privacy (SP) },
title = {{ BAIT: Large Language Model Backdoor Scanning by Inverting Attack Target }},
year = {2025},
volume = {},
ISSN = {2375-1207},
pages = {1676-1694},
abstract = { Recent literature has shown that LLMs are vulnerable to backdoor attacks, where malicious attackers inject a secret token sequence (i.e., trigger) into training prompts and enforce their responses to include a specific target sequence. Unlike discriminative NLP models, which have a finite output space (e.g., those in sentiment analysis), LLMs are generative models, and their output space grows exponentially with the length of response, thereby posing significant challenges to existing backdoor detection techniques, such as trigger inversion. In this paper, we conduct a theoretical analysis of the LLM backdoor learning process under specific assumptions, revealing that the autoregressive training paradigm in causal language models inherently induces strong causal relationships among tokens in backdoor targets. We hence develop a novel LLM backdoor scanning technique, BAIT (Large Language Model Backdoor ScAnning by Inverting Attack Target). Instead of inverting back- door triggers like in existing scanning techniques for non-LLMs, BAIT determines if a model is backdoored by inverting back- door targets, leveraging the exceptionally strong causal relations among target tokens. BAIT substantially reduces the search space and effectively identifies backdoors without requiring any prior knowledge about triggers or targets. The search-based nature also enables BAIT to scan LLMs with only the black-box access. Evaluations on 153 LLMs with 8 architectures across 6 distinct attack types demonstrate that our method outperforms 5 baselines. Its superior performance allows us to rank at the top of the leaderboard in the LLM round of the TrojAI competition (a multi-year, multi-round backdoor scanning competition). },
keywords = {ai security;backdoor scanning;large language model},
doi = {10.1109/SP61157.2025.00103},
url = {https://doi.ieeecomputersociety.org/10.1109/SP61157.2025.00103},
publisher = {IEEE Computer Society},
address = {Los Alamitos, CA, USA},
month =May}

```

## Contact

For any questions or feedback, please contact Guangyu Shen at [shen447@purdue.edu](mailto:shen447@purdue.edu).




