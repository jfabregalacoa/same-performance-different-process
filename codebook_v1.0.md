# Codebook v1.0

## 1. Coding sheet

The primary unit is **each student turn**. Episodes provide local context, and the full conversation is summarized only as a trajectory. **No global Epistemic Ownership score is calculated.**

### A. Identification and context

| Field | What it records |
|---|---|
| `case_id` | Conversation identifier |
| `episode_id` | Episode or subproblem to which the turn belongs |
| `turn_id` | Order of the turn within the conversation |
| `student_text` | Literal student text |
| `episode_goal` | Local goal of the episode |
| `functional_move` | What the student is functionally doing |
| `relation_to_prior` | Relation of the move to the immediately preceding interaction |

Usual values of `functional_move`: `initiates`, `specifies`, `adds_context`, `implementation_request`, `conceptual_question`, `applies`, `accepts_selectively`, `inspects`, `questions`, `corrects`, `rejects`, `extends`, `redirects`, `resets`, `terminates`, `integrates/reuses`, `requests_verification`, `reports_failure`, `proposes_diagnostic_candidate`, `tests`, `acknowledges`, `representation_translation`.

Usual values of `relation_to_prior`: `new`, `continues`, `applies_prior_AI`, `transforms_prior_AI`, `challenges`, `corrects`, `re-ask/unresolved`, `evidence_escalation`, `task_reset`.

### B. Epistemic Ownership core

| Field | Values | Operational question |
|---|---:|---|
| `direction` | 0 / 1 | Does the student observably govern the cognitive goal, assistance, procedure, artifact, or trajectory? |
| `integration` | 0 / 1 | Does the student incorporate, reuse, transform, or apply an AI contribution? |
| `evaluation` | 0 / 1 | Does the student inspect, judge, contrast, challenge, or verify something using an observable criterion? |

The three dimensions are **independent and multilabel**. The same turn may be D=1, I=1, and E=1.

### C. Behavioral characterization

| Field | Main values |
|---|---|
| `target` | `format/presentation`, `communicative_fit`, `data_representation/schema`, `artifact`, `implementation/tool`, `procedure`, `result`, `concept/model`, `architecture/system`, `delegation/scope`, `workflow_state/artifact_lineage` |
| `reference_basis` | `assignment_instructions`, `prior_AI_output`, `own_artifact/code`, `prior_procedure`, `own/domain_knowledge`, `runtime_feedback`, `observed_behavior`, `expected_result`, `communicative_intention`, `supplied_mapping/query`, `supplied_data/corpus`
| `validation_mode` | `none`, `inspection`, `instruction_comparison`, `conceptual_reasoning`, `execution`, `external_result`, `static_analysis/IDE_feedback` |
| `diagnostic_contribution` | `none`, `report`, `locate`, `hypothesize`, `test`, `revise` |
| `evidence_type` | `explicit`, `inferable` |
| `evidence_quote` | Minimal fragment that justifies the coding |
| `ambiguity_note` | Doubt, plausible alternative, or limit of inference |

### D. Provenance

| Field | What it distinguishes |
|---|---|
| `content_provenance` | Where the instruction, criterion, hypothesis, or material used comes from |
| `criterion_provenance` | Where the criterion used to evaluate something comes from |
| `judgment_provenance` | Who actually makes the judgment |

Values:

`student-generated`

`assignment-provided`

`AI-derived`

`runtime/environment`

`external/unclear`

The last two fields are especially useful for Evaluation. For example:

> “The assignment says use linear regression. Why are you using logistic regression?”

Here:

`criterion_provenance = assignment-provided`

but

`judgment_provenance = student-generated`.

This allows us to recognize Evaluation without attributing authorship of the criterion to the student.

---

## 2. Manual for using the coding sheet

The guiding question for all coding is:

> **What is the student doing with respect to what the AI has just produced and with respect to the course they want to give the task?**

This question must be answered from **observable behavior in the conversation**, not from what we suppose the student thought outside it.

### Step 1. Divide the conversation into episodes

An episode is a sequence oriented toward a relatively stable local goal.

For example:

**load data → resolve pandas error → remove duplicates → merge → normalize**

may constitute five episodes within the same conversation.

A change in subproblem, artifact, or purpose usually indicates a change of episode. Segmentation provides context; **D/I/E coding continues to be conducted at the turn level**.

---

### Step 2. Identify what the student is functionally doing

Before deciding D/I/E, describe the move in functional language.

Examples:

> “Use deque for this”

`accepts_selectively / implementation_refinement`

> “That output cannot be correct because e never occurs after b”

`corrects / diagnostic_candidate`

> “Can you make it 350 words?”

`specifies`

This step reduces the temptation to decide too early that something “is EO.”

---

## 3. How to code Direction

### Operational definition

**Direction exists when the student exercises observable control over a cognitive or task goal, over the limits of AI assistance, or over a procedure, artifact, decision, or work trajectory.**

### Code Direction = 1 when, for example:

| Behavior | Example |
|---|---|
| defines a cognitive task | “Explain how BFS works.” |
| limits assistance | “Give me hints, don't write the solution.” |
| selects a procedure | “Use deque instead.” |
| imposes constraints | “Use only two variables.” |
| corrects the course | “No, I mean the final report section.” |
| controls relevant format | “Write it in simpler words.” |
| controls pipeline state | “Use `final_cleaned_dataset`, not `cleaned_combined_dataset`.” |
| terminates or abandons a path | “Forget about trying to fix that.” |

### Do not code Direction simply because:

the student speaks, asks any question, changes topic, or initiates a conversation.

Examples that **do not count**:

> “Hi”

> “Do you shop at Aldi's?”

> “Why are you a different version of GPT?”

These actions show conversational agency, but they do not yet show **epistemic or task direction**.

A meta-question about the AI can become Direction if it is used to govern delegation:

> “Can you write the code, or should I only ask you for hints? I want to solve it myself.”

Here it does.

---

## 4. How to code Integration

### Operational definition

**Integration exists when there is observable evidence that the student incorporates, reuses, applies, transforms, or transfers an AI contribution.**

The key question is:

> **Do we see the student doing something with a prior AI contribution?**

### Integration = 1

Clear examples:

> The AI recommends `deque`; the student responds: “Use deque for this.”

The alternative comes from the AI and the student incorporates it into the next step.

Another case:

**AI provides code → student executes it → returns with a traceback.**

The execution error constitutes evidence that the contribution was incorporated.

Another:

**AI recommends obtaining `coef_` and `intercept_` → later the student returns with concrete coefficients and asks to construct the equation.**

This may be `Integration = 1`, `evidence_type = inferable`, because there is a specific and plausible connection between the recommendation and the later result.

### Integration = 0

It is not enough that:

- the AI uses material provided by the student;
- the student continues talking about the same topic;
- the conversation advances through the steps of the assignment;
- the AI produces an artifact;
- the student requests successive revisions of the same artifact.

Example:

> student provides notes → AI writes essay → “write it in simpler words” → “make it around 350 words”.

This shows Direction and potentially Evaluation, but **does not demonstrate appropriation or reuse by the student**.

### Crucial rule

> **Sequential continuity is not sufficient evidence of Integration.**

There must be some observable form of **uptake**.

---

## 5. How to code Evaluation

### Operational definition

**Evaluation exists when the student inspects, judges, compares, challenges, verifies, or diagnoses a result, procedure, explanation, artifact, or behavior using some observable criterion.**

### Evaluation = 1

Simple example:

> “This is too complicated. Use simpler words.”

The student judged communicative fit.

Technical example:

> “Nowhere in the document does `e` occur after `b`, so that sequence can never exist.”

Here there is:

observed result → evidence inspection → criterion → judgment.

Debugging example:

> “It works in the isolated test, but main.py is still generating impossible sequences.”

There is comparison across components and localization of the problem.

Assignment-based example:

> “Why are you using logistic regression? I need linear regression.”

Although the criterion comes from the assignment, the student performs the incompatibility judgment.

### Evaluation = 0: verification-seeking

This is one of the most important boundaries in the manual.

> “Does this make sense?”

> “Is this correct?”

> “0.001?”

> “Is it okay that many values are negative?”

These are normally **requests for evaluation**, not Evaluation.

The student delegates the task of judging to the AI.

There may be Direction:

`Direction = 1`

but:

`Evaluation = 0`.

### When does a question cross the threshold?

When it incorporates the student’s own judgment, anomaly, or criterion.

Compare:

> “Is this correct?”

E=0.

with:

> “This cannot be correct because the assignment says the model must be linear.”

E=1.

Or:

> “Could `[0]` be causing the error?”

This may be E=1 because it contains a diagnostic hypothesis that the student is using to evaluate the failure.

---

## 6. Special cases of Evaluation

### Runtime error

A traceback by itself is feedback from the environment. When the student brings it back to continue diagnosis:

`validation_mode = execution`

and there may be Evaluation.

But:

> **runtime error reporting ≠ student-generated diagnosis.**

The diagnosis may come later.

This is why `diagnostic_contribution` exists.

A typical trajectory may be:

`report → locate → hypothesize → test → revise`.

### Metrics

Generating accuracy, cross-validation scores, or model outputs **does not automatically constitute Evaluation**.

> “Accuracy = 0.87”

by itself, no.

> “The neural network performed better than the other models.”

may show Evaluation because it contains a comparative judgment.

### Correctness

Never conflate:

**observable Evaluation**

with

**correct Evaluation**.

A student may actively evaluate and still be wrong.

If the research needs to determine technical validity, it should be recorded separately, for example:

`adjudicated_validity = correct / incorrect / indeterminate`

but **it is not part of EO**.

---

## 7. Use of auxiliary fields

Auxiliary fields should describe **how D/I/E is manifested**, not become independent proxies.

For example:

> “This output cannot be right because there is no `e` after `b` in the corpus.”

could be coded as:

| Field | Value |
|---|---|
| Direction | 1 |
| Integration | 1 if it follows from previously executed AI code |
| Evaluation | 1 |
| Functional move | `corrects / diagnostic_candidate` |
| Target | `result / implementation` |
| Reference basis | `observed_behavior + supplied_data/corpus` |
| Validation mode | `execution + inspection` |
| Diagnostic contribution | `locate/hypothesize` |
| Criterion provenance | `student-generated from supplied data` |
| Judgment provenance | `student-generated` |
| Evidence type | `explicit` |

The auxiliary fields allow us to explain **why** we marked Evaluation; they do not substitute for Evaluation.

---

## 8. Explicit versus inferable evidence

The coding sheet should allow inference, but in a controlled way.

### `explicit`

The behavior appears directly.

> “Use the deque you suggested.”

### `inferable`

The behavior is not stated, but there is a sufficiently specific sequence.

For example:

1. AI recommends running `model.coef_`.
2. Later the student returns with exactly those coefficients.

This may be coded as Integration, but:

`evidence_type = inferable`.

### Conservative rule

> **When several alternative explanations are equally plausible, do not attribute the dimension.**

And record the issue in `ambiguity_note`.

---

## 9. Provenance

This field avoids one of the major potential sources of overinterpretation in StudyChat.

If the student pastes:

> “Use 5-fold cross-validation and random_state=42”

because that comes literally from the assignment, the instruction may function as Direction in the interaction, but:

`content_provenance = assignment-provided`.

This prevents specificity from being interpreted as the student’s own intellectual creation.

Likewise:

**AI-generated idea reused later**

→ `content_provenance = AI-derived`.

**Python error**

→ `runtime/environment`.

Provenance qualifies the evidence; it does not automatically eliminate D/I/E.

---

## 10. How to close each episode

At the end of an episode, record a brief description of the pattern.

For example:

> **AI generation → student implementation → execution error → student diagnosis → AI repair → student retest.**

Or:

> **assignment specification → delegated implementation → artifact inspection → communicative correction.**

These patterns can later be used to analyze trajectories without reducing them to an index.

---

## 11. How to close the conversation

The full conversation should end with a **narrative trajectory**, not a score.

Example:

> *The interaction begins with largely assignment-provided direction and substantial delegation. After implementation, the student increasingly integrates AI-generated code through execution and develops stronger evaluation through runtime testing, diagnosis, correction, and control over successive versions of the artifact.*

This allows us to say something substantive about EO without claiming that the student has, for example, “7.4 points of Epistemic Ownership.”

---

## 12. Minimal coding sheet ready to fill

In practice, this could be the working table:

| Field |
|---|
| `case_id` |
| `episode_id` |
| `turn_id` |
| `student_text` |
| `functional_move` |
| `relation_to_prior` |
| `direction` |
| `integration` |
| `evaluation` |
| `target` |
| `reference_basis` |
| `validation_mode` |
| `diagnostic_contribution` |
| `content_provenance` |
| `criterion_provenance` |
| `judgment_provenance` |
| `evidence_type` |
| `evidence_quote` |
| `ambiguity_note` |

And below each case I would keep only two additional fields:

**Episode trajectory:** brief sequential pattern for each episode.

**Conversation trajectory:** interpretive summary of the full conversation.
