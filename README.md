# Same Performance, Different Process

**Research materials for: *Same Performance, Different Process: Epistemic Ownership in AI-Mediated Education***

## About the paper

This paper examines 150 student–AI conversations from StudyChat to investigate how cognitive participation becomes visible during AI-mediated educational work. It uses Epistemic Ownership as an interpretive framework, focusing on observable traces of Direction, Integration, and Evaluation. Direction concerns students' control over goals, assistance, procedures, and the course of a task. Integration concerns observable uptake of prior AI contributions. Evaluation concerns students' participation in judgment through criteria, comparisons, inspection, or diagnosis. These dimensions are coded independently at the student-turn level and interpreted in their conversational sequence.

The central finding is that similar assessed performance can coexist with different observable configurations of cognitive participation. Assignment scores describe assessed outcomes, but cannot by themselves reconstruct how cognitive work was distributed between student and AI. The analysis is descriptive and does not establish a causal relationship between interaction patterns and performance. It also distinguishes the absence of visible evidence from the absence of cognitive activity. Answerability belongs to the conceptual framework but is not directly measured with these data: the conversations do not systematically elicit students' capacity to reconstruct and defend the reasoning embodied in their work.

## Repository scope

This repository contains the frozen research materials used for the analyses reported in the paper. It is a methodological snapshot of that study and does not include subsequent extensions of the framework, assessment methods, or associated software systems.

## Repository contents

- [codebook_v1.0.md](codebook_v1.0.md): substantive definitions, boundary rules, evidence, provenance, and sequence interpretation.
- [coding_prompt_v1.0.txt](coding_prompt_v1.0.txt): verbatim coding prompt template, with case and transcript placeholders.
- [coding_schema_v1.0.json](coding_schema_v1.0.json): original strict Structured Outputs specification, included for self-contained inspection.
- [output_fields_v1.0.md](output_fields_v1.0.md): readable dictionary of the actual structured-output fields and allowed values.
- [methods_note.md](methods_note.md): sampling, protocol freezing, API configuration, and validation.
- [manual_review_note.md](manual_review_note.md): scope and limits of the ten-conversation manual audit.
- [example/synthetic_conversation.txt](example/synthetic_conversation.txt): wholly invented short transcript with illustrative metadata.
- [example/synthetic_output.json](example/synthetic_output.json): manually authored illustration conforming to the frozen schema; not an API result or study observation.
- [NOTICE.md](NOTICE.md): scope and copyright notice.

## Data

StudyChat is an external, publicly released dataset. **StudyChat data are not redistributed in this repository.** The example conversation is entirely synthetic.

McNichols, H., Ikram, F., & Lan, A. (2026). The StudyChat Dataset: Analyzing Student Dialogues With ChatGPT in an Artificial Intelligence Course. *Proceedings of the LAK26: 16th International Learning Analytics and Knowledge Conference*, 53–63. Association for Computing Machinery. [https://doi.org/10.1145/3785022.3785029](https://doi.org/10.1145/3785022.3785029).

Dataset: [StudyChat on Hugging Face](https://huggingface.co/datasets/wmcnicho/StudyChat).

## Methodological note

The codebook, prompt, and output specification were frozen before the main analytical sample. This repository documents that frozen protocol. The analysis concerns observable manifestations of Direction, Integration, and Evaluation; it does not measure Epistemic Ownership as a single score.

The substantive codebook is reproduced without its closing non-operational editorial paragraph. The prompt and JSON schema are unaltered copies. See the methods note for how these components were supplied together and the manual review note for the limits of the audit.

## Citation

Fábrega, J. (2026). *Same Performance, Different Process: Epistemic Ownership in AI-Mediated Education*. WAILS 2026 - 3rd Workshop on Artificial Intelligence with and for Learning Sciences. Manuscript. 

This reference will be updated when the final proceedings citation and DOI are available. No proceedings DOI is asserted here.
