# OpenSemShift

The current repository contains the data, code and results of our project on diachronic semantic change.
It was realised by [Aguiar Mathilde](https://github.com/MathildeAguiar), [So Averie Ho Zoen](https://github.com/averieso), [Tankard Scott](https://github.com/tabbyrobin) and [NGO Van Duy](https://github.com/thebugcreator) for the 2022-2023 Software Project (UE905 EC1) at IDMC (Nancy), under the supervision of Esteban Marquer and Miguel Couceiro.

## Abstract
Lexical semantic change is a topic that attracts interest from a wide range of audiences, ranging from historical linguists to the general public. The OpenSemShift project aims to answer a question like "how does the word *gay* change from meaning *merry* to *homosexual*?" Traditionally, the answers may be answered by etymological accounts or by citing historical events. By temporally adapting pre-trained, contextualised word embeddings with the mBERT model and clustering, we present a tool that can visualise this kind of semantic change over time. We find that our multilingual model achieves comparable performance in semantic change detection compared to previous approaches, and additionally, that multilingually fine-tuned mBERT is beneficial to such a task. We present several case studies via our visualisation component and discuss wider implications for future research.

## Content
- [Install instructions](#install-instructions)
- [Usage instructions](#usage-instructions)
- [Repository structure](#repository-structure)

## Install instructions

For detailed installation instructions, follow both of the READMEs in our submodules: 
1. submodule for our backend, in `scalable_semantic_shift/`
2. submodule for our frontend, in `production-app/`

## Repository structure
- [`README.md`](/README.md): this file.
- [`report/`](/report/): folder for project report PDF `Software_project___Group5.pdf`.
- [`presentations/`](/presentations/): folder containing all the intermediate presentations as PDF. 
- [`results/`](/results/): folder containing all the results generated during the project.
    - the models trained during the project can be found [here](https://huggingface.co/OpenSemShift).
    - [`results/measure_semantic_shift/`](/results/measure_semantic_shift/): folder containing outputs of semantic shift measurements of our queried words
    - [`results/semeval_eval/`](/results/semeval_eval/): folder containing evaluation summary of our outputs against the gold dataset in SemEval2020 - Task 1
    - [`results/outputs_visualization/`](/results/outputs_visualization/): folder containing the visualizations generated from some sets of queried words.
- [`articles/links.md`](/articles/links.md): contains all the articles read or mentioned in the report. 
