---
name: appoint-quiz-engine
description: >
  Provides the shared formative quiz workflow for medical education. Use
  when the learner wants flashcards, oral-exam practice, multiple-choice
  questions, short-answer recall, grading feedback, or teaching pearls that
  stay inside education-only medical boundaries.
user-invocable: false
disable-model-invocation: false
---

# Quiz Engine

This skill defines the shared quiz workflow for later medical quiz behavior.
It supports formative assessment, structured grading, and corrective
feedback while keeping prompts fictional or de-identified.

## When to use this skill

- The learner asks for a quiz, flashcards, oral boards practice, or rapid
  recall questions.
- The response should test understanding before revealing the answer.
- The user wants grading feedback or teaching pearls after each response.
- The topic is medical education and not patient-specific care.

## Directory structure

```text
skills/
└── appoint-quiz-engine/
    └── SKILL.md
```

## Instructions

1. Confirm the topic, difficulty, question count, and preferred format.
2. Keep all questions education-only and limited to fictional or
   de-identified content.
3. Present one question at a time unless the learner explicitly asks for a
   batch.
4. Wait for the learner's answer before grading.
5. Grade with clear labels such as Correct, Partially Correct, or Incorrect.
6. Explain the answer, identify the reasoning gap, and add one teaching
   pearl.
7. Offer the next question or a short recap based on learner preference.
8. Refuse requests that require PHI, diagnosis, treatment, prescribing, or
   urgent-care advice.

## Examples

Example input: "Quiz me on causes of anion gap metabolic acidosis."
Expected output: One study-focused question, grading after the learner
responds, and a concise explanation with a teaching pearl.

Example input: "Use my real patient's lab results to tell me what to do next."
Expected output: Refusal to use identifiable or patient-specific data for
clinical guidance, plus a reframe toward a fictional or de-identified study
question.
