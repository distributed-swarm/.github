Hypha — Distributed Compute Engine for Long-Running Research Workloads

Hypha is a lightweight distributed compute engine designed to execute large-scale workloads reliably on commodity hardware — without container overhead, centralized orchestration bottlenecks, or intermediate disk amplification.

Agents autonomously pull micro-batches, execute entirely in memory, and write only final structured outputs. The system scales across heterogeneous hardware while maintaining stability over multi-day execution windows.

Proven behavior under real load

Most systems can go fast for a few minutes.
Very few can run for days without falling apart.

Hypha was validated on a real H01 synapse workload:

166,216,068 records processed
166 / 166 shard jobs completed
7.1 GB Parquet output generated
59 hours continuous execution
0 pipeline failures
0 agent crashes
0 orchestration stalls

No restarts. No babysitting. No partial outputs.

This is not a synthetic benchmark — it is a sustained execution test under real data.

What it does differently

Most pipeline frameworks assume disk is cheap and abundant.

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
Engine characteristics

During the H01 run:

96-worker execution capacity on a CPU agent
64-task concurrent leasing (controller-safe cap)
sustained multi-hour execution with no degradation
stable controller–agent coordination throughout

Hypha behaves more like a pull-based compute mesh than a traditional pipeline runner.

Benchmarks
Workload	Hardware	Time	Notes
166M H01 synapse records → Parquet	Dual Xeon (13-year-old server)	sustained 59h run	zero failures, full completion
10M synapse subset	same system	~24 min	linear scaling behavior
H01 connectome feature extraction	AMD mini PC	~58 sec	micro-op efficiency
Neuroimaging QC (5 subjects, 3 agents)	commodity CPU cluster	7.4 sec	4.2× speedup vs sequential

Hardware is intentionally unimpressive. That is the constraint Hypha is built for.

Architecture
Controller
lightweight REST API
lease-based scheduling
epoch fencing prevents stale execution
Agents
pull work autonomously
execute in memory
scale to hardware limits without central coordination
Operations
micro-batch compute units
composable without shared intermediate state
Failure model
agent loss → automatic lease recovery
no global pipeline failure
Why this matters

The hard problem is not making pipelines fast.
The hard problem is making them finish reliably at scale.

Hypha demonstrates:

If work is submitted, it will complete — even across long-running workloads on commodity hardware.

Current focus
Connectome-scale pipelines (H01, HCP)
Neuroimaging QC and validation
Edge and low-cost hardware execution
Eliminating I/O bottlenecks in scientific workflows
Contact

Open an issue or start a discussion.
Also active on NeuroStars
.
