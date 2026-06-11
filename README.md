# Contribution [#]: Missing definitions for `Proc#<<` and `Proc#>>`

**Contribution Number:** 1  
**Student:** Jesse Oseafiana  
**Issue:** https://github.com/sorbet/sorbet/issues/9656  
**Status:** Phase I

---

## Why I Chose This Issue

This common update issue goes into how a production type checker maintains its knowledge of a language's standard library. It's interesting to me because something that is ever so common is engineers forgetting that updates come with very important additions, somewhere out there is a SWE still programming with Python 2 and refusing to update. In this case here, It looks like Sorbet does not introspect Ruby's runtime to know what methods exist on built-in classes, however it does ship to a directory of hand-maintained Ruby Interface files under rbi/core/, and proc.rbi which looks like the knowledge file for Sorbet. The bug is caused by Proc#<< and Proc#>>, the two function-composition operators which weren't added to the file. Sorbet sees f << g on a Proc, finds no matching method signature, and emits a false positive type error, even suggesting the user meant the unary < operator from Module. The fix is not a runtime patch or a C++ change; it is writing two new typed method signatures in proc.rbi using Sorbet's sig DSL, replicating the pattern you can already see in the existing file for methods like call, curry, and lambda?. The composition semantics are well-documented: f << g returns a new Proc that applies g first then f, so the signature needs to accept a Proc argument and return a Proc. The tricky and interesting part is whether you try to preserve generic callable types rather than just Proc, since Ruby actually allows << and >> with any callable responding to call, and Sorbet's type system has constructs like T.proc that could express something more precise, which means there is a real design decision to document in the PR even for a "good first issue."

What makes this a strong match for phase 1 is that the bug sits at the intersection of static analysis, type systems, and the problem of modeling a dynamic language with a stricter formal layer, which maps directly to the LLM tooling and AI/ML data engineering work that I do. Contributing here forces me to read an existing, well-structured codebase's conventions for expressing method types before I write a single line, which is exactly the discipline required when writing type stubs or pydantic schemas or OpenAPI specs for tool-calling pipelines. I will also hope to further understand concretely how a large open source project using Bazel as its build system and C++ as its core implementation exposes a Ruby-facing surface through generated interface files.


---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

[Which parts of the codebase are involved?]

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
