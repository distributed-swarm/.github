Hypha — Distributed Compute Engine for Long-Running Research Workloads

Hypha is a lightweight distributed compute engine designed to execute large-scale workloads reliably on commodity hardware — without container overhead, centralized orchestration bottlenecks, or intermediate disk amplification.

Agents autonomously pull micro-batches, execute entirely in memory, and write only final structured outputs. The system scales across heterogeneous hardware while maintaining stability over multi-day execution windows.

What it actually proves

Most systems can go fast for a few minutes.
Very few can run for days without falling apart.

Hypha was validated on a real H01 synapse workload:

166,216,068 records processed
166 shard jobs completed
7.1 GB structured output generated
59 hours continuous execution
0 pipeline failures
0 agent crashes
0 orchestration stalls

No restarts. No babysitting. No partial outputs.

This is not a synthetic benchmark — it is a sustained execution test under real data load.

What it does differently

Most pipeline frameworks assume disk is cheap and infinite.

Hypha assumes:

disk is slow
scratch space is fragile
intermediate artifacts are failure multipliers

Instead:

operations execute in memory
micro-batches flow directly through agents
only final artifacts are written

This removes entire classes of failures:

disk exhaustion (No space left on device)
partial intermediate corruption
pipeline restarts due to I/O pressure
Engine behavior under load

During the H01 run:

96-worker execution capacity on CPU agent
64-task concurrent leasing (controller-safe cap)
sustained multi-hour workload without degradation
stable controller-agent coordination throughout

Hypha does not rely on:

centralized schedulers pushing work
static worker pools
fragile DAG state on disk

It behaves more like a pull-based compute mesh than a pipeline runner.

Benchmarks
Workload	Hardware	Time	Notes
166M H01 synapse records → Parquet	Dual Xeon (13-year-old server)	sustained 59h run	zero failures, full completion
10M synapse subset	same system	~24 min	linear scaling behavior
H01 connectome feature extraction	AMD mini PC	~58 sec	demonstrates micro-op efficiency
Neuroimaging QC pipeline (5 subjects)	3 agents	7.4 sec	4.2× speedup vs sequential

Hardware is intentionally unimpressive. That is the constraint Hypha is built for.

Architecture (minimal, but strict)
Controller
REST-based lease system
epoch fencing prevents stale work corruption
Agents
pull work autonomously
execute in memory
scale to hardware limits without coordination overhead
Operations
micro-batch units
composable without shared intermediate state
Failure model
agent loss → automatic lease recovery
no global pipeline failure
Why this matters

The hard problem is not making pipelines fast.
The hard problem is making them finish reliably at scale.

Hypha demonstrates:

If work is submitted, it will complete — even across long-running, large-scale workloads on commodity hardware.

Current focus
Connectome-scale pipelines (H01, HCP)
Neuroimaging QC at scale
Edge / low-cost hardware deployment
Eliminating I/O bottlenecks in scientific pipelines
Contact

Open an issue, start a discussion, or reach out directly.
Also active on NeuroStars.
