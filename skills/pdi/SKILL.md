---
name: pdi
description: Apply Problem Driven Improvement (PDI) to turn external review findings, defect lists, audit feedback, or post-task critiques into durable improvements. Use when Codex must process one or more reported problems by independently checking each finding, identifying root cause, designing and implementing a bounded fix, verifying the result, and updating the relevant reusable skill or guidance to prevent recurrence, or when the user invokes PDI.
---

# Problem Driven Improvement

Apply PDI as a closed-loop workflow: problem intake → independent assessment → root cause → design → implementation → verification → recurrence prevention.

## Establish the Work Boundary

1. Identify the exact source repository, artifact under review, external problem list, and requested outcome.
2. Inspect applicable repository instructions, current worktree state, and relevant skills before changing files.
3. Preserve the original problem wording and source. Separate reported claims from independently verified facts.
4. Extract a numbered issue queue. Capture the full list before implementation, but process only one issue at a time.
5. Mark destructive actions, approval-state changes, commits, pushes, releases, deployments, and external messages as separate authority decisions. Do not infer permission for them.

If the repository uses governed Sessions, plans, validation lanes, or closeout artifacts, follow those mechanisms instead of creating a parallel process.

## Process One Issue

### 1. Restate and Assess

- Quote or faithfully restate the issue without strengthening it.
- Locate the relevant source, behavior, schema, tests, evidence, and history.
- Decide `accept`, `accept_with_revision`, `reject`, or `needs_evidence`.
- Explain the decision with concrete evidence. Do not accept feedback merely because it came from a reviewer or another model.
- If the issue combines multiple independent defects, split it before implementation and return the resulting items to the queue.

### 2. Determine Root Cause

- Distinguish the visible symptom from the earliest controllable cause.
- Trace the defect through contracts, data flow, implementation, validation, rendering, and reusable instructions as applicable.
- Identify why existing tests, checks, review guidance, or skills failed to prevent or detect it.
- State the affected scope and claim ceiling. Avoid generalizing beyond the observed evidence.

### 3. Design the Fix

Write a bounded design before editing. Include:

- intended semantic or behavioral change;
- files and interfaces expected to change;
- compatibility or migration behavior;
- positive and negative acceptance cases;
- verification commands or observations;
- rollback or recovery considerations when risk warrants them;
- any human decision required before continuing.

Prefer fixing the authoritative source over patching generated output. If the issue is invalid or already fixed, stop the implementation path and document the evidence.

### 4. Implement and Verify

- Make only changes required by the accepted issue and its necessary contract consequences.
- Preserve unrelated user changes in a dirty worktree.
- Run focused tests first, then the relevant broader validation in proportion to risk.
- Inspect generated artifacts or user-visible output when behavior is not proven by tests alone.
- Compare the final diff with the issue and design. A green test suite does not by itself prove the review concern is resolved.
- Record limitations, untested surfaces, and any remaining follow-up separately.

### 5. Prevent Recurrence

Identify the reusable instruction, skill, template, validator, or test that should have prevented the defect.

- Update a skill only when the root cause reveals a transferable process or decision rule.
- Modify the authoritative skill source, not an installed copy, when a source-of-truth repository exists.
- Add the smallest durable rule that would change future behavior; avoid embedding repository-specific names or the exact one-off fix.
- Add or strengthen a validator/test when the rule can be checked deterministically.
- Validate every changed skill with its prescribed validator and confirm UI metadata remains aligned.
- If no relevant skill exists or the lesson is not generalizable, state that explicitly rather than forcing a skill change.

## Close the Issue

Report the issue in this order:

1. disposition and evidence;
2. root cause;
3. implemented change;
4. verification result;
5. recurrence-prevention change;
6. residual risks and claim ceiling;
7. issue status: `resolved`, `resolved_with_bounds`, `rejected`, or `blocked`.

Do not mark an issue resolved until both the direct fix and relevant verification are complete. A skill update is preventive evidence, not proof that the product fix works.

After closing the current issue, move to the next queued issue unless a real authority decision or hard blocker requires user input. Reassess shared assumptions after each issue because an earlier fix may invalidate later findings.

## Completion Criteria

Complete the PDI run only when:

- every captured issue has a disposition and terminal status;
- accepted issues have bounded fixes and verification evidence;
- rejected issues have evidence-backed reasons;
- transferable root causes have an appropriate preventive update or an explicit reason none applies;
- the final repository state and diff have been reviewed;
- the summary distinguishes completed work, bounded claims, remaining risks, and actions that still require authority.

Do not commit, push, release, deploy, or publish unless the user explicitly requested that action or an applicable governed workflow already authorizes it.
