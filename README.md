# arabic-llm-quantisation
Reproducible Colab benchmark of FP16, GPTQ 4-bit and GPTQ 8-bit for Qwen2.5 on ArabicMMLU.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/khalid2423/arabic-llm-quantisation/blob/main/arabic_llm_quantisation_reproducible.ipynb)

## Reproduce the experiment

Open the notebook in Colab, select a T4 GPU under **Runtime → Change runtime type**, and run the cells in order. The notebook installs vLLM and GPTQModel, evaluates Qwen2.5-1.5B-Instruct in FP16, and creates and evaluates both GPTQ checkpoints.

## Results from a fresh Colab session

| Model | Median response time | Output throughput | Model-load GPU memory | Running-server GPU memory | ArabicMMLU accuracy |
|---|---:|---:|---:|---:|---:|
| FP16 | 2.17 s | 28.26 tokens/s | 2.98 GiB | 13,435 MiB | 12/30 (40.0%) |
| Custom GPTQ 4-bit | 2.86 s | 22.04 tokens/s | 1.10 GiB | 13,403 MiB | 3/30 (10.0%) |
| Custom GPTQ 8-bit | 2.88 s | 21.44 tokens/s | 1.74 GiB | 13,429 MiB | 12/30 (40.0%) |

On one Tesla T4, FP16 had the highest output throughput. Custom 8-bit reduced model-load GPU memory by about 42% and matched FP16's score on the 30-question sample. Custom 4-bit saved about 63% of model-load GPU memory and scored 3/30. Sixteen of its replies did not follow the single-letter answer format and were counted as incorrect.

Timing used five sequential Arabic prompts after one warm-up request. Accuracy was measured on a 30-question [ArabicMMLU](https://huggingface.co/datasets/MBZUAI/ArabicMMLU) sample. The running-server memory measurement includes vLLM allocations such as the KV cache.

## Report and charts

[Benchmark report and charts (HTML)](arabic-llm-quantisation-report.html)

Download the HTML file and open it in a browser to view the charts. The report shows the initial run, while the table above shows the rerun in a fresh Colab session.
