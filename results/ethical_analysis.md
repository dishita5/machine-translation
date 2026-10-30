# Ethical Considerations and Limitations

## 1. Translation Accuracy and Meaning Preservation

Machine translation systems can produce fluent-looking sentences that do not accurately preserve the meaning of the original text.

This was observed in the TinyLlama predictions. For example, the English sentence "I know what the lies were" was translated as "Siento lo que fueron las mentiras." Although the output is readable, it does not preserve the intended meaning.

The Basic Transformer also produced repetitive sequences and `<unk>` tokens, demonstrating that poor translation quality can make generated text difficult to interpret.

Such errors may be particularly consequential when translating educational, legal, medical, or other important information.

**Mitigation measures:**
- Compare generated translations with the original source text.
- Verify important terms, names, dates, numbers, and negations.
- Use human review for translations where accuracy is important.
- Avoid relying exclusively on fluency or automatic evaluation scores to determine correctness.

## 2. Bias and Dataset Limitations

The project uses the OPUS-100 English–Spanish parallel corpus. The quality and coverage of a translation model depend partly on the examples represented in its training data.

Potential limitations include:

- Uneven representation of domains, topics, and language varieties.
- Differences in regional vocabulary and expressions.
- Cultural references and idioms that may not translate directly.
- Biases or inaccuracies inherited from source text.
- Differences in how gendered expressions and demographic references are represented.

The experiments did not include a dedicated demographic or linguistic bias evaluation. Therefore, these are potential risks rather than measured findings of this project.

**Mitigation measures:**
- Evaluate translations across different topics and language varieties.
- Include examples containing different demographic references and gendered expressions during testing.
- Review potentially biased translations with fluent speakers.
- Use diverse and appropriately reviewed training and evaluation data where possible.

## 3. Hallucination and Unsupported Output

Generative models may produce words, phrases, or interpretations that are not supported by the source sentence.

The TinyLlama examples demonstrate that a readable output can still alter the intended meaning. The Basic Transformer examples also demonstrate that generated sequences may become repetitive or incomplete.

A translation system should preserve the information in the source rather than introduce unsupported details.

**Mitigation measures:**
- Check that generated translations do not introduce information absent from the source.
- Compare source and target sentences for omissions and additions.
- Use appropriate generation constraints and decoding configurations.
- Flag uncertain or incomplete outputs for human verification.

## 4. Cultural Misrepresentation

Expressions, idioms, humour, and cultural references may not have direct equivalents between English and Spanish.

A literal translation may preserve individual words while changing the intended meaning or cultural context. Regional vocabulary differences can also affect how naturally a translation is understood.

The project includes an idiomatic example: "a man of flesh and blood." TinyLlama translated it as "un hombre de sangre y carne," while the reference uses "un hombre de carne y hueso."

This illustrates that different expressions may be produced when translating idiomatic language. A translation should be assessed in context rather than judged only by word-level similarity.

**Mitigation measures:**
- Evaluate idioms and culturally specific expressions separately.
- Prefer contextually appropriate translations over direct word-for-word substitutions.
- Involve fluent speakers familiar with the relevant regional and cultural context.
- Consider regional Spanish vocabulary and usage when selecting a translation.

## 5. Inappropriate Translation of Sensitive Content

Machine translation may incorrectly interpret sensitive information involving medical, legal, educational, or personal matters.

An incorrect translation of a medical instruction, legal statement, or other important information could change its meaning and lead to misunderstanding.

The project did not conduct a dedicated sensitive-content evaluation. This is therefore a potential deployment risk rather than an observed experimental result.

**Mitigation measures:**
- Require qualified human review for medical, legal, and other high-impact translations.
- Verify technical terminology and critical instructions against the source.
- Avoid treating machine-generated translations as authoritative.
- Apply appropriate privacy, authorization, and data-handling safeguards when processing sensitive text.

## 6. Privacy and Responsible Data Use

The project uses a public parallel-text dataset and does not require personal user information for its reported experiments.

For future applications, input text should be handled responsibly. Sensitive personal information should not be submitted to external translation services without appropriate privacy safeguards, authorization, and data-handling policies.

**Mitigation measures:**
- Minimize the collection and retention of personal information.
- Avoid including confidential information in public datasets or shared prediction files.
- Follow applicable privacy and data-protection requirements.
- Review the data-handling policies of any external translation service before using it.

## 7. Human Oversight

Machine-generated translations should be reviewed by a person when accuracy is important.

Recommended safeguards include:

- Human review of important translations.
- Verification of names, dates, numbers, and technical terminology.
- Additional checks for idiomatic expressions and cultural context.
- Clear communication that generated translations may contain errors.
- Avoiding sole reliance on automated translation for high-impact decisions.

Human oversight is particularly important when translations could influence medical, legal, financial, or other consequential decisions.

## 8. Evaluation Limitations

The project uses BLEU and chrF to evaluate translation quality. These metrics measure similarity to reference translations but do not fully assess semantic accuracy, grammaticality, fluency, or cultural appropriateness.

The qualitative examples in `translation_samples.md` are drawn from the available Basic Transformer and TinyLlama prediction files. Individual prediction files for mT5, OPUS-MT, and Gemma were not available for the qualitative comparison.

The Gemma decoding experiment used a separate 100-example subset, while the main model comparison used 1,000 test examples. These results should therefore be interpreted separately.

The models also differ in architecture, pretraining, tokenization, adaptation, and decoding configuration. The comparison does not isolate the effect of any single factor.

## 9. Responsible Use Conclusion

The experiments demonstrate that machine translation quality varies across models and configurations. OPUS-MT achieved the strongest recorded BLEU and chrF scores in the main evaluation, but automatic metrics alone cannot guarantee that every translation is correct.

Bias, hallucination, cultural misrepresentation, and inappropriate handling of sensitive content remain important considerations when deploying translation systems.

These systems should be treated as language-assistance tools rather than unquestionable sources of translated information. Human verification, appropriate data handling, and context-aware evaluation remain important, particularly when incorrect translations could have significant consequences.
