# Output fields v1.0

This dictionary describes the frozen [JSON schema](coding_schema_v1.0.json). All listed object fields are required; extra properties are disallowed. Arrays may contain multiple applicable categories. `ambiguity_note` alone may be `null`. D/I/E are independent integer codes, not a combined score.

## Conversation record

| Field | Type / allowed values | Meaning |
|---|---|---|
| `schema_version` | string: `1.0` | Version of the coding schema. |
| `case_id` | string | Unique conversation identifier supplied in the input. |
| `semester` | string: `F24`, `S25` | Semester of the conversation. |
| `assignment` | string: `a1`, `a2`, `a3`, `a4`, `a5`, `a6`, `a7` | Course assignment associated with the conversation. |
| `episodes` | array of object | Local sequences organized around relatively stable subproblems, artifacts, or goals. |
| `turn_codings` | array of object | One coding record for every student turn in the conversation. |
| `conversation_trajectory` | string | Concise narrative of how delegation, human re-entry, Direction, Integration, and Evaluation unfold across the conversation. Do not provide an overall Epistemic Ownership score or rating. |

## Episode record: `episodes[]`

| Field | Type / allowed values | Meaning |
|---|---|---|
| `episode_id` | string | Episode identifier, e.g. E1, E2. |
| `episode_goal` | string | Concise description of the local goal of the episode. |
| `turn_ids` | array of integer | Student turn IDs assigned to this episode. |
| `episode_trajectory` | string | Brief sequential description of the episode without assigning a global Epistemic Ownership score. |

## Student-turn record: `turn_codings[]`

| Field | Type / allowed values | Meaning |
|---|---|---|
| `turn_id` | integer | Student turn number in the supplied transcript. |
| `episode_id` | string | Episode to which the student turn belongs. |
| `functional_move` | array of string: `initiates`, `specifies`, `adds_context`, `implementation_request`, `conceptual_question`, `applies`, `accepts_selectively`, `inspects`, `questions`, `corrects`, `rejects`, `extends`, `redirects`, `resets`, `terminates`, `integrates/reuses`, `requests_verification`, `reports_failure`, `proposes_diagnostic_candidate`, `tests`, `acknowledges`, `representation_translation` | Functional description(s) of what the student is doing. Use only Codebook v1.0 values. Use an empty array when no listed functional move can be defensibly assigned. |
| `relation_to_prior` | array of string: `new`, `continues`, `applies_prior_AI`, `transforms_prior_AI`, `challenges`, `corrects`, `re-ask/unresolved`, `evidence_escalation`, `task_reset` | Relation of the student move to prior interaction. More than one value may apply. |
| `direction` | integer: `0`, `1` | 1 only when the student observably governs the cognitive/task goal, assistance, procedure, artifact, decision, constraint, workflow state, or trajectory. |
| `integration` | integer: `0`, `1` | 1 only when there is observable uptake of a prior AI contribution through incorporation, application, transformation, reuse, selective adoption, or transfer. |
| `evaluation` | integer: `0`, `1` | 1 only when the student observably participates in judgment through inspection, comparison, challenge, criterion application, testing, diagnosis, correction, rejection, or interpretation. |
| `target` | array of string: `format/presentation`, `communicative_fit`, `data_representation/schema`, `artifact`, `implementation/tool`, `procedure`, `result`, `concept/model`, `architecture/system`, `delegation/scope`, `workflow_state/artifact_lineage` | Object(s) or aspect(s) toward which the coded behavior is directed. Use an empty array when none is applicable. |
| `reference_basis` | array of string: `assignment_instructions`, `prior_AI_output`, `own_artifact/code`, `prior_procedure`, `own/domain_knowledge`, `runtime_feedback`, `observed_behavior`, `expected_result`, `communicative_intention`, `supplied_mapping/query`, `supplied_data/corpus` | Observable basis or bases on which the student action relies. Use an empty array when none is applicable. |
| `validation_mode` | array of string: `none`, `inspection`, `instruction_comparison`, `conceptual_reasoning`, `execution`, `external_result`, `static_analysis/IDE_feedback` | Mode(s) through which evaluation or checking becomes observable. |
| `diagnostic_contribution` | array of string: `none`, `report`, `locate`, `hypothesize`, `test`, `revise` | Student contribution to diagnosis. Use 'none' when no diagnostic contribution is observable. |
| `content_provenance` | array of string: `student-generated`, `assignment-provided`, `AI-derived`, `runtime/environment`, `external/unclear` | Origin(s) of content, instruction, hypothesis, or material used in the student move. |
| `criterion_provenance` | array of string: `student-generated`, `assignment-provided`, `AI-derived`, `runtime/environment`, `external/unclear` | Origin(s) of the criterion used for evaluation. Use an empty array when no evaluation criterion is involved. |
| `judgment_provenance` | array of string: `student-generated`, `assignment-provided`, `AI-derived`, `runtime/environment`, `external/unclear` | Source(s) of the judgment. For observable student Evaluation, student-generated should normally be present. |
| `evidence` | array of object | Evidence entries for every positive Direction, Integration, or Evaluation code. Leave empty when D=I=E=0. |
| `ambiguity_note` | string or null | Concise note about plausible alternative interpretations or limits of inference. Null when no material ambiguity is present. |

## Evidence record: `turn_codings[].evidence[]`

| Field | Type / allowed values | Meaning |
|---|---|---|
| `dimension` | string: `direction`, `integration`, `evaluation` | Dimension supported by this evidence. |
| `evidence_type` | string: `explicit`, `inferable` | Whether the evidence is directly visible or conservatively inferable from a sufficiently specific sequence. |
| `evidence_quote` | string | Shortest sufficient verbatim quote from the student turn. Do not reconstruct or paraphrase. |

## Interpretation rules

- Every student turn is coded once and assigned to one episode. AI responses provide context and are not separately coded as student behavior.
- Each positive D/I/E code has its own evidence record. A zero code has no matching evidence record; all three zero codes require an empty evidence array.
- `evidence_quote` is a minimal verbatim span from the corresponding student turn. `explicit` means directly visible behavior; `inferable` requires a specific supporting sequence. Sequential continuity alone is insufficient for Integration.
- Content, criterion, and judgment provenance are separate arrays. An assignment-provided criterion can coexist with a student-generated judgment. Provenance qualifies attribution without automatically cancelling D/I/E.
- `validation_mode` and `diagnostic_contribution` describe the behavior; `none` must not be combined with another value in the same array. These fields do not replace the substantive D/I/E decision rules.
- Episode and conversation trajectories are narrative summaries, not ratings. Answerability is not an output field.
- The JSON does not repeat `student_text`: the input transcript supplies it, while evidence records preserve selected quotes. `episode_goal` belongs to the episode record, and evidence type and quote belong to individual evidence records.
- The codebook's illustrative `adjudicated_validity` is not a field in this frozen schema and is not part of the D/I/E coding output. Technical correctness and observable Evaluation must be distinguished.
