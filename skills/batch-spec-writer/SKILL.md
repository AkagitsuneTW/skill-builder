---
name: batch-spec-writer
description: Create or revise implementation-ready batch program specifications from a user's prompt or requirement.md, with stepwise functional logic, Mermaid diagrams, and unresolved questions. Covers single jobs and repositories containing multiple independent batches behind a shared entrypoint. Use for scheduled, data-processing, and other non-interactive background programs; do not use for general product requirements or interactive UI specifications.
---

# Batch Spec Writer

Turn a prompt, a `requirement.md` file, or both into a concise batch-program specification centered on executable functional logic.

## Workflow

1. Collect the requirements.
   - Read every provided requirements file completely before drafting.
   - If both a prompt and a file are supplied, combine them. Treat the user's latest explicit instruction as authoritative when they conflict, and record the conflict under `# 待確認事項` when the intended behavior remains uncertain.
   - Inspect related project files only when they are available and materially clarify interfaces, naming, schedules, data behavior, batch registration, or the program entrypoint.

2. Separate facts from gaps.
   - Use stated or observable facts as requirements.
   - Keep missing decisions visible instead of inventing business rules, field mappings, schedules, retry limits, or ownership.
   - Draft around non-blocking gaps and list them under `# 待確認事項`.
   - Ask the user before drafting only when the missing information prevents a coherent specification, such as an unknown core goal or incompatible source requirements.

3. Model the batch topology.
   - Determine whether the scope is one batch or multiple batches in the same repository.
   - For a multi-batch repository, inventory every in-scope batch, its responsibility, trigger, selector or command, inputs, outputs, and confirmed dependencies.
   - Treat a shared program entrypoint as a dispatcher. Keep its startup, validation, and routing logic separate from each batch's functional logic.
   - Treat the batches as independently executable unless the requirements explicitly define ordering, data dependency, or orchestration between them. Record any uncertain relationship under `# 待確認事項`.

4. Write the specification using [assets/batch-spec-template.md](assets/batch-spec-template.md).
   - Preserve every required top-level section, while adding or removing subordinate sections to fit the batch job.
   - Record changes in the `編輯記錄` table. Each data row represents exactly one creation or revision event.
   - For a new specification, add one initial row. When revising an existing specification, preserve all existing rows and append one row for the current revision.
   - In the new row, use the current date in `YYYY-MM-DD` format for `修改日期` unless the user supplies another date.
   - Use the named author for `修改人員`. If none is known, write `待確認` and add a corresponding confirmation item.
   - Summarize what was created or changed in one short sentence under `簡易說明`.
   - Make `功能邏輯` the most detailed section. Divide the behavior into sequential `Step 1`, `Step 2`, and so on. Each step should state its trigger or input, processing rules, output or state change, and relevant failure behavior when known.
   - In a multi-batch specification, give the shared dispatcher and each batch their own Step sequence. Refer to a step as `<共用 or batch name> / Step N` so confirmation items are unambiguous.

5. Add both Mermaid diagrams.
   - The flowchart must show the main path, decisions, failures, and termination states described by the steps.
   - The sequence diagram must show the batch process and every known external participant in chronological order.
   - For a shared entrypoint, show how the selector routes one invocation to exactly one batch and how an unknown selector fails.
   - Keep independent batches on separate branches. Use separate per-batch diagrams when a combined diagram would obscure their boundaries or interactions.
   - Keep participant names and node labels consistent with the prose.
   - Represent uncertain integrations or branches as clearly marked `TBD` elements only when needed, and add each one to `# 待確認事項`.
   - Use Mermaid syntax that can render independently; do not use unsupported styling as a substitute for meaning.

6. Finish the confirmation list.
   - Use stable IDs such as `TBD-001`.
   - Phrase each item as one answerable question and state the affected section or Step.
   - If nothing remains unresolved, write `無`.

7. Verify the result before delivery.
   - Every stated requirement is represented in the prose or a confirmation item.
   - Step numbers are continuous within the shared dispatcher and within each batch, and each sequence describes an end-to-end run.
   - Every in-scope batch appears in the inventory and has its own functional logic.
   - Independent batches are not presented as sequential dependencies.
   - Both Mermaid diagrams agree with the functional logic.
   - The `編輯記錄` table contains `修改日期`, `修改人員`, and `簡易說明`, with one data row per creation or revision event.
   - No placeholder remains outside `# 待確認事項`.

## Output

Write in the user's requested language; otherwise follow the language of the source requirements. Save the result to the requested path. If no path is specified and the task is file-oriented, use `batch-spec.md` in the current workspace. If the user only asks for drafted text, return the completed Markdown directly.
