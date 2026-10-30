# English-to-Spanish Machine Translation

A comparative study of Transformer-based architectures and pretrained language models for English-to-Spanish machine translation, evaluating translation quality, computational cost, and parameter-efficient fine-tuning.

## 1. Project Overview

This project investigates how different Transformer architectures and model adaptation strategies perform on English-to-Spanish machine translation.

Five translation systems are evaluated:

1. A custom Basic Transformer trained from scratch.
2. mT5-small with full fine-tuning.
3. OPUS-MT as a pretrained translation baseline.
4. TinyLlama 1.1B Chat adapted using 4-bit QLoRA.
5. Gemma 2B IT adapted using 4-bit QLoRA.

The models are compared using BLEU and chrF scores, training or fine-tuning time, inference time, and model parameter counts.

The project also investigates how decoding temperature and sampling affect translation quality using Gemma.

## 2. Dataset

### OPUS-100: English-Spanish

The project uses the publicly available **OPUS-100 parallel corpus**, specifically its English-Spanish (`en-es`) language pair.

- **Dataset:** OPUS-100
- **Hugging Face dataset:** `Helsinki-NLP/opus-100`
- **Configuration:** `en-es`
- **Source language:** English
- **Target language:** Spanish

**Dataset source:** [OPUS-100 on Hugging Face](https://huggingface.co/datasets/Helsinki-NLP/opus-100)

### Dataset Split

A manageable subset of the parallel corpus was selected for the experiments.

| Split | Number of examples |
|---|---:|
| Training | 20,000 |
| Validation | 1,000 |
| Testing | 1,000 |
| **Total** | **22,000** |

Each example contains an English source sentence and its corresponding Spanish reference translation.

The dataset preparation notebook handles loading, selecting the language pair, preprocessing, and preparing the train, validation, and test sets.

Dataset files are not committed to the repository. They should be obtained from the original source when reproducing the experiments.

## 3. Models Evaluated

| Model | Architecture | Adaptation Method |
|---|---|---|
| Basic Transformer | Custom encoder-decoder Transformer | Trained from scratch |
| mT5-small | Encoder-decoder Transformer | Full fine-tuning |
| OPUS-MT | Marian encoder-decoder | Pretrained baseline; no additional fine-tuning |
| TinyLlama 1.1B | Decoder-only Transformer | 4-bit QLoRA |
| Gemma 2B IT | Decoder-only Transformer | 4-bit QLoRA |

### 3.1 Basic Transformer

A custom encoder-decoder Transformer was implemented and trained from scratch using the prepared parallel corpus.

The model uses a vocabulary of 16,000 tokens, an embedding dimension of 128, four attention heads, and two encoder and decoder layers.

### 3.2 mT5-small

The pretrained `google/mt5-small` model was fine-tuned for English-to-Spanish translation.

Full fine-tuning was used, updating the model parameters on the selected parallel dataset.

### 3.3 OPUS-MT

The pretrained `Helsinki-NLP/opus-mt-en-es` translation model was evaluated directly without additional fine-tuning.

It serves as a task-specific pretrained translation baseline.

### 3.4 TinyLlama 1.1B

The `TinyLlama/TinyLlama-1.1B-Chat-v1.0` decoder-only model was adapted using 4-bit QLoRA.

The method uses quantization and LoRA adapters to reduce the number of trainable parameters.

### 3.5 Gemma 2B IT

The `google/gemma-2b-it` decoder-only model was adapted using 4-bit QLoRA.

LoRA adapters were applied to selected attention and feed-forward projection layers.

## 4. Main Experimental Results

The following table presents the recorded results from the main evaluation using 1,000 test examples.

| Model | BLEU | chrF | Training Time (min) | Inference Time (sec/1000) | Total Parameters |
|---|---:|---:|---:|---:|---:|
| Basic Transformer | 13.0148 | 6.1098 | 2.54 | 73.141 | 4,783,232 |
| mT5-small | 1.5833 | 11.4274 | 26.89 | 124.400 | 300,176,768 |
| OPUS-MT | 42.5432 | 64.8982 | 0.00* | 43.270 | 77,943,296 |
| TinyLlama 1.1B | 23.8893 | 48.2915 | 62.96 | 1,150.440 | 1,102,301,184 |
| Gemma 2B IT | 29.4843 | 53.2853 | 51.92 | 656.920 | 2,515,978,240** |

\* OPUS-MT was evaluated without additional training. Its original pretraining cost is not included.

\** Gemma's total parameter count uses the recorded PEFT-reported count. Parameter-counting conventions for quantized models may vary.

### Trainable Parameters for QLoRA Models

| Model | Trainable Parameters |
|---|---:|
| TinyLlama 1.1B | 2,252,800 |
| Gemma 2B IT | 9,805,824 |

For QLoRA, the base model is loaded in quantized form while only the selected adapter parameters are trained. Therefore, trainable parameters and total parameters represent different quantities.

## 5. Results and Model Comparison

### 5.1 Translation Quality

- **OPUS-MT** achieved the highest BLEU score (42.5432) and chrF score (64.8982).
- **Gemma 2B IT** outperformed TinyLlama 1.1B on both BLEU and chrF.
- The custom Basic Transformer achieved a higher BLEU score than mT5-small, while mT5-small achieved a higher chrF score.
- mT5-small recorded low BLEU and modest chrF under the specific fine-tuning configuration used.

### 5.2 Computational Performance

- OPUS-MT had the fastest measured inference time at 43.270 seconds per 1,000 examples.
- The Basic Transformer had the shortest additional training time among the models trained in this experiment.
- TinyLlama had the longest measured inference time.
- Gemma was faster than TinyLlama in the recorded inference evaluation.

### 5.3 Architecture and Adaptation

The results illustrate differences between encoder-decoder and decoder-only approaches.

The custom Transformer was relatively inexpensive to train but had limited translation quality. The pretrained encoder-decoder OPUS-MT model performed strongly because it already had task-specific translation capabilities.

The decoder-only models benefited from QLoRA, which allowed adaptation with a comparatively small number of trainable parameters. However, their inference times were higher in the recorded setup.

The comparison is between complete experimental systems rather than a controlled comparison of architecture alone. Pretraining, tokenization, model size, adaptation strategy, decoding configuration, and implementation all influence the results.

For a more detailed discussion, see [`results/final_results_analysis.md`](results/final_results_analysis.md).

## 6. Gemma Temperature and Sampling Experiment

A separate experiment investigated the effect of decoding temperature and sampling on Gemma's translation quality.

This experiment used a **100-example test subset**, rather than the 1,000-example test set used for the main comparison.

| Decoding Strategy | Temperature | Top-p | BLEU | chrF | Inference Time (sec/100 examples) |
|---|---:|---:|---:|---:|---:|
| Greedy | 0.0 | — | 29.4521 | 53.3279 | 66.44 |
| Sampling | 0.7 | 0.9 | 24.7170 | 48.5303 | 65.66 |
| Sampling | 1.0 | 0.9 | 21.6606 | 45.8957 | 64.09 |

### Observations

- Greedy decoding achieved the highest BLEU and chrF scores.
- Sampling at temperature 0.7 produced lower scores than greedy decoding.
- Sampling at temperature 1.0 produced the lowest BLEU and chrF scores among the three configurations.
- The measured inference times were relatively close; the small differences do not establish that sampling is inherently faster.

For this experiment, greedy decoding provided the strongest reference-based translation results. These findings apply to the tested subset and settings and should not be generalized to every translation task.

Detailed results and visualizations are available in [`results/gemma_sampling/`](results/gemma_sampling/).

## 7. Evaluation Metrics

### BLEU

BLEU (Bilingual Evaluation Understudy) measures the overlap of n-grams between generated translations and reference translations, with a brevity penalty to account for overly short outputs.

Higher BLEU scores generally indicate greater similarity to the reference translations.

### chrF

chrF measures character-level n-gram similarity between generated translations and references.

It can capture character-level differences and morphological variations that word-level metrics may not fully represent.

Both metrics are useful automatic evaluation measures, but neither fully captures semantic adequacy, grammaticality, fluency, or meaning preservation.

## 8. Qualitative Analysis

Automatic metrics alone cannot fully determine whether a translation preserves the original meaning.

The generated translations should also be examined for:

- Meaning preservation and translation accuracy.
- Grammatical correctness and fluency.
- Missing or additional information.
- Repetition and incomplete outputs.
- Incorrect handling of negation, named entities, and context.

The Basic Transformer showed issues such as `<unk>` tokens and repetitive outputs. TinyLlama produced more coherent translations in some examples, although its outputs could still differ from the reference translations.

Selected examples are documented in [`results/translation_samples.md`](results/translation_samples.md).

Individual prediction files are included for the Basic Transformer and TinyLlama. The aggregate metrics and analysis are retained for the other model evaluations.

## 9. Ethical Considerations and Responsible AI

Machine translation systems can introduce errors that affect how information is interpreted.

Important concerns include:

### Bias

A model may produce translations that reflect biases in its training data, potentially changing the tone or representation of people and groups.

**Mitigation:** Evaluate translations across different topics, demographic references, and language varieties. Review potentially biased outputs with human evaluators.

### Hallucinated Information

A translation model may add information that was not present in the original sentence or omit important details.

**Mitigation:** Compare generated translations with their source sentences and use human review for important or high-impact content.

### Cultural Misrepresentation

Idioms, cultural references, and context-dependent expressions may be translated incorrectly when the model lacks the relevant cultural understanding.

**Mitigation:** Use context-aware evaluation and involve fluent speakers familiar with the relevant cultural context.

### Sensitive Content

Incorrect translation of medical, legal, or other sensitive information may lead to misunderstanding or inappropriate decisions.

**Mitigation:** Require qualified human review for high-impact translations and avoid treating machine-generated translations as authoritative without verification.

Further discussion and mitigation measures are provided in [`results/ethical_analysis.md`](results/ethical_analysis.md).

## 10. Repository Structure

The repository is organized into two main directories: `notebooks/` and `results/`.

The `notebooks/` directory contains the implementation and evaluation notebooks. The `results/` directory contains three model-specific or experiment-specific subdirectories, along with the overall comparison files, analysis, and visualizations.

```text
machine-translation/
│
├── README.md
├── .gitignore
│
├── notebooks/
│   ├── 00_Dataset_Preparation.ipynb
│   ├── 01_Basic_Transformer.ipynb
│   ├── 02-mt5-clean.ipynb
│   ├── 03-llama-translation.ipynb
│   ├── 04-gemma-translation last.ipynb
│   ├── 05-final-results-comparison.ipynb
│   └── README.md
│
└── results/
    │
    ├── basic_transformer/
    │   ├── README.md
    │   ├── basic_transformer_predictions.csv
    │   ├── basic_transformer_metrics.json
    │   ├── basic_transformer_training_history.json
    │   ├── basic_transformer_training_summary.json
    │   └── basic_transformer_loss_curve.png
    │
    ├── tinyllama/
    │   ├── README.md
    │   ├── llama_predictions.csv
    │   ├── llama_metrics.json
    │   ├── llama_training_summary.json
    │   └── llama_inference_summary.json
    │
    ├── gemma_sampling/
    │   ├── gemma_sampling_analysis.csv
    │   ├── gemma_sampling_analysis.md
    │   ├── gemma_sampling_bleu.png
    │   ├── gemma_sampling_chrf.png
    │   └── gemma_sampling_inference_time.png
    │
    ├── final_model_comparison_detailed.csv
    ├── final_results_analysis.md
    ├── comparison_bleu.png
    ├── comparison_chrf.png
    ├── comparison_training_time.png
    ├── comparison_inference_time.png
    ├── comparison_parameters.png
    ├── ethical_analysis.md
    └── translation_samples.md
```

## 11. Execution Instructions

The experiments were conducted using a GPU-enabled notebook environment. A Kaggle or Google Colab GPU runtime can be used, subject to available GPU memory and model access.

### Recommended Execution Order

1. **Dataset Preparation:** Run `00_Dataset_Preparation.ipynb` to load and prepare the OPUS-100 English-Spanish dataset.
2. **Basic Transformer:** Run `01_Basic_Transformer.ipynb` to implement and train the custom encoder-decoder Transformer.
3. **mT5 and OPUS-MT:** Run `02-mt5-clean.ipynb` to fine-tune mT5-small and evaluate the pretrained OPUS-MT baseline.
4. **TinyLlama:** Run `03-llama-translation.ipynb` to adapt TinyLlama using 4-bit QLoRA and generate translations.
5. **Gemma:** Run `04-gemma-translation last.ipynb` to adapt Gemma using 4-bit QLoRA and evaluate its translations.
6. **Final Comparison:** Run `05-final-results-comparison.ipynb` to consolidate the evaluation results and generate comparison visualizations.

The notebooks may require access to previously generated datasets, checkpoints, or result files. Follow the paths and setup instructions provided within each notebook.

### Hardware Requirements

- A GPU-enabled runtime is recommended, especially for mT5, TinyLlama, and Gemma.
- The decoder-only model experiments use 4-bit quantization to reduce memory requirements.
- Runtime and memory requirements depend on the GPU, batch size, sequence length, and model configuration.

### Model Access

Some pretrained checkpoints may require accepting their model-specific access or license conditions on Hugging Face. In particular, verify access to the Gemma checkpoint before running its notebook.

Do not upload access tokens or credentials to the public repository.

## 12. Dependencies

The notebooks use the following Python libraries and frameworks:

| Library | Purpose |
|---|---|
| PyTorch | Model implementation and training |
| Transformers | Pretrained Transformer models and tokenizers |
| Datasets | Loading and processing OPUS-100 |
| PEFT | LoRA and parameter-efficient fine-tuning |
| Accelerate | Model training and device management |
| BitsAndBytes | 4-bit quantization |
| SentencePiece | Tokenization support |
| Evaluate | Evaluation utilities |
| SacreBLEU | BLEU evaluation |
| NumPy | Numerical operations |
| Pandas | Dataset and result handling |
| Matplotlib | Result visualizations |

Install the required libraries in the notebook environment before execution. For example:

```bash
pip install torch transformers datasets peft accelerate bitsandbytes sentencepiece evaluate sacrebleu numpy pandas matplotlib
```

The exact compatible package versions may depend on the notebook runtime, CUDA version, and GPU environment.

## 13. Reproducibility and Limitations

The reported metrics are preserved from the completed experiments.

Reproducing the results requires the corresponding dataset, model configurations, code, dependency versions, hardware environment, and model access where applicable.

The following limitations should be considered:

- The models use different architectures, pretrained checkpoints, tokenizers, and adaptation methods.
- OPUS-MT uses beam search with four beams, whereas the other main evaluations use greedy decoding.
- Training times do not include the original pretraining costs of pretrained models.
- The main evaluation uses 1,000 test examples, while the Gemma sampling experiment uses 100 examples.
- Inference timings depend on the hardware, batch sizes, generation settings, and implementation.
- Automatic evaluation metrics cannot fully assess translation quality or meaning preservation.
- The conclusions apply to the specific dataset subset and experimental configurations used.

Original model checkpoints and large intermediate files are maintained separately from this repository.

## 14. Conclusion

Among the evaluated systems, **OPUS-MT was the best-performing model overall** for English-to-Spanish translation under the tested configurations.

It achieved the highest BLEU score of 42.5432 and chrF score of 64.8982, while also recording the fastest measured inference time.

Gemma 2B IT outperformed TinyLlama 1.1B on both translation metrics, while the custom Basic Transformer required the least additional training time but produced weaker translations.

The experiments demonstrate the importance of pretrained translation capabilities, model adaptation methods, architecture, and computational requirements when developing machine-translation systems.

Overall, the results suggest that a pretrained task-specific translation model can outperform larger general-purpose language models adapted through parameter-efficient fine-tuning for this particular translation task.

## 15. Acknowledgements

This project was developed as part of the **Foundations of Generative AI (ICT4403) Assignment 3**, exploring Transformer architectures, pretrained translation systems, and parameter-efficient fine-tuning.

Dataset: [OPUS-100 – Helsinki-NLP](https://huggingface.co/datasets/Helsinki-NLP/opus-100)

