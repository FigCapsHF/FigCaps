# FigCaps-HF: A Figure-to-Caption Generative Framework and Benchmark with Human Feedback

<!-- [[`Paper`]()]  [[`BibTex`]()]  -->
[[`Website`](https://figcapshf.github.io/)] [[`Benchmark Dataset`](https://figshare.com/s/c034fd77bea9475319cb)]

## Abstract
Captions are crucial for understanding scientific visualizations and documents.
Existing captioning methods for scientific figures rely on figure-caption pairs extracted from documents for training, many of which fall short with respect to metrics like helpfulness, explainability, and visual-descriptiveness \cite{summaries-as-captions-preprint} leading to generated captions being misaligned with reader preferences. 
To enable the generation of high-quality figure captions, we introduce \textbf{FigCaps-HF} a new framework for figure-caption generation that can incorporate domain expert feedback in generating captions optimized for reader preferences. 
Our framework comprises of 1) an automatic method for evaluating quality of figure-caption pairs, 2) a novel reinforcement learning with human feedback (RLHF) method to optimize a generative figure-to-caption model for reader preferences.
We demonstrate the effectiveness of our simple learning framework by improving performance over standard fine-tuning across different types of models. 
In particular, when using BLIP as the base model, our RLHF framework achieves a mean gain of 35.7\%, 16.9\%, and 9\% in ROUGE, BLEU, and Meteor, respectively. 
Finally, we release a large-scale benchmark dataset with human feedback on figure-caption pairs to enable further evaluation and development of RLHF techniques for this problem.

| Model            | Parameters | ROUGE-L | BLEU   | Meteor |
|------------------|------------|---------|--------|--------|
| BLIP             | 0.25B      | 0.130   | 0.014  | 0.132  |
| Ours-BLIP-RLHF   | 0.25B      | 0.152   | 0.019  | 0.145  |

## Benchmark Dataset

Number of Figures in Each Subset

|                         |  Train  | Validate |  Test  |
|------------------------:|:-------:|:--------:|:------:|
| Benchmark               | 106,834 |  13,354  | 13,355 |

Additionally, we include the human evaluations of 439 figure-caption pairs as a CSV in the dataset. These evaluations consist of ratings, for each image-caption pair, of the “helpfulness”, “OCR (quality)”, “takeaway” and “visual (descriptiveness)”, from which we learn about human feedback for figure-caption pairs. 

<!-- ## Retrieving the dataset
Our benchmark dataset can be downloaded from [[`here`](https://figshare.com/s/c034fd77bea9475319cb)]. -->

## Installation 
We first need to clone this repository, install the requirements, and download the benchmark dataset
```shell
pip install --upgrade pip
git clone https://github.com/FigCapsHF/FigCapsHF
pip install -r requirements.txt
wget  https://figshare.com/ndownloader/files/41222934 -O benchmark.zip
unzip benchmark.zip
```
## Example Usage

## RLHF Fine-tuning
```python
python train_blip.py --mixed_precision fp16 --hf_score_type helpfulness --benchmark_path XX/benchmark
```
## Inference
```python
python inference.py --figure_path /path/test_image.png --model_path /path/model.pth
```
## Human Feedback Generation
```python
from FigCapsHF import FigCapsHF
FigCapsHF = FigCapsHF("path/to/benchmark/data")
inferred_hf_df = FigCapsHF.infer_hf_training_set(hf_score_type = "helpfulness", embedding_model = "BERT", max_num_samples = 100, quantization_levels = 3, mapped_hf_labels = ["Bad", "Neutral", "Good"])
```

## License

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
  
This dataset uses data in the [arXiv dataset](https://www.kaggle.com/Cornell-University/arxiv).
The [arXiv dataset](https://www.kaggle.com/Cornell-University/arxiv) uses the [CC0 1.0 Universal (CC0 1.0) Public Domain Dedication license](https://creativecommons.org/publicdomain/zero/1.0/) for the metadata, which grants permission to remix, remake, annotate, and publish the metadata.

<!-- Another way for Training. -->
<!-- Here we are using BLIP as a sample model for training using Pytorch's native DataLoader library combined with Huggingface's dataset class. It also has a training loop. User can provide arguments for their desired functionality as shown in the script below. To change the the number of epochs and learning rate, modify config variable in train_blip.py.
```shell
python train_blip.py --f16 --output_dir output

```

## Inference
（after downloading and setting up the dataset) To predict a scientific caption from an scientific image, and if the user desire to use other models for inference, please make the modification in inference.py. Below, we will provide a pre-trained BLIP model trained on this benchmark dataset and make simple inference using the model provided.
```shell 
./pred.sh
cd BLIP
```
Now use this [link](https://drive.google.com/file/d/1FZh95Xeyt3RlaYs_TeeiiSPwYvAuGogQ/view?usp=share_link) to download our model, and place it under directory BLIP, and use the following script to do an inference on the sample.png
```shell
python inference.py sample.png
```
If running on a CPU, the expected result is *the results of comparing oa and noa in terms of mean of error.* (on seed 42).

![Sample Scientific figure](/Figures/sample.png)  -->
