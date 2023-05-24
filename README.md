
## ChatGLM-6b-int4
This project focuses on the fine tuning of [ChatGLM-6B-int4 model](https://huggingface.co/THUDM/chatglm-6b-int4) in different ways (freeze\embeding\PT\LoRA), and comparing the effect of different fine tuning methods on the large model, mainly for information extraction task, generation task, classification task, etc.
And if you fine tuning other version of ChatGLM-6B(like [pf16](https://huggingface.co/THUDM/chatglm-6b)), you need to upate the 
## Fine Tuning
### Freeze Tuning
    the parameters of the original model are frozen. For example, only the layer behind the model can be trained.\
\
The parameters of the final training are as follows：\
trainable params: 81920 || all params: 3.356B || trainable%: 0.0024
```
be train layer: transformer.layers.23.input_layernorm.weight
be train layer: transformer.layers.23.input_layernorm.bias
be train layer: transformer.layers.23.post_attention_layernorm.weight
be train layer: transformer.layers.23.post_attention_layernorm.bias
be train layer: transformer.layers.24.input_layernorm.weight
be train layer: transformer.layers.24.input_layernorm.bias
be train layer: transformer.layers.24.post_attention_layernorm.weight
be train layer: transformer.layers.24.post_attention_layernorm.bias
be train layer: transformer.layers.25.input_layernorm.weight
be train layer: transformer.layers.25.input_layernorm.bias
be train layer: transformer.layers.25.post_attention_layernorm.weight
be train layer: transformer.layers.25.post_attention_layernorm.bias
be train layer: transformer.layers.26.input_layernorm.weight
be train layer: transformer.layers.26.input_layernorm.bias
be train layer: transformer.layers.26.post_attention_layernorm.weight
be train layer: transformer.layers.26.post_attention_layernorm.bias
be train layer: transformer.layers.27.input_layernorm.weight
be train layer: transformer.layers.27.input_layernorm.bias
be train layer: transformer.layers.27.post_attention_layernorm.weight
be train layer: transformer.layers.27.post_attention_layernorm.bias
```
### Embedding Tuning
    Freeze the model entirely and train only the ebedding part of the model as one of the soft prompt ways.\
\
The parameters of the final training are as follows：\
trainable params: 0.53B || all params: 3.356B || trainable%: 15.9

```
be train layer: transformer.word_embeddings.weight
```
### P Tuning
    P Tuning [P-tuning-V2](https://github.com/THUDM/P-tuning-v2) A soft prompt improvement,P-tuning-V2 is not only for the embedding layer, but continuous tokens are inserted into each layer, increasing the amount of change and interaction.
![](https://github.com/THUDM/P-tuning-v2/blob/main/figures/P-tuning-v2.png)\
\
The parameters of the final training are as follows：\
trainable params: 0.957B || all params: 4.312B || trainable%: 22.18

```
transformer.prefix_encoder.embedding.weight
transformer.prefix_encoder.trans.0.weight
transformer.prefix_encoder.trans.0.bias
transformer.prefix_encoder.trans.2.weight
transformer.prefix_encoder.trans.2.bias
```
### LoRA Tuning
[LoRA](https://arxiv.org/pdf/2106.09685.pdf) allows us to train some dense layers in a neural network indirectly by optimizing rank decomposition matrices of the dense layers’ change during
adaptation instead, while keeping the pre-trained weights frozen.\
![](https://github.com/shanggangli/ChatGLM-6B-int4-finetuning/blob/main/image/LoRA.png)\

## experiment
Fine tuning the model in **Google Colab pro** with **A100-40G**
So you need to pip install somethings in **Colab**:
```
!pip install --upgrade tensorboard
!pip install --upgrade protobuf
!pip install transformers
!pip install sentencepiece
!pip install deepspeed
!pip install mpi4py
!pip install cpm_kernels
!pip install icetk
!pip install peft
!pip install tensorboard
!pip install tqdm
```
## Loss
Freeze loss\
<img src="https://github.com/shanggangli/ChatGLM-6B-int4-finetuning/blob/main/image/int4_finetuning_freeze%20Loss.jpg" width="800"/>

embedding loss\
<img src="https://github.com/shanggangli/ChatGLM-6B-int4-finetuning/blob/main/image/int4_finetuning-embed%20Loss.jpg" width="800"/>

PT loss\
<img src="https://github.com/shanggangli/ChatGLM-6B-int4-finetuning/blob/main/image/in4_finetuning_pt%20Loss.jpg" width="800"/>
