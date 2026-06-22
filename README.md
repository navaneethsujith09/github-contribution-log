# Contribution [#]: [Issue Title]

**Contribution Number:** [1 / 2 / 3]  1
**Student:** Navaneeth Sujith
**Issue:** https://github.com/apache/gluten/issues/4945
**Status:** [Phase IV] [In Progress]

---

## Why I Chose This Issue

[1-2 paragraphs explaining why this issue interests you, how it matches your skills/learning goals, what you hope to learn]
I'm interested in supporting the skewness aggregate function because it seems like a useful feature that's currently missing from the Gluten ClickHouse backend. It would give me a chance to learn more about how aggregate functions are implemented in Gluten and how Spark functions are mapped to ClickHouse execution. I also like that it's a relatively focused contribution that can help improve Spark compatibility while adding support for a meaningful statistical function.
---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]
This feature needs to be added.
### Expected Behavior

[What should happen?]
N/A
### Current Behavior

[What actually happens?]
N/A
### Affected Components

[Which parts of the codebase are involved?]
There are a few specific files.
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
Not a Bug
## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan
Support the skewness aggregate function in the ClickHouse backend:

Maps Spark's skewness to ClickHouse's skewSamp
Adds NaN→NULL conversion to match Spark's behavior when fewer than 3 non-null values are present
Removes skewness from the unsupported function blacklist in CHExpressionUtil.scala
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

**PR Description:** https://github.com/apache/gluten/pull/12294

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- remove comment, add additional support
- [Date]: [How you addressed it]

**Status:** [Awaiting review / **Iterating** / Approved / Merged]

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
