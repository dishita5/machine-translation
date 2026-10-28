# English-to-Spanish Machine Translation

A comparative study of Transformer-based architectures and pretrained language models for English-to-Spanish translation, evaluating translation quality, computational cost, and parameter-efficient fine-tuning.

## Project Overview

This project investigates how different model architectures and adaptation strategies perform on English-to-Spanish machine translation. It compares a custom Transformer trained from scratch, a fully fine-tuned mT5-small model, a pretrained OPUS-MT baseline, and two decoder-only language models adapted using 4-bit QLoRA.

The experiments evaluate translation quality using **BLEU** and **chrF**, alongside training time, inference time, model size, and trainable parameter count.

## Models Evaluated

| Model | Architecture | Adaptation |
|---|---|---|
| Basic Transformer | Custom encoder-decoder Transformer | Trained from scratch |
| mT5-small | Encoder-decoder Transformer | Full fine-tuning |
| OPUS-MT | Marian encoder-decoder | Pretrained baseline; no additional fine-tuning |
| TinyLlama 1.1B | Decoder-only Transformer | 4-bit QLoRA |
| Gemma 2B IT | Decoder-only Transformer | 4-bit QLoRA |

## Dataset

The experiments use an English-Spanish parallel translation dataset containing:

- Training set: 20,000 examples
- Validation set: 1,000 examples
- Test set: 1,000 examples

The dataset contains English source text (`en`) and Spanish reference translations (`es`). Dataset files should be obtained from the original dataset source rather than committed to this repository unless redistribution is permitted.

## Main Results

The following are the recorded results from the main evaluation.

| Model | BLEU | chrF | Training Time (min) | Inference Time (sec/1000) | Total Parameters |
|---|---:|---:|---:|---:|---:|
| Basic Transformer | 13.0148 | 6.1098 | 2.54 | 73.141 | 4,783,232 |
| mT5-small | 1.5833 | 11.4274 | 26.89 | 124.400 | 300,176,768 |
| OPUS-MT | 42.5432 | 64.8982 | 0.00* | 43.270 | 77,943,296 |
| TinyLlama 1.1B | 23.8893 | 48.2915 | 62.96 | 1,150.440 | 1,102,301,184 |
| Gemma 2B IT | 29.4843 | 53.2853 | 51.92 | 656.920 | 2,515,978,240** |

\* OPUS-MT was evaluated without additional training; its original pretraining cost is not included.

\** Gemma's total parameter count uses the recorded PEFT-reported count for the quantized model.

### Trainable Parameters for QLoRA Models

| Model | Trainable LoRA Parameters |
|---|---:|
| TinyLlama 1.1B | 2,252,800 |
| Gemma 2B IT | 9,805,824 |

The trainable parameter count is distinct from total model parameters. In QLoRA, the base model is loaded in quantized form while the selected adapter parameters are trained.

## Key Findings

- **OPUS-MT** achieved the highest BLEU and chrF scores and the fastest measured inference among the evaluated systems.
- **Gemma 2B IT** outperformed TinyLlama 1.1B on both recorded translation metrics.
- The custom Transformer required the least additional training time but achieved weak translation scores.
- mT5-small recorded low BLEU and modest chrF under the specific fine-tuning configuration used.
- The results compare complete experimental systems; architecture alone cannot explain the differences because pretraining, tokenization, adaptation, decoding, and implementation also vary.

## Gemma Temperature and Sampling Experiment

A separate experiment evaluated three Gemma decoding configurations on a **100-example test subset**. These results are distinct from the main 1,000-example evaluation.

| Decoding | Temperature | Top-p | BLEU | chrF | Inference Time (sec/100 examples) |
|---|---:|---:|---:|---:|---:|
| Greedy | 0.0 | — | 29.4521 | 53.3279 | 66.44 |
| Sampling | 0.7 | 0.9 | 24.7170 | 48.5303 | 65.66 |
| Sampling | 1.0 | 0.9 | 21.6606 | 45.8957 | 64.09 |

Greedy decoding achieved the highest BLEU and chrF on this subset. Higher-temperature sampling was associated with lower reference-based scores in this experiment. These findings apply to the tested settings and subset; they do not establish that greedy decoding is universally superior.

## Evaluation Metrics

- **BLEU:** Measures n-gram overlap between generated translations and reference translations.
- **chrF:** Measures character n-gram similarity between generated translations and references.

Both metrics are useful automatic evaluation measures, but neither fully captures semantic adequacy, grammaticality, fluency, or meaning preservation.

## Experimental Considerations

- OPUS-MT used beam search with four beams; the other main evaluations used greedy decoding.
- Training and inference times reflect the recorded experimental environment and configurations.
- The main comparison uses 1,000 test examples, whereas the temperature/sampling experiment uses 100.
- Results from different test subset sizes should not be treated as directly equivalent.
- Automatic metrics should be supplemented with manual review when possible.

## Limitations and Ethical Considerations

Machine translation may introduce omissions, mistranslations, grammatical errors, or changes in meaning. Performance may also vary across domains, writing styles, and sentence lengths. The experiments use a specific dataset and configuration, so results may not generalize to every translation setting.

Translations should be reviewed by a qualified human when errors could have significant consequences. Dataset licensing, privacy, bias, and appropriate handling of sensitive text should be considered when deploying translation systems.

## Repository Structure

The repository will contain the project documentation, notebooks, source code where available, and selected evaluation results and visualizations.

```text
machine-translation/
├── README.md
├── .gitignore
├── requirements.txt
├── notebooks/
├── src/
├── data/
└── results/
```

This describes the intended organization; files should be added as they are prepared and uploaded.

## Reproducibility

The reported metrics are preserved from the completed experiments. Reproducing them requires the corresponding dataset, model configurations, code, dependency versions, and model access where applicable. The original model checkpoints and full experiment artifacts are maintained separately from this repository.

Do not commit access tokens, credentials, private data, or large model checkpoints to a public repository.

## Future Improvements

- Add source notebooks and scripts for each model.
- Include example translations and qualitative error analysis.
- Evaluate additional test examples and repeat experiments to measure variability.
- Add environment and dependency specifications.
- Compare translation quality across different decoding strategies and domains.

## Acknowledgements

This project was developed as part of an academic Generative AI assignment, exploring Transformer architectures, pretrained translation systems, and parameter-efficient fine-tuning.
