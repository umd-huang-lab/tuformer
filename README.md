# Tuformer: Data-drive Design of Transformers for Improved Generalization or Efficiency

Xiaoyu Liu, Jiahao Su, Furong huang

Link to our paper on [arXiv].

[arXiv]: https://openreview.net/forum?id=V0A5g83gdQ_

### Abstract

Transformers are neural network architectures that achieve remarkable performance in many areas. However, the core component of Transformers, multi-head self- attention (MHSA), is mainly derived from heuristics, and the interactions across its components are not well understood. However, the core component of Transformers, multi-head self- attention (MHSA), is mainly derived from heuristics, and the interactions across its components are not well understood. To address the problem, we first introduce a mathematically rigorous and yet intuitive tensor diagram representation of MHSA. Guided by tensor diagram representations, we propose a novel design, namely Tunable Transformers (Tuformers), by allowing data-driven weights across heads, whereas MHSA adopts pre-defined and fixed weights across heads, as will be explained in our paper. Tuformers naturally reveal a flexible design space that a user, depending on the needs, can choose a structure that has either improved performance (generalization error) or higher model efficiency. Any pre-trained Transformer can be an initialization of the corresponding Tuformer with trainable number of heads for efficient training and fine-tuning. Tuformers universally outperform Transformers on various tasks across multiple domains under a wide range of model sizes.

---

### Overview

We address the problem in the following steps

#### Step 1: Better Representation - Tensor Diagram

- A visually intuitive and mathematically rigorous representation.
- Orientation invarant, obtains universally representation no matter how you represent the data objects (in row vectors or column vectors).
- Clear information flow among components.

<img src="https://github.com/umd-huang-lab/tuformer/blob/main/img/attention-module.png" width="320"><img src="https://i.imgur.com/7qHW4qz.png" width="320">





#### Step 2: Propose a data-drive structure: Tuformer

In MHSA, the **weights are collaborating in the form of a kronecker product** following the heuristics. Then it is natural to make the $$C$$ fully trainable for higher expressive power.

<img src="https://i.imgur.com/N1XzFzO.png" width="320"><img src="https://i.imgur.com/1Y8Jf4k.png" width="320">


- the number of heads generalizes to the stable rank of the fully trainable component, making the structure data-driven.
- can be initialized with any pre-trained models.
- universally outperform vanilla Transformers.
- plug-and-play, compatible with efficient Transformer designs using kernel tricks for linear complexity.

#### Step 3: Identify a design space and possible more efficient/general variations


<img src="https://i.imgur.com/bzOLcLK.png" width="320"><img src="https://i.imgur.com/jqwfE1A.png" width="320">

<img src="https://i.imgur.com/XHY6Wb2.png" width="400">




#### Step 4: Expressive power as a tool to guide the design in the space

- Models with higher expressive power in the design space have better generalization.
- Models can also be made very efficient sacrificing some expressive power.
- Structually more expressive design performs way beyond naivelly increasing number of parameters.

---
### Instructions

Code are based on fair-seq implementation of MHSA.

To run tuformers, register tuformer module to any Transformer related architectures.

For example, to perform NMT task on tuformer version of iwslt_de_en model, run commands below. Other setup remains the same.

```python=

CUDA_VISIBLE_DEVICES=0 fairseq-train \
    data-bin/iwslt14.tokenized.de-en \
    --arch tuformer_iwslt_de_en --share-decoder-input-output-embed \
    --optimizer adam --adam-betas '(0.9, 0.98)' --clip-norm 0.0 \
    --lr 5e-4 --lr-scheduler inverse_sqrt --warmup-updates 4000 \
    --dropout 0.3 --weight-decay 0.0001 \
    --criterion label_smoothed_cross_entropy --label-smoothing 0.1 \
    --max-tokens 4096 \
    --eval-bleu \
    --eval-bleu-args '{"beam": 5, "max_len_a": 1.2, "max_len_b": 10}' \
    --eval-bleu-detok moses \
    --eval-bleu-remove-bpe \
    --eval-bleu-print-samples \
    --best-checkpoint-metric bleu --maximize-best-checkpoint-metric

```

---

### Citation
Please cite our paper as

<pre>
@inproceedings{liu2021tuformer,
               title={Tuformer: Data-driven Design of Transformers for Improved Generalization or Efficiency},
               author={Liu, Xiaoyu and Su, Jiahao and Huang, Furong},
               booktitle={International Conference on Learning Representations},
               year&={2021}
}
</pre>
