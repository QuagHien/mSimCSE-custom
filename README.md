## mSimCSE
Multilingual Contrastive Learning for transformer models in an encoder-based architecture. This is the official implementation of the paper [English Contrastive Learning Can Learn Universal Cross-lingual Sentence Embeddings](https://arxiv.org/pdf/2211.06127.pdf). Our model is a multilingual version of [SimCSE](https://arxiv.org/abs/2104.08821) which maps cross-lingual sentences into a shared embedding space. Our implementation is mainly based on official [SimCSE repository](https://github.com/princeton-nlp/SimCSE). Our model can be used for cross-lingual retrieval/mining and cross-lingual sentence embeddings evaluation.

## My checkpoints:
   Hugging Face: wanhin/msimcse-xlm-roberta-base-vien-envi 
   
   (base model: FacebookAI/xlm-roberta-base)
   
   Hugging Face: wanhin/msimcse-mt5-luat-at
   
   (base model: google/mt5-base)
   
## Getting Started:
### Step 1: Build virtual environment.
```bash
conda create -n mSimCSE python=3.8
conda activate mSimCSE
```

### Step 2: Install Packages
Before install requirements.txt, install [pytorch](https://pytorch.org/get-started/locally/) from the official website. We test our model on pytorch LTS(1.8.2). It should also work on a later version.
```bash
git clone https://github.com/QuagHien/mSimCSE.git
cd mSimCSE
pip install -r requirements.txt
```

### Step 3: Download Data for training and testing
For English NLI training, we directly use the NLI data preprocessed by the [SimCSE repository](https://github.com/princeton-nlp/SimCSE). We use the preprocess script of [XTREME](https://github.com/google-research/xtreme/blob/master/scripts/download_data.sh) to download and preprocess BUCC2018. The tatoeba dataset is downloaded from [LASER](https://github.com/facebookresearch/LASER/tree/main/data/tatoeba/v1) and has been put into the data directory.

```bash
cd data
chmod +x download_nli.sh
./download_nli.sh

git clone https://github.com/facebookresearch/SentEval.git
cd ..
mv data/SentEval/data SentEval/

cd SentEval/data/downstream
chmod +x get_transfer_data.bash
./get_transfer_data.bash
cd ../../..
```

## Training and Testing
### Training:
Our model requires 40GB memory for training. Notice that our code doesn't support multi-gpu training, so please specify a GPU to use by "CUDA_VISIBLE_DEVICES=GPUID" prefix.  
For English NLI training:
```bash
chmod +x train_english.sh
./train_english.sh
```
For cross-lingual NLI:
```bash
chmod +x train_cross.sh
./train_cross.sh
```
Notice that in cross-lingual NLI training, using a larger batch size and larger epoch number decreases the performance because our implementation sometimes puts cross-lingual sentences with the same meaning into the same batch. Using a smaller batch size reduces the chance of putting identical cross-lingual sentences into the same batch and thus improving the performance.


### Testing:
We evaluate model performance on cross-lingual retrieval (BUCC and Tatoeba) and multi-lingual STS tasks. The "model_dir" denotes the "output_dir" in the training script. 
```bash
./eval.sh [model_dir]
```
### Pre-trained Model:
Our pre-trained model is available at [here](https://huggingface.co/yaushian/mSimCSE). For pre-trained cross-lingual model trained on English NLI, please download model [here](https://huggingface.co/yaushian/mSimCSE/resolve/main/xlm-roberta-large-mono_en.zip). For pre-trained cross-lingual model trained on cross-lingual NLI, please download model [here](https://huggingface.co/yaushian/mSimCSE/resolve/main/xlm-roberta-large-cross_all.zip).  
To evaluate pre-trained models, please run:
```bash
cd results
./download_model.sh
cd ..
./eval.sh results/xlm-roberta-large-mono_en 
./eval.sh results/xlm-roberta-large-cross_all
```

## Citation

Please cite our paper if you use mSimCSE in your work:

```bibtex
@inproceedings{msimcse,
   title={English Contrastive Learning Can Learn Universal Cross-lingualSentence Embeddings},
   author={Yau-Shian Wang and Ashley Wu and Graham Neubig},
   booktitle={Empirical Methods in Natural Language Processing (EMNLP)},
   year={2022}
}
```
