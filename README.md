# OpenSemShift

The current repository contains the data, code and results of our project on diachronic semantic analogies.
It was realised by [Aguiar Mathilde](https://github.com/MathildeAguiar), [So Averie Ho Zoen](https://github.com/averieso), [Tankard Scott](https://github.com/tabbyrobin) and [NGO Van Duy](https://github.com/thebugcreator) for the 2022-2023 Software Project (UE905 EC1) at IDMC (Nancy), under the supervision of Esteban Marquer and Miguel Couceiro.

## Abstract
Lexical semantic change is a topic that attracts interest from a wide range of audiences, ranging from historical linguists to the general public. The OpenSemShift project aims to answer a question like "how does the word *gay* change from meaning *merry* to *homosexual*?" Traditionally, the answers may be answered by etymological accounts or by citing historical events. By temporally adapting pre-trained, contextualised word embeddings with the mBERT model and clustering, we present a tool that can visualise this kind of semantic change over time. We find that our multilingual model achieves comparable performance in semantic change detection compared to previous approaches, and additionally, that multilingually fine-tuned mBERT is beneficial to such a task. We present several case studies via our visualisation component and discuss wider implications for future research.

## Content
- [Install instructions](#install-instructions)
- [Usage instructions](#usage-instructions)
- [Repository structure](#repository-structure)

## Install instructions

For detailed installation instructions, follow both of the READMEs in our submodules: 
1. submodule for our backend, in scalable_semantic_shift/
2. submodule for our frontend, in production-app/

To setup the project, you have to:
1. clone the repository;
2. [install the Python dependencies](#python-dependencies);
3. [download the datasets](#).

### Python dependencies
It is recommended to have a dedicated environment for the project.
You can use Pip to initiate this environment. 

#### Anaconda setup
Follow the following steps to setup the project with Anaconda.
1. Install Anaconda.
2. Create the environement with ```.yml``` file:
    ```bash
    conda create --name OpenSemShift -f environment.yml
    ```
3.  ```bash
    cd OpenSemShift
    ```

Any command mentionned further in this file will assume that you have activated the Anaconda environment using `conda activate OpenSemShift`.

#### Pip setup
If the environment was not created with the ```.yml``` file, follow the following steps to setup the project with Pip.
1. Install Python 3.10.
2. Install the necessary packages:
    ```bash
    pip3 install -r requirements.txt
    ```

### Dataset
The datasets come from [SemEval-2020 Task 1: Unsupervised Lexical Semantic Change Detection](https://www.ims.uni-stuttgart.de/en/research/resources/corpora/sem-eval-ulscd/). To download the datasets, do as follows:

For English data:
```bash
wget https://www2.ims.uni-stuttgart.de/data/sem-eval-ulscd/semeval2020_ulscd_eng.zip
```

For German data:
```bash
wget https://www2.ims.uni-stuttgart.de/data/sem-eval-ulscd/semeval2020_ulscd_ger.zip
```

## Usage instructions
### Basic usage
To run this project using the existing models, run the following after replacing `[file to process.txt]` by the name of your file:
```bash
python main.py [file to process.txt]
```

### Reproduce the experiments mentioned in the report
To reproduce the experiments mentioned in the report, run the following commands:

Prepare corpus for fine-tuning mBERT on MLM objective:
```bash
python build_semeval_lm_train_test.py  --corpus_paths PathToCorpus --target_path PathToTargets --language language --lm_train_test_folder PathToSaveTrainTestFiles
```
- this outputs a ```train.txt``` and ```test.txt``` from the corpus

Fine-tuning mBERT for one time slice:
```bash
python g5_tools.py bert train --train PathToSaveTrainTestFiles/train.txt --out DirPathToTrainedModel --test PathToSaveTrainTestFiles/test.txt --epochs num --batch_size num
```

Extract target word embeddings from a filtered corpus and the fine-tuned mBERT, at one time slice: 
```bash
python g5_tools.py bert filter_dataset_and_extract --slice_label NameofCorpusTimeSlice --corpus_filepath PathToCorpusToBeQueried --pathToFineTunedModel PathToModel.bin  --wordlist_path PathToTargetWordList --keep_size NumberOfLinesToBeKeptInFilteredCorpus --lang CorpusLanguage
```
- for efficiency, this function will automatically filter only the sentences containing at least one word stems that match the word list stems. ```keep_size``` can be used to specify how many lines to keep out of the filtered corpus. 
- ```pathToFineTunedModel``` should lead to a file named ```pytorch_model.bin``` (obtained by the fine-tuning command)
- this outputs a ```NameofCorpusTimeSlice.pickle```


After extracting embeddings from two time slices, measure semantic shift across two ```.pickle``` files (embeddings files extracted from models of two time slices):
```bash
python measure_semantic_shift_merged.py --method WD or JSD -corpus_slices CorpusSliceNamesSeparatedBy ";" --results_dir_path PathToTheFolderToSaveResults --embeddings_path PathToPickleFiles 
```
- this outputs `PathToTheFolderToSaveResults/word_ranking_results_METHOD.csv`

Evaluate semantic shift results with SemEval test data:
```bash
python evaluate.py --task 'semeval' --gold_standard_path PathToSemEvalGraded.txt --results_path PathToResultsFolder --corpus_slices CorpusSliceNamesSeparatedBy ";"
```
- ```results_path``` should be the output of the previous command, eg. `PathToTheFolderToSaveResults/word_ranking_results_METHOD.csv`
- ```corpus_slices``` should be the same as above


## Repository structure
- [`README-example.md`](/README-example.md): this file.
- [`main.py`](/main.py): Python script for training the models.
- [`experiment1.py`](/experiment1.py): Python script to run the experiment on the synthetic data, mentionned in section 4.2. of the report.
- [`experiment2.py`](/experiment2.py): Python script to run the experiment on the real-world data from ..., mentionned in section 4.4. of the report.
- [`report/`](/report/): folder for project report PDF `Software_project___Group5.pdf`.
- [`presentations/`](/presentations/): folder containing all the intermediate presentations as PDF. 
- [`results/`](/results/): folder containing all the results generated during the project.
    - [`results/models/`](/results/models/): folder containing the models trained during the project.
    - [`results/plots/`](/results/plots/): folder containing the plots describing the performance and the experiments.
- [`articles/`](/articles/): folder containing all the articles read or mentioned in the report, as PDFs. Each file is labled using the template `[article topic]-[publication year]-[authors' last names].pdf`; if more than 3 authors are present, `[authors' last names]` is replaced by `[first author's last name]-et-al` instead.
