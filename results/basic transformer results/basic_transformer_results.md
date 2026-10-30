# Basic Transformer — Experimental Results

## Overview

This directory contains the experimental artifacts for the custom encoder–decoder Transformer developed and trained from scratch for English-to-Spanish machine translation using the OPUS-100 dataset.

## Model Configuration

- **Architecture:** Custom Encoder–Decoder Transformer
- **Training approach:** Trained from scratch
- **Vocabulary size:** 16,000
- **Maximum sequence length:** 64
- **Model dimension:** 128
- **Attention heads:** 4
- **Encoder/decoder layers:** 2
- **Feed-forward dimension:** 256
- **Batch size:** 32
- **Epochs:** 10
- **Optimizer:** Adam
- **Learning rate:** 0.0003

## Evaluation Results

| Metric | Result |
|---|---:|
| Total parameters | 4,783,232 |
| Training time | 2.54 minutes |
| BLEU | 13.0148 |
| chrF | 6.1098 |
| Inference time (1,000 examples) | 73.14 seconds |
| Best validation loss | 4.2787 |

## Files

| File | Description |
|---|---|
| `basic_transformer_predictions.csv` | Generated translations for 1,000 test examples, including English inputs, Spanish references, and model predictions |
| `basic_transformer_metrics.json` | Evaluation metrics, parameter count, and inference performance |
| `basic_transformer_training_history.json` | Training and validation loss recorded across 10 epochs |
| `basic_transformer_training_summary.json` | Training configuration summary and best validation loss |
| `basic_transformer_loss_curve.png` | Training versus validation loss visualization |

## Observations

- Training and validation losses decreased throughout the 10 epochs.
- The model achieved a BLEU score of 13.0148 and a chrF score of 6.1098.
- Some generated translations contained `<unk>` tokens and repetitive sequences.
- These outputs indicate limitations in translation quality under the implemented training configuration.

## Notes

The prediction file contains actual outputs from the completed test evaluation. No additional training or inference was performed to produce these stored artifacts.
