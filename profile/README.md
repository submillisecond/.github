<div align="center">

# submillisecond

### Hold the line on p99.

A working notebook of sub-millisecond data structures - and the perf-CI gate
that catches the regression before it ships.

<br/>

[![cookbook](https://img.shields.io/badge/-cookbook-1f6feb?style=for-the-badge&logo=jupyter&logoColor=white&labelColor=0d1117)](https://submillisecond.com/cookbook)
[![actions suite](https://img.shields.io/badge/-perf--CI%20gate-238636?style=for-the-badge&logo=githubactions&logoColor=white&labelColor=0d1117)](https://github.com/submillisecond/subms-actions)
[![license MIT](https://img.shields.io/badge/-MIT-9d2235?style=for-the-badge&labelColor=0d1117)](https://github.com/submillisecond/subms-actions/blob/main/LICENSE)
[![status](https://img.shields.io/badge/-pre--release-d29922?style=for-the-badge&labelColor=0d1117)](#status)

<br/>

*16 dual-language Rust + Java recipes. 1 perf harness. 5 GitHub Actions. 1 JSON contract.*
*Every number measured; every claim reproducible; every regression gated.*

</div>

---

## What we ship

### The harness library

A zero-dependency perf-test harness for Rust and Java that records timed
samples per stage, computes percentiles, supports coordinated-omission
correction, and emits a stable JSON contract the rest of the ecosystem
consumes.

| repo | one-line | status |
|---|---|---|
| [`subms`](https://github.com/submillisecond/subms) | The library. `subms` on [crates.io](https://crates.io/crates/subms); `com.submillisecond:subms` on [Maven Central](https://central.sonatype.com/artifact/com.submillisecond/subms). | [![ci](https://github.com/submillisecond/subms/actions/workflows/ci.yml/badge.svg)](https://github.com/submillisecond/subms/actions/workflows/ci.yml) |

### The cookbook

16 dual-language Rust + Java recipes, 3 Java guides, all writeups + perf
JSON, and the discovery CLI - one repo, one clone, the whole working
notebook. Every recipe ships >= 90% line coverage, a documented quality-bar
contract (reference impl + claim conditions + non-claims), and an asserted
sub-millisecond p99 in CI.

| repo | one-line | status |
|---|---|---|
| [`subms-cookbook`](https://github.com/submillisecond/subms-cookbook) | Recipes (`subms-bloom-filter`, `subms-lsm-tree`, `subms-hyperloglog`, ...) + guides + content + `@submillisecond/subms` CLI. | [![ci](https://github.com/submillisecond/subms-cookbook/actions/workflows/ci.yml/badge.svg)](https://github.com/submillisecond/subms-cookbook/actions/workflows/ci.yml) |

### The website

The Next.js site that renders [submillisecond.com](https://submillisecond.com) -
cookbook, blog, glossary, and topic pages. Statically exported to Cloudflare
for prod; local dev runs an admin layer for editing posts + glossary entries
straight to disk.

| repo | one-line | status |
|---|---|---|
| [`subms-ui`](https://github.com/submillisecond/subms-ui) | Next.js 15 + React 19. Renders the cookbook content + perf JSON; deploys to Cloudflare Workers via `ks publish subms-ui:ui`. | [![ci](https://github.com/submillisecond/subms-ui/actions/workflows/ci.yml/badge.svg)](https://github.com/submillisecond/subms-ui/actions/workflows/ci.yml) |

### Perf-CI gate suite (GitHub Actions)

Composable actions that diff perf JSON against a baseline, post a sticky PR
comment with per-stage deltas, fail the status check on regression, push to
13 downstream sinks (Slack/Datadog/Prometheus/S3/...), and detect slow
drift across a rolling window.

| repo | one-line | status |
|---|---|---|
| [`subms-action-bench`](https://github.com/submillisecond/subms-action-bench) | Run a bench command, capture JSON, validate, retry on flake. | [![self-test](https://github.com/submillisecond/subms-action-bench/actions/workflows/self-test.yml/badge.svg)](https://github.com/submillisecond/subms-action-bench/actions/workflows/self-test.yml) |
| [`subms-action-diff`](https://github.com/submillisecond/subms-action-diff) | The gate. Baseline vs candidate -> sticky PR comment + status check. | [![self-test](https://github.com/submillisecond/subms-action-diff/actions/workflows/self-test.yml/badge.svg)](https://github.com/submillisecond/subms-action-diff/actions/workflows/self-test.yml) |
| [`subms-action-diff-aggregate`](https://github.com/submillisecond/subms-action-diff-aggregate) | Roll N matrix diffs into one PR comment + one verdict. | [![self-test](https://github.com/submillisecond/subms-action-diff-aggregate/actions/workflows/self-test.yml/badge.svg)](https://github.com/submillisecond/subms-action-diff-aggregate/actions/workflows/self-test.yml) |
| [`subms-action-diff-sink`](https://github.com/submillisecond/subms-action-diff-sink) | Push diff JSON to 13 sinks: Slack, HTTP, S3, GCS, Azure, Prometheus, InfluxDB, Datadog, Splunk, NewRelic, Honeycomb, file, stdout. | [![self-test](https://github.com/submillisecond/subms-action-diff-sink/actions/workflows/self-test.yml/badge.svg)](https://github.com/submillisecond/subms-action-diff-sink/actions/workflows/self-test.yml) |
| [`subms-action-drift`](https://github.com/submillisecond/subms-action-drift) | Detect slow drift (Welford rolling mean +/- k*sigma). | [![self-test](https://github.com/submillisecond/subms-action-drift/actions/workflows/self-test.yml/badge.svg)](https://github.com/submillisecond/subms-action-drift/actions/workflows/self-test.yml) |
| [`subms-actions`](https://github.com/submillisecond/subms-actions) | Umbrella - reusable workflow + pre-commit hook + shared scripts + suite docs. | [![self-test](https://github.com/submillisecond/subms-actions/actions/workflows/self-test.yml/badge.svg)](https://github.com/submillisecond/subms-actions/actions/workflows/self-test.yml) |

## Why use it

- **One JSON shape across runtimes.** Rust + Java emit it natively (via the
  `subms` crate / jar); JMH, Criterion, HdrHistogram plug in via ~60-LOC
  adapters. One pipeline accepts them all.
- **Status-check tollgate.** The action fails the PR's check on regression -
  branch-protection rules can block merges automatically.
- **Sticky PR comments.** Re-runs and force-pushes update the same comment.
  No 32-comment matrix spam.
- **Drift detection.** Catches "p99 has been creeping +1 %/week for 8 weeks"
  that base-ref diffing misses.
- **Enterprise-grade transport.** HTTPS_PROXY / mTLS / custom CA bundles /
  retry-with-backoff / PII scrubbing.
- **Zero npm install.** Composite actions + Node std-lib only. Works on any
  GitHub runner without dependency setup.

## Quickstart

Three-step CI gate for a Rust crate:

```yaml
# .github/workflows/perf.yml
name: perf
on:
  pull_request:

permissions:
  contents: read
  pull-requests: write

jobs:
  diff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with: { fetch-depth: 2 }

      - name: Snapshot baseline
        run: git show ${{ github.event.pull_request.base.sha }}:perf/myworkload.rust.json > baseline.json

      - name: Use PR perf JSON
        run: cp perf/myworkload.rust.json candidate.json

      - uses: submillisecond/subms-action-diff@v0
        with:
          baseline: baseline.json
          candidate: candidate.json
          threshold-pct: "15"
```

Or one call wraps bench -> diff -> sink -> drift via the
[reusable workflow](https://github.com/submillisecond/subms-actions/blob/main/.github/workflows/subms-perf-suite.yml).

## Cookbook

The [submillisecond.com cookbook](https://submillisecond.com/cookbook) is a
working notebook of low-latency engineering. 16 dual-language Rust + Java
recipes that hit sub-millisecond at p99 - bloom / cuckoo / HLL / count-min /
HDR-histogram / SPSC / MPSC / rate-limiter / timer-wheel / arena / ART /
treap / LSM / segment-reader / merge-iterator / block-cache. Each comes with
a quality bar contract, >= 90 % test coverage, and real measured bench
numbers from the [subms perf harness](https://submillisecond.com/cookbook/guides/subms-perf-harness).

## Status

**Pre-release.** The actions are functionally complete, smoke-tested via
their own self-test workflows, and ready for use. They have not yet been
through real-world adoption beyond the cookbook itself. Bug reports welcome.

Releases follow semver. Float to a major tag (`@v0`) for auto-patches; pin to
an exact tag (`@v0.1.0`) for supply-chain audit.

## Contact

- Web: [submillisecond.com](https://submillisecond.com)
- Terms: [submillisecond.com/terms](https://submillisecond.com/terms)
- Security: see each repo's `SECURITY.md` for private vulnerability reporting.

---

<sub>Released under the [MIT License](https://github.com/submillisecond/subms-actions/blob/main/LICENSE). The submillisecond name and logo are not registered trademarks; use in editorial and reference contexts is encouraged, use in product names that imply endorsement is not.</sub>
