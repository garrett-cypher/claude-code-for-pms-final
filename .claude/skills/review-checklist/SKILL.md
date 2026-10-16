---
name: review-checklist
description: Reviews a product brief against a fixed four-point checklist before it goes any further. Use when the user says "review this brief", "run the review checklist", "check this brief", or points at a brief file or page and asks for the standard check.
---

# Review checklist

Run the same four checks on any brief, every time. The user should not need to explain the checks again.

## Input

A brief the user points at: a file path, a wiki page, or pasted text. If none is given, ask which brief. Read the whole brief before judging anything.

## The four checks

1. **Owner.** The brief names a person or role who owns it. "The team" or a blank owner field fails. Naming only an author, with no owner, fails.
2. **Success measure.** The brief says how we will know it worked: a metric, a target or direction, and ideally a baseline and a time frame. "Improve the experience" fails. A metric with no way to measure it today is a partial pass; say so.
3. **Scope matches start to end.** Compare what the opening says is in scope (goal, summary, problem) with what the end says (requirements, deliverables, "not doing", next steps). Flag anything that appears at the end but not the start (scope creep), and anything promised at the start that never shows up at the end (dropped scope).
4. **Problem before fix.** The brief explains the problem (who has it, what happens, evidence) before it proposes a solution. Fail if the solution comes first, or the problem is only implied by the solution. Note whether the problem is backed by evidence or just asserted.

## Rules

- Judge only what the brief says. Don't fill gaps from memory or guess an owner. If the brief is silent, that is a fail.
- Quote or point to the line that supports each verdict, so the user can check it.
- Do not rewrite the brief. Suggest the smallest fix for each failed check.
- Use "Pass", "Partial" or "Fail" for each check. No overall score.

## Output format

```
Brief: <name or path>

1. Owner: Pass / Partial / Fail
   Evidence: <quote or location>
   Fix: <only if not Pass>

2. Success measure: ...
3. Scope start vs end: ...
4. Problem before fix: ...

Verdict: Ready to move on / Fix first
Biggest gap: <one sentence>
```

"Ready to move on" only if all four are Pass. Otherwise "Fix first", and the biggest gap is the one that most undermines the brief.
