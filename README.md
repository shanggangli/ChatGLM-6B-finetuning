
## ChatGLM-6b-int4-微调
本项目主要针对ChatGLM-6B模型进行不同方式的微调(freeze\embedding\PT\LoRA)，并对比大模型在不同微调方法上的效果，主要针对信息抽取任务、生成任务、分类任务等。
## 微调
### Freeze方法
Freeze方法：仅原始模型的参数冻结,如可以仅训练模型的后面的layer。\
最终训练的参数如下：\
训练的参数：
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
### embedding方法
embedding方法：将模型全部冻结，仅训练模型ebedding部分，soft prompt方式之一。\
训练的参数：
trainable params: 0.53B || all params: 3.356B || trainable%: 15.9

```
be train layer: transformer.word_embeddings.weight
```
### PT方法
PT方法：即[P-tuning-V2](https://github.com/THUDM/P-tuning-v2) 其实是soft prompt的一种改进,p tuning v2则不只是针对embedding层，而是将连续型token插入每一层，增大改变量和交互性。
![](https://github.com/THUDM/P-tuning-v2/blob/main/figures/P-tuning-v2.png)
训练的参数：
trainable params: 0.957B || all params: 4.312B || trainable%: 22.18

```
transformer.prefix_encoder.embedding.weight
transformer.prefix_encoder.trans.0.weight
transformer.prefix_encoder.trans.0.bias
transformer.prefix_encoder.trans.2.weight
transformer.prefix_encoder.trans.2.bias
```
### LoRA方法
待更新

## Loss
Freeze loss\
<img src="https://github.com/shanggangli/ChatGLM-6B-int4-finetuning/blob/main/image/int4_finetuning_freeze%20Loss.jpg" width="800"/>

embedding loss\
<img src="https://github.com/shanggangli/ChatGLM-6B-int4-finetuning/blob/main/image/int4_finetuning-embed%20Loss.jpg" width="800"/>

PT loss\
<img src="https://github.com/shanggangli/ChatGLM-6B-int4-finetuning/blob/main/image/in4_finetuning_pt%20Loss.jpg" width="800"/>
