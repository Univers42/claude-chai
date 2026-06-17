---
description: >
  Find and fix performance bottlenecks with proof.
  Usage: /workflow:perf-sprint <module or "full">
---

# Performance Sprint

Target: $ARGUMENTS

## 1. Baseline

- Run the full benchmark suite
- Record: operation, p50, p95, p99, throughput, memory
- Save as `benchmarks/baseline-<date>.md`
- This is the number to beat. No vibes, only numbers.

## 2. Profile

- Run the profiler (pprof for Go, perf/flamegraph for C/Rust,
  clinic.js for Node)
- Identify the top 5 hottest functions
- For each: why is it hot? (CPU, alloc, I/O, contention)

## 3. Prioritize

- Rank by: impact × ease of fix
- Present the list. Wait for approval on which to tackle.

## 4. Optimize (per item)

a. Explain the optimization in one sentence before coding
b. Implement the change
c. Run the benchmark for that specific operation
d. Record: before → after → delta
e. If regression or no improvement, revert immediately
f. Commit with benchmark numbers in the message

## 5. Full suite

- Run complete benchmark suite again
- Compare against baseline from step 1
- Produce a comparison table

## 6. Report

Output: `benchmarks/sprint-<date>.md`

| Operation | Before | After | Delta |
| --------- | ------ | ----- | ----- |
