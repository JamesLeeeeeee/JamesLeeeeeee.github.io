---
layout: post
title: bitsandbytes 에러 해결 libbitsandbytes libcusparse.so
categories: BNB_CUDA_VERSION libbitsandbytes CUDA bitsandbytes
description: bitsandbytes 에러 해결 libbitsandbytes 설치 후 python -m bitsandbytes 에러
keywords: BNB_CUDA_VERSION libbitsandbytes CUDA bitsandbytes llama quantinzation load_in_8bit
---



![image](https://github.com/user-attachments/assets/ac350958-4881-41d7-a068-75ab87bb8bd6)

llama3를 테스트 하려는데 bitsandbytes를 import시 에러가 났다.

![image](https://github.com/user-attachments/assets/372032ea-d28a-4000-8297-db4cd7d9692e)

바로 여기서 load_in_8bit를 할 때.

~~~python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, TrainingArguments, BitsAndBytesConfig
from datasets import load_dataset
from peft import LoraConfig, get_peft_model, prepare_model_for_kbit_training
from trl import SFTTrainer

base_model_name = "meta-llama/Meta-Llama-3.1-8B-Instruct"

quantization_config = BitsAndBytesConfig(load_in_8bit=True,
                                         llm_int8_threshold=200.0)


base_model = AutoModelForCausalLM.from_pretrained(
    base_model_name, 
    device_map="auto", 
    quantization_config=quantization_config,
)

# base_model.config.use_cache = False

tokenizer = AutoTokenizer.from_pretrained(base_model_name)
tokenizer.pad_token = tokenizer.eos_token
tokenizer.padding_side = "right"
~~~


~~~bash
git clone https://github.com/TimDettmers/bitsandbytes.git
cd bitsandbytes
cmake -DCOMPUTE_BACKEND=cuda -S .
~~~
* 직접 깃클론으로 가져와 cmake를 하고

![image](https://github.com/user-attachments/assets/abb067a3-ce33-4648-a3a1-2bc14b0ba34f)

이 부분이 있어서 현재 cuda version을 적어서 다시 make 했다.
~~~bash
make CUDA_VERSION=118
~~~

![image](https://github.com/user-attachments/assets/91d9258e-d790-402b-a3f1-27f638a7a6a0)

기분 좋게 마지막에 successful이 떴다

