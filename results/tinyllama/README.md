# TinyLlama 1.1B – English-to-Spanish Translation

## 1. Overview

This experiment evaluates **TinyLlama 1.1B Chat**, a decoder-only Transformer model, for English-to-Spanish machine translation.

The model was adapted using **4-bit Quantized Low-Rank Adaptation (QLoRA)** on the OPUS-100 English-Spanish parallel dataset. This approach allows parameter-efficient fine-tuning while keeping the base model quantized.

## 2. Model Details

| Configuration | Value |
|---|---|
| Model | `TinyLlama/TinyLlama-1.1B-Chat-v1.0` |
| Architecture | Decoder-only Transformer |
| Adaptation | 4-bit QLoRA |
| Quantization | 4-bit NF4 with double quantization |
| Compute data type | FP16 |
| Training epochs | 1 |
| Training examples | 20,000 |
| Validation examples | 1,000 |
| Test examples | 1,000 |
| Maximum sequence length | 256 |
| Batch size | 2 |
| Gradient accumulation steps | 8 |
| Learning rate | 2e-4 |
| Decoding strategy | Greedy decoding |
| Maximum new tokens | 96 |

## 3. Training and Evaluation Results

| Metric | Result |
|---|---:|
| BLEU | 23.8893 |
| chrF | 48.2915 |
| Training time | 62.96 minutes |
| Inference time | 1,150.44 seconds per 1,000 examples |
| Total parameters | 1,102,301,184 |
| Trainable parameters | 2,252,800 |

Only the LoRA adapter parameters were trained. The base model was loaded in 4-bit quantized form.

**Note:** The training summary records approximately 62.96 minutes, while the metrics file contains a slightly different training-time value. The training summary value is reported here; the original experiment files are retained unchanged.

## 4. Observations

- QLoRA enabled fine-tuning without updating the entire base model.
- TinyLlama achieved a BLEU score of 23.8893 and a chrF score of 48.2915.
- Its inference time was considerably higher than the encoder-decoder models in this experiment.
- TinyLlama performed below Gemma 2B IT and the pretrained OPUS-MT baseline on both BLEU and chrF.
- The results demonstrate that parameter-efficient fine-tuning reduces the number of trainable parameters, but does not necessarily provide fast inference.

## 5. Files in This Folder

| File | Description |
|---|---|
| `llama_predictions.csv` | English inputs, reference Spanish translations and TinyLlama-generated translations for the 1,000-example test set |
| `llama_metrics.json` | Recorded evaluation metrics |
| `llama_training_summary.json` | Model configuration, training details, parameter counts and training time |
| `llama_inference_summary.json` | Inference configuration and timing information |

## 6. Experimental Context

The experiment used the OPUS-100 English-Spanish parallel dataset, with 20,000 training examples, 1,000 validation examples and 1,000 test examples.

The results are specific to the model, dataset subset, hardware, fine-tuning configuration and decoding settings used in this project. They should not be interpreted as a general benchmark of all LLaMA-family models.

For the overall comparison with the Basic Transformer, mT5, OPUS-MT and Gemma, refer to [`../final_results_analysis.md`](../final_results_analysis.md).

