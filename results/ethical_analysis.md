# Ethical Considerations and Limitations

## 1. Translation Accuracy and Meaning Preservation

Machine translation systems can produce fluent-looking sentences that do not accurately preserve the meaning of the original text.

This was observed in the TinyLlama predictions. For example, the English sentence "I know what the lies were" was translated as "Siento lo que fueron las mentiras." Although the output is readable, it does not preserve the intended meaning.

The Basic Transformer also produced repetitive sequences and `<unk>` tokens, demonstrating that poor translation quality can make generated text difficult to interpret.

Such errors may be particularly consequential when translating educational, legal, medical, or other important information.

## 2. Bias and Dataset Limitations

The project uses the OPUS-100 English–Spanish parallel corpus. The quality and coverage of a translation model depend partly on the examples represented in its training data.

Potential limitations include:

- Uneven representation of domains, topics, and language varieties.
- Differences in regional vocabulary and expressions.
- Cultural references and idioms that may not translate directly.
- Biases or inaccuracies inherited from source text.

The experiments did not include a dedicated demographic or linguistic bias evaluation. Therefore, these are potential risks rather than measured findings of this project.

## 3. Hallucination and Unsupported Output

Generative models may produce words, phrases, or interpretations that are not supported by the source sentence.

The TinyLlama examples demonstrate that a readable output can still alter the intended meaning. The Basic Transformer examples also demonstrate that generated sequences may become repetitive or incomplete.

Users should not assume that fluent-looking translations are necessarily accurate.

## 4. Privacy and Responsible Data Use

The project uses a public parallel-text dataset and does not require personal user information for its reported experiments.

For future applications, input text should be handled responsibly. Sensitive personal information should not be submitted to translation services without appropriate privacy safeguards, authorization, and data-handling policies.

## 5. Human Oversight

Machine-generated translations should be reviewed by a person when accuracy is important.

Recommended safeguards include:

- Human review of important translations.
- Verification of names, dates, numbers, and technical terminology.
- Additional checks for idiomatic expressions and cultural context.
- Clear communication that generated translations may contain errors.
- Avoiding sole reliance on automated translation for high-impact decisions.

## 6. Evaluation Limitations

The project uses BLEU and chrF to evaluate translation quality. These metrics measure similarity to reference translations but do not fully assess semantic accuracy, grammaticality, fluency, or cultural appropriateness.

The qualitative examples in `translation_samples.md` are drawn from the available Basic Transformer and TinyLlama prediction files. Individual prediction files for mT5, OPUS-MT, and Gemma were not available for the qualitative comparison.

The Gemma decoding experiment used a separate 100-example subset, while the main model comparison used 1,000 test examples. These results should therefore be interpreted separately.

The models also differ in architecture, pretraining, tokenization, adaptation, and decoding configuration. The comparison does not isolate the effect of any single factor.

## 7. Responsible Use Conclusion

The experiments demonstrate that machine translation quality varies across models and configurations. OPUS-MT achieved the strongest recorded BLEU and chrF scores in the main evaluation, but automatic metrics alone cannot guarantee that every translation is correct.

These systems should be treated as language-assistance tools rather than unquestionable sources of translated information. Human verification remains important, particularly when incorrect translations could have significant consequences.
