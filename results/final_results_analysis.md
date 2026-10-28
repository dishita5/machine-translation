# Final Model Comparison: Results and Discussion

## 1. Overview

Five translation systems were evaluated for English-to-Spanish translation: a custom Transformer trained from scratch, mT5-small with full fine-tuning, OPUS-MT as a pretrained baseline, and TinyLlama and Gemma adapted using 4-bit QLoRA.

BLEU measures n-gram overlap with reference translations, while chrF measures character n-gram similarity. Neither metric alone fully captures meaning, fluency, or grammatical correctness.

## 2. Translation quality

- **Highest BLEU:** OPUS-MT (42.5432).
- **Highest chrF:** OPUS-MT (64.8982).
- OPUS-MT achieved BLEU 42.5432 and chrF 64.8982, the highest scores among the evaluated systems.
- Gemma 2B IT outperformed TinyLlama 1.1B on both recorded metrics.
- The custom Transformer achieved a higher BLEU score than mT5-small, while mT5-small achieved a higher chrF score than the custom Transformer.

These results indicate that the pretrained OPUS-MT baseline was the strongest translation system in this experiment. The results do not establish that pretraining or model size alone caused the differences, because architecture, tokenization, adaptation, decoding, and implementation also differed.

## 3. Computational performance

- **Fastest measured inference:** OPUS-MT (43.270 seconds per 1,000 examples).
- **Shortest additional training among trained models:** Basic Transformer (2.54 minutes).
- OPUS-MT required no additional training in this experiment, so its training time is recorded as 0.00 minutes; this does not mean that pretraining was free.
- TinyLlama and Gemma had substantially longer measured inference times than the encoder-decoder systems in this setup.

Inference timings are specific to the recorded hardware, batch sizes, generation settings, and implementation. They should not be treated as hardware-independent benchmarks.

## 4. Model size and parameter-efficient adaptation

- The custom Transformer had 4.78 million total parameters.
- mT5-small had approximately 300.18 million total parameters.
- OPUS-MT had approximately 77.94 million total parameters.
- TinyLlama had approximately 1,102.30 million total parameters, with 2.25 million trainable LoRA parameters reported.
- Gemma 2B IT had approximately 2,515.98 million total parameters by the recorded PEFT parameter count, with 9.81 million trainable LoRA parameters.

For the QLoRA models, only the adapter parameters were trained; the base model was loaded in 4-bit quantized form. The total parameter counts and trainable parameter counts therefore describe different quantities. Parameter counts for quantized models can also vary by counting method.

## 5. Interpretation of individual models

### Basic Transformer
The custom model was computationally inexpensive to train, but its low chrF score indicates weak character-level agreement with the references. A model trained from scratch on a relatively limited parallel corpus may struggle to learn robust translation representations.

### mT5-small
Despite full fine-tuning, mT5-small recorded low BLEU and modest chrF in this experiment. Its outputs and metrics should be interpreted as the result of this specific training configuration, preprocessing, and evaluation—not as a general claim that mT5 is unsuitable for translation.

### OPUS-MT
OPUS-MT delivered the strongest recorded translation metrics and the fastest measured inference. Its pretrained English-to-Spanish translation capability provides a useful reference point for evaluating the models adapted in this project.

### TinyLlama 1.1B
QLoRA made adaptation possible without updating all base-model parameters. However, the measured inference time was high relative to the encoder-decoder models, and translation quality remained below OPUS-MT.

### Gemma 2B IT
Gemma achieved higher BLEU and chrF than TinyLlama in the recorded evaluation, with shorter inference time than TinyLlama. It still did not exceed OPUS-MT on either translation metric.

## 6. Important experimental limitations

- The models use different architectures, pretrained checkpoints, tokenizers, adaptation methods, and decoding strategies.
- OPUS-MT uses beam search with four beams, while the other recorded evaluations use greedy decoding.
- Training times do not include the original pretraining cost of pretrained models.
- BLEU and chrF are automatic metrics; manual review is needed to assess adequacy, fluency, omissions, and meaning preservation.
- The conclusions apply to the recorded test evaluation and configuration. Repeated runs and broader test data would help assess variability and generalization.

## 7. Conclusion

Under the configurations tested, OPUS-MT was the strongest overall choice for English-to-Spanish translation, achieving the highest BLEU and chrF scores and the lowest measured inference time. Gemma outperformed TinyLlama on both recorded translation metrics, while the custom Transformer was the least expensive to train but produced weak translation-quality scores. The experiment illustrates that pretrained task-specific translation systems can be highly competitive against general-purpose models adapted with parameter-efficient fine-tuning.

The results should be interpreted as a comparison of complete experimental systems, rather than a controlled test of architecture alone.