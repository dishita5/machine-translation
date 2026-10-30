# Project Notebooks

This directory contains the Jupyter notebooks used for dataset preparation, model implementation, training, evaluation, and final comparison.

| Notebook | Description |
|---|---|
| `00_Dataset_Preparation.ipynb` | Dataset loading, preprocessing, and preparation |
| `01_Basic_Transformer.ipynb` | Custom Transformer implementation and evaluation |
| `02-mt5-clean.ipynb` | mT5 fine-tuning and OPUS-MT evaluation |
| `03-llama-translation.ipynb` | TinyLlama translation using QLoRA |
| `04-gemma-translation last.ipynb` | Gemma translation using QLoRA |
| `05-final-results-comparison.ipynb` | Final model comparison, metrics, and analysis |

## Execution Environment

The experiments were conducted using Python in Kaggle notebooks. GPU acceleration was used where required for model training.

## Notes

- Dataset preparation and model experiments are documented in the respective notebooks.
- The final comparison notebook consolidates the evaluation results.
- Some models use pretrained checkpoints, while others are trained or fine-tuned as described in their notebooks.
