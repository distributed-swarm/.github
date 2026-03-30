# Hypha — Distributed Compute for Research Workloads

Hypha is a lightweight distributed mesh that dispatches workloads as micro-operations across autonomous agents — no containers, no centralized orchestration, no intermediate temp file accumulation.

Agents pull work, process in memory, and write only final structured output to shared storage. The result is a system that scales across commodity hardware without the I/O bloat that causes traditional monolithic pipelines to fail.

---

## What it does differently

Most pipeline frameworks assume disk is cheap and abundant. Hypha assumes it isn't.

Instead of writing intermediate derivatives to scratch space between steps, operations are dispatched as micro-batches, executed in agent memory, and reconciled into a final output manifest. This eliminates the class of failures — `[Errno 28] No space left on device` and its cousins — that plague large neuroimaging and connectome workloads running in constrained environments.

---

## Benchmarks on real workloads

| Workload | Hardware | Time | Peak Memory | Disk failures |
|---|---|---|---|---|
| 10M H01 synapse records (7.1GB JSON → Parquet) | 13-year-old Dell workstation | 24 min | 16.2 GB | 0 |
| 13.6M neurons / 9.6M edges, H01 connectome feature extraction | $800 AMD 16-core mini PC | ~58 sec | — | 0 |
| Neuroimaging pipeline, 5 subjects, 3 agents (ColeAnticevic atlas) | Commodity CPU cluster | 7.4 sec | — | 0 (4.2× speedup vs sequential) |

Hardware is intentionally unimpressive. That's the point.

---

## Architecture in brief

- **Controller** — lightweight REST API, manages leases and epoch fencing
- **Agents** — pull micro-batches autonomously, no central push required
- **Operations** — composable units that chain into pipelines without shared temp state
- **Fault tolerance** — agent loss mid-run reclaims and requeues automatically

The routing and dispatch logic is proprietary. Everything else is standard Python.

---

## Current focus

Active benchmarking against neuroimaging QC and connectome workloads. Interested in research collaborations where pipeline scaling, I/O bottlenecks, or compute cost on legacy hardware are the constraint.

If you're working on a dataset that's too slow or too big for your current setup, happy to run a benchmark and share the numbers.

---

## Contact

Open an issue, start a discussion, or reach out directly.  
Also active on [NeuroStars](https://neurostars.org/u/maysjack).
