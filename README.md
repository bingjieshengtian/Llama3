---
license: Apache License 2.0
---
### Clone with HTTP
```bash
git clone https://www.modelscope.cn/FlagAlpha/Llama3-Chinese-8B-Instruct.git
```

# Llama3-Chinese-8B 

Llama3-Chinese-8B基于Llama3-8B的中文对话模型，由Llama中文社区和AtomEcho（原子回声）联合研发，我们会持续提供更新的模型参数，模型训练过程见(https://llama.family)。

模型的部署、训练、微调等方法详见Llama中文社区GitHub仓库：https://github.com/LlamaFamily/Llama-Chinese

## 在线体验

https://llama.family/chat/#/

## 如何使用

下载模型
```
git clone https://www.modelscope.cn/FlagAlpha/Llama3-Chinese-8B-Instruct.git
```

使用
```
import transformers
import torch


model_id = "./Llama3-Chinese-8B-Instruct"

pipeline = transformers.pipeline(
    "text-generation",
    model=model_id,
    model_kwargs={"torch_dtype": torch.float16},
    device="cuda",
)


messages = [{"role": "system", "content": ""}]

messages.append(
                {"role": "user", "content": "介绍一下机器学习"}
            )

prompt = pipeline.tokenizer.apply_chat_template(
        messages, 
        tokenize=False, 
        add_generation_prompt=True
    )

terminators = [
        pipeline.tokenizer.eos_token_id,
        pipeline.tokenizer.convert_tokens_to_ids("<|eot_id|>")
    ]
outputs = pipeline(
    prompt,
    max_new_tokens=512,
    eos_token_id=terminators,
    do_sample=True,
    temperature=0.6,
    top_p=0.9
)

content = outputs[0]["generated_text"][len(prompt):]

print(content)
```
