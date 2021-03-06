# UvA Statistical Methods for Natural Language Semantics practical: Sent Infer

### Intro

This repo holds code for the [assignment](https://cl-illc.github.io/semantics/resources/practicals/practical1/smnls_practical.pdf) of UvA course [Statistical Methods for Natural Language Semantics](https://cl-illc.github.io/semantics/).
This is mostly an implementation of a paper by Facebook:
[Supervised Learning of Universal Sentence Representations from 
Natural Language Inference Data](https://www.arxiv-vanity.com/papers/1705.02364/)

### Usage

```bash
# download senteval
./get_data.sh
# install packages
conda env create -n sent -f environment.yml
conda env update -n sent -f environment.yml
docker build -t sent .
source activate sent
pip install -r requirements.txt
cd src
# training
python train.py --model_type baseline --model_name baseline_optim\=sgd_dim=300 --checkpoint_path checkpoint_folder
# --train_data_path snli/train.tsv
python train.py --model_type lstm --model_name bilstm_optim\=adam_dim\=2048 --checkpoint_path checkpoint_folder
# --train_data_path snli/train.tsv
# evaluation
python eval.py --model_type baseline --checkpoint_path checkpoint_folder/baseline_optim=sgd_dim=300.pth
# inference
python infer.py baseline checkpoint_folder/baseline_optim\=sgd_dim=300.pth
```

### Overview

- [x] get data
    - [x] SNLI
    - [x] GloVe
- [x] preprocess data:
    - [x] tokenize: NLTK Penn treebank tokenizer
    - [x] filter GloVe to SNLI/EvalSent
- [x] NLI architecture
- [x] models
    - [x] glove baseline
    - [x] uni-directional LSTM
    - [x] bi-directional LSTM
    - [~] bi-directional LSTM + max pooling
- [x] hyperparams: Conneau 3.3
    - [x] SGD
    - [x] learning rate 0.1
    - [x] weight decay 0.99
    - [x] at each epoch, we divide the learning rate by 5 if the dev accuracy decreases
    - [x] use mini-batches of size 64
    - [x] training is stopped when the learning rate goes under the threshold of 10e-5
    - [x] classifier
        - [x] MLP
        - [x] 1 hidden layer of 512 hidden units
- [x] evaluation
    - [x] SentEval: https://uva-slpl.github.io/ull/resources/practicals/practical3/senteval_example.ipynb
        [~] micro/macro metrics
- [x] [jupyter notebook](https://colab.research.google.com/drive/1z9ncZwkO7CEGNohcgw6YGBmNlkIQAZO3)
- [ ] surf sara cuda errors
- [ ] collect runs
- [x] visualization
    - [ ] debug single-row CSVs
    - [x] tensorboard: https://github.com/lanpa/tensorboardX
- [ ] paper
