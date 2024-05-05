---
id: lnzw89w69exbqm9xtuhq9lg
title: Flaky Tests
desc: ''
updated: 1714895176837
created: 1714895176837
tags: testing flaky-test
---

[Tests that sometimes fail - flaky test tips](https://samsaffron.com/archive/2019/05/15/tests-that-sometimes-fail)

- [1. Definition](#1-definition)
- [2. Background](#2-background)
- [3. Several pattern of Flakiness](#3-several-pattern-of-flakiness)
  - [3.1. Hard coded id](#31-hard-coded-id)
  - [3.2. Random data](#32-random-data)
  - [3.3. Database execution ordering assumptions](#33-database-execution-ordering-assumptions)
  - [3.4. Time assumptions](#34-time-assumptions)
  - [3.5. Concurrency](#35-concurrency)
  - [3.6. Leaky global state](#36-leaky-global-state)
  - [3.7. Assumptions about environment](#37-assumptions-about-environment)
- [4. How to handle Flaky test](#4-how-to-handle-flaky-test)

## 1. Definition

Sometimes when tests are written, they might become test that fails intermittently. These are called flaky test or heisentest (in relation to [heisenbug](https://en.wikipedia.org/wiki/Heisenbug))

## 2. Background

Flaky test incurs cost not only in time spent on the pipeline, but also on developer's time.
Hence it is crucial that we either remove flakiness or fix it.
Fixing flaky tests might also result in fixing an actual bug hidden inside the code.

## 3. Several pattern of Flakiness

### 3.1. Hard coded id

When using database with auto increment ids, sometimes its easier for us to simply assign id and check whether it is equal to some hard coded id.
This can be further generalized as any hardcoded assumptions.
TODO: reread and fix this part

### 3.2. Random data

some unexpected data/input can expose a bug or a wrong assumption deep inside the code.
This is how Fuzzy testing (or commonly known as [Fuzzing](https://en.wikipedia.org/wiki/Fuzzing) come to be.
Fuzzy testing work by generating random data with some restriction which is used to hit the system.

### 3.3. Database execution ordering assumptions

this is related to point 1 in that order should not be assumed unless explicitly ordered by the developer (no pun intended)

### 3.4. Time assumptions

Time is one of those things that we don't usually think about testing for/about; because, how can Time fail?
However, code that is basing its logic on time can fail and code should be designed to be easily testable even against time.

Related to this is places where we use time.Sleep to wait on some asynchronous or background processing.
This is a typical flaky smell; not only does this make your test take longer, it's also implicit on what it's waiting for.
If the code needs to take some specific amount of time, then it should be explicit.
Otherwise, Test should not rely on sleep to wait on some correct state.
Instead, use polling or some explicit way to sync the state.
Possibly by calling a function and processing it synchronously instead of waiting on some state to be true.

### 3.5. Concurrency
Semi-related to [time assumptions](#time-assumptions).
If we use background processing in test, it could result race condition.
Especially if tests are run in parallel.

### 3.6. Leaky global state
Sometimes tests modifies some global state.
Unless the test suite are reset to a pristine state, there can be some modified global state between tests.
This would result in some condition being true only if tests are run in certain order.
Hence why tests should always be run in random order.

### 3.7. Assumptions about environment
Remember that in the end your code are not only running locally.
It is also tested locally, in CI/CD pipeline, and running in production.
It is hard to make sure the condition is always the same (even if you use container).
There will be small differences here and there and thats why you should challenge your assumptions.
TODO: reread this part and fix

## 4. How to handle Flaky test

Handling flaky test should be a team effort.
Assign flaky test to original test writer to fix, and give post mortem on how it come to be.
Treat it like an incident and figure out the root cause.

Below are several points on how to work as a team to reduce flakiness:

1. Should be a team concern
It should be a team decision to either remove, run repeatedly, ignore, or invest time to find cause and fix
2. Stopping deployment if any test failed
Depends on urgency, when deployment timeline is tight, test can be quarantined/excluded temporarily
3. Run test in random order
4. Invest in making tests run faster
Tighter loop means it can be run faster on a loop and flaky test are found faster
5. Regularly run test suite
This can be done in asynchronous manner to actual code change to prod.
Very rare flaky test can be found if your test suite are fast and you're running it endlessly (even if say chance of happening is 1/xxxxx)
6. Add log in unexpected flaky case
