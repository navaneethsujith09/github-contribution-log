Contribution Number: **1**

**Student:** Navaneeth Sujith

**Issue:** apache/gluten#4945

**Status:** **[Phase IV] [Complete]**

## Why I Chose This Issue

I'm interested in supporting the skewness aggregate function because it seemed like a useful feature that was missing from the Gluten ClickHouse backend. It gave me the opportunity to learn more about how aggregate functions are implemented in Gluten and how Spark functions are translated into native ClickHouse execution.

I also liked that this issue was focused enough to let me understand a specific part of the codebase while still making a meaningful contribution. Through this issue, I hoped to gain experience working with a large open-source project, following existing design patterns, and contributing code that improves Spark compatibility.

---

## Understanding the Issue

### Problem Description

The ClickHouse backend in Gluten did not support Spark's `skewness` aggregate function. Because of this, queries using `skewness` could not execute natively and instead fell back to Spark execution.

### Expected Behavior

Queries using `skewness` should execute natively using ClickHouse while producing results consistent with Spark SQL.

### Current Behavior

`skewness` was listed as an unsupported aggregate function in the ClickHouse backend, preventing native execution.

### Affected Components

* `CHExpressionUtil.scala`
* Aggregate function transformer implementation
* ClickHouse SQL tests
* Native execution tests

---

## Reproduction Process

### Environment Setup

Built Gluten locally with the ClickHouse backend and configured a Spark development environment for running the ClickHouse unit tests. Most of the setup involved becoming familiar with the project's build process and test framework.

### Steps to Reproduce

**Step 1**

Build Gluten with the ClickHouse backend enabled.

**Step 2**

Run a Spark SQL query using the `skewness` aggregate function.

**Observed Result**

The query falls back instead of executing natively because `skewness` is marked as unsupported.

### Reproduction Evidence

**Commit showing reproduction:** N/A

**Screenshots/logs:** N/A

**My findings:**

I found that the unsupported behavior came from `CHExpressionUtil.scala`, where `skewness` was explicitly included in the unsupported aggregate function list.

**Not a Bug**

This is a missing feature rather than a software bug.

---

## Solution Approach

### Analysis

ClickHouse already provides the `skewSamp` aggregate function, which computes the same statistic as Spark's `skewness`. The main work was mapping Spark's function to the ClickHouse implementation while preserving Spark semantics. One difference is that ClickHouse returns `NaN` when fewer than three non-null values are present, while Spark returns `NULL`.

### Proposed Solution

Support the skewness aggregate function in the ClickHouse backend by:

* Mapping Spark's `skewness` to ClickHouse's `skewSamp`.
* Adding `NaN → NULL` conversion to match Spark behavior.
* Removing `skewness` from the unsupported function blacklist.
* Adding unit and SQL tests.

### Implementation Plan

Support the skewness aggregate function in the ClickHouse backend:

* Map Spark's `skewness` to ClickHouse's `skewSamp`.
* Add `NaN → NULL` conversion to match Spark's behavior when fewer than three non-null values are present.
* Remove `skewness` from the unsupported function blacklist in `CHExpressionUtil.scala`.
* Add unit tests and SQL tests to verify correctness and native execution.

**Using UMPIRE framework (adapted):**

**Understand:** Support Spark's `skewness` aggregate function while maintaining Spark-compatible behavior.

**Match:** Follow the implementation pattern used by existing statistical aggregate functions such as variance and standard deviation.

**Plan:**

* Remove `skewness` from the unsupported function list.
* Add mapping to `skewSamp`.
* Handle `NaN → NULL`.
* Update SQL tests.
* Add native execution verification.

**Implement:** Implemented and submitted in PR **apache/gluten#12294**.

**Review:** Verified coding style, Spark compatibility, and successful native execution.

**Evaluate:** Ran ClickHouse backend tests, verified no fallback using `checkFallBack(df, noFallback = true)`, and compared results against vanilla Spark using `compareResultsAgainstVanillaSpark`.

---

## Testing Strategy

### Unit Tests

☑ Test case 1: Verify correct skewness values for standard datasets.

☑ Test case 2: Verify `NULL` is returned when fewer than three non-null values are present.

☑ Test case 3: Verify native execution with no fallback.

### Integration Tests

☑ Integration scenario 1: Execute SQL queries containing `skewness`.

☑ Integration scenario 2: Compare ClickHouse backend results against vanilla Spark.

### Manual Testing

Ran representative SQL queries locally and confirmed that results matched Spark while executing natively.

---

## Implementation Notes

### Week 1 Progress

* Investigated the issue.
* Learned how aggregate functions are implemented.
* Identified the required files.

### Week 2 Progress

* Implemented the function mapping.
* Added compatibility handling for `NaN → NULL`.
* Updated tests.
* Submitted the pull request.
* Addressed maintainer feedback.

---

## Code Changes

**Files modified:**

* `CHExpressionUtil.scala`
* Aggregate function transformer
* ClickHouse SQL tests
* Native execution tests

**Key commits:**

(PR contained in **apache/gluten#12294**)

**Approach decisions:**

Used ClickHouse's existing `skewSamp` implementation instead of creating a new aggregate function, minimizing changes while preserving Spark compatibility.
https://github.com/apache/gluten/commit/fb646bedb9dbbfd04d8cfb5871c64c9dac0ddd84
---

## Pull Request

**PR Link:** https://github.com/apache/gluten/commit/fb646bedb9dbbfd04d8cfb5871c64c9dac0ddd84
**PR Description:** Support Spark skewness aggregate function in the ClickHouse backend by mapping to `skewSamp`, adding `NaN → NULL` conversion, removing the unsupported function restriction, and adding compatibility tests.

**Maintainer Feedback:**
He asked me to fix a couple of small things. 

**Status:** Complete

---

## Learnings & Reflections

### Technical Skills Gained

* Learned how Gluten maps Spark SQL functions to ClickHouse.
* Better understood aggregate function transformers.
* Learned how Gluten verifies native execution through its testing framework.

### Challenges Overcome

The biggest challenge was understanding where aggregate functions are registered and transformed. Looking at existing implementations helped me follow the project's architecture.

### What I'd Do Differently Next Time

I would spend more time reading similar implementations before starting so I could become familiar with the relevant code paths earlier.

---

## Resources Used

* Apache Gluten source code
* GitHub issue `apache/gluten#4945`
* Pull Request `apache/gluten#12294`
* Spark SQL documentation
* ClickHouse aggregate function documentation
