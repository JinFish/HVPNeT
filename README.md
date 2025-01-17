# HVPNet

Code for the NAACL2022 (Findings) paper "[Good Visual Guidance Makes A Better Extractor: Hierarchical Visual Prefix for Multimodal Entity and Relation Extraction](https://arxiv.org/pdf/2205.03521.pdf)".

Model Architecture
==========
<div align=center>
<img src="resource/model.png" width="80%" height="80%" />
</div>
The overall architecture of our hierarchical modality fusion network.


Requirements
==========
To run the codes, you need to install the requirements:
```
pip install -r requirements.txt
```

Data Collection
==========
The datasets that we used in our experiments are as follows:


+ Twitter2015 & Twitter2017
    
    The text data follows the conll format. You can download the Twitter2015 data via this [link](https://drive.google.com/file/d/1qAWrV9IaiBadICFb7mAreXy3llao_teZ/view?usp=sharing) and download the Twitter2017 data via this [link](https://drive.google.com/file/d/1ogfbn-XEYtk9GpUECq1-IwzINnhKGJqy/view?usp=sharing). Please place them in `data/NER_data`.

    You can also put them anywhere and modify the path configuration in `run.py`

+ MNER
    
    The MRE dataset comes from [MEGA](https://github.com/thecharm/MNRE) and you can download the MRE dataset with detected visual objects using folloing    
    command:
    ```bash
    cd data
    wget 120.27.214.45/Data/re/multimodal/data.tar.gz
    tar -xzvf data.tar.gz
    mv data RE_data
    ```


Data Preprocess
==========
To extract visual object images, we first use the NLTK parser to extract noun phrases from the text and apply the [visual grouding toolkit](https://github.com/zyang-ur/onestage_grounding) to detect objects. The detected objects are available in our data links.


The expected structure of files is:


```
HMNeT
 |-- data	# conll2003, mit-movie, mit-restaurant and atis
 |    |-- NER_data
 |    |    |-- twitter2015  # text data
 |    |    |    |-- train.txt
 |    |    |    |-- valid.txt
 |    |    |    |-- test.txt
 |    |    |    |-- twitter2015_train_dict.pth  # {full-image-[object-image]}
 |    |    |    |-- ...
 |    |    |-- twitter2015_images       # full image data
 |    |    |-- twitter2015_aux_images   # object image data
 |    |    |-- twitter2017
 |    |    |-- twitter2017_images
 |    |-- RE_data
 |    |    |-- ...
 |-- models	# models
 |    |-- bert_model.py
 |    |-- modeling_bert.py
 |-- modules
 |    |-- metrics.py    # metric
 |    |-- train.py  # trainer
 |-- processor
 |    |-- dataset.py    # processor, dataset
 |-- logs     # code logs
 |-- run.py   # main 
 |-- run_ner_task.sh
 |-- run_re_task.sh
```

Train
==========

## NER Task

The data path and GPU related configuration are in the `run.py`. To train ner model, run this script.

```shell
bash run_twitter15.sh
bash run_twitter17.sh
```

checkpoints can be download via [Twitter15_ckpt](https://drive.google.com/file/d/1E6ed_V2aGAPLExYAF3C3G8j8Td0v-7BM/view?usp=sharing), [Twitter17_ckpt](https://drive.google.com/file/d/1sgsjx_JVMYfu-_95e_3hB8tTR-NUiD27/view?usp=sharing).

## RE Task

To train re model, run this script.

```shell
bash run_re_task.sh
```
checkpoints can be download via [re_ckpt](https://drive.google.com/file/d/1x-yPYy8pjhsDzhhLLzLzEjyVFeQ063HM/view?usp=sharing)

Test
==========
## NER Task

To test ner model, you can download the model chekpoints we provide via [Twitter15_ckpt](https://drive.google.com/file/d/1E6ed_V2aGAPLExYAF3C3G8j8Td0v-7BM/view?usp=sharing), [Twitter17_ckpt](https://drive.google.com/file/d/1sgsjx_JVMYfu-_95e_3hB8tTR-NUiD27/view?usp=sharing) or use your own tained model and set `load_path` to the model path, then run following script:

```shell
python -u run.py \
      --dataset_name="twitter15/twitter17" \
      --bert_name="bert-base-uncased" \
      --seed=1234 \
      --only_test \
      --max_seq=80 \
      --use_prompt \
      --prompt_len=4 \
      --sample_ratio=1.0 \
      --load_path='your_ner_ckpt_path'

```

## RE Task

To test re model, you can download the model chekpoints we provide via [re_ckpt](https://drive.google.com/file/d/1x-yPYy8pjhsDzhhLLzLzEjyVFeQ063HM/view?usp=sharing) or use your own tained model and set `load_path` to the model path, then run following script:

```shell
python -u run.py \
      --dataset_name="MRE" \
      --bert_name="bert-base-uncased" \
      --seed=1234 \
      --only_test \
      --max_seq=80 \
      --use_prompt \
      --prompt_len=4 \
      --sample_ratio=1.0 \
      --load_path='your_re_ckpt_path'

```

Acknowledgement
==========

The acquisition of Twitter15 and Twitter17 data refer to the code from [UMT](https://github.com/jefferyYu/UMT/), many thanks.

The acquisition of MNRE data for multimodal relation extraction task refer to the code from [MEGA](https://github.com/thecharm/Mega), many thanks.

Papers for the Project & How to Cite
==========


If you use or extend our work, please cite the paper as follows:

```bibtex
@article{DBLP:journals/corr/abs-2205-03521,
  author    = {Xiang Chen and
               Ningyu Zhang and
               Lei Li and
               Yunzhi Yao and
               Shumin Deng and
               Chuanqi Tan and
               Fei Huang and
               Luo Si and
               Huajun Chen},
  title     = {Good Visual Guidance Makes {A} Better Extractor: Hierarchical Visual
               Prefix for Multimodal Entity and Relation Extraction},
  journal   = {CoRR},
  volume    = {abs/2205.03521},
  year      = {2022},
  url       = {https://doi.org/10.48550/arXiv.2205.03521},
  doi       = {10.48550/arXiv.2205.03521},
  eprinttype = {arXiv},
  eprint    = {2205.03521},
  timestamp = {Wed, 11 May 2022 17:29:40 +0200},
  biburl    = {https://dblp.org/rec/journals/corr/abs-2205-03521.bib},
  bibsource = {dblp computer science bibliography, https://dblp.org}
}
```
