# Gemma Decoding Strategy: Temperature and Sampling Analysis

## 1. Objective

This experiment examined how decoding strategy affects English-to-Spanish
translation quality and inference time for the Gemma 2B IT model. Three
configurations were evaluated on the same 100-example test subset:

- Greedy decoding
- Sampling with temperature 0.7 and top-p 0.9
- Sampling with temperature 1.0 and top-p 0.9

The results are reported separately from the main 1,000-example evaluation.

## 2. Results

| Decoding configuration | BLEU | chrF | Inference time (100 examples) |
|---|---:|---:|---:|
| Greedy | 29.4521 | 53.3279 | 66.44 s |
| Sampling (T=0.7, top-p=0.9) | 24.7170 | 48.5303 | 65.66 s |
| Sampling (T=1.0, top-p=0.9) | 21.6606 | 45.8957 | 64.09 s |

## 3. Translation quality

Greedy decoding achieved the highest scores for both evaluation metrics,
with BLEU 29.4521 and chrF 53.3279.

Compared with greedy decoding:

- Sampling at temperature 0.7 reduced BLEU by 4.7351 points and chrF
  by 4.7976 points.
- Sampling at temperature 1.0 reduced BLEU by 7.7915 points and chrF
  by 7.4322 points.

In this experiment, the higher-temperature configuration produced the
lowest BLEU and chrF scores. This is consistent with sampled outputs
matching the reference translations less closely according to these
automatic metrics.

## 4. Inference time

The measured inference times were 66.44 seconds for greedy decoding,
65.66 seconds for temperature 0.7 sampling, and 64.09 seconds for
temperature 1.0 sampling over the 100 examples.

The observed differences were small. These measurements alone do not
establish that sampling is inherently faster than greedy decoding;
timings may vary with execution conditions and generation settings.

## 5. Interpretation

For this test subset and these settings, greedy decoding provided the
best reference-based translation scores. Increasing the sampling
temperature from 0.7 to 1.0 was associated with lower BLEU and chrF.

Sampling introduces variation into token selection, whereas greedy
decoding selects the highest-probability next token at each generation
step. More varied outputs are not necessarily more accurate translations.

BLEU and chrF measure similarity to reference translations. They do
not directly measure creativity, semantic adequacy, or every aspect
of fluency. Therefore, these results should not be interpreted as
proof that greedy decoding is always superior for translation.

## 6. Limitations

- The experiment used only 100 test examples, not the full 1,000-example
  evaluation set.
- The results apply to the tested model, prompts, and decoding settings.
- A single recorded run does not quantify variation across random seeds.
- Automatic metrics should be supplemented with human review of meaning,
  grammaticality, omissions, and fluency when possible.
- The sampling results must not be directly compared with the main
  1,000-example scores as though they came from the same evaluation set.

## 7. Conclusion

On the 100-example subset, greedy decoding achieved the strongest
reference-based translation scores. Sampling with temperature 0.7
and 1.0 produced lower BLEU and chrF, with the largest decrease at
temperature 1.0. The measured inference times were relatively close.

These findings support using greedy decoding as the preferred
configuration among the three tested for this particular evaluation.
Broader testing would be required before generalizing this conclusion.
