# Evidence for Reviewer HQFL14

## Table 1. Prompt-only DFJSP objective adaptation

| Scale | TD gain vs fixed no-LLM | TD gain vs structured goal rule | AGA-Sched adaptation |
|---|---:|---:|---|
| 25x8 | 23.62% | 17.69% | Prompt only |
| 30x10 | 42.83% | 29.27% | Prompt only |

StateBuilder, valid-action generation, constraint logic, executable policy code, and model weights are unchanged. The no-LLM controller can be strong for an already encoded objective, but it has no mechanism for interpreting a free-form objective; each new goal or composition needs parser/score/switching code.

## Table 2. RCPSP urgent-objective comparison

| Scale | Method | Mean MK | Urgent flow | Completion | Violations | Adaptation |
|---|---|---:|---:|---:|---:|---|
| J30 | CPL | 158.8 | 57.53 | 100% | 0 | None |
| J30 | GRPW | 159.8 | 60.73 | 100% | 0 | None |
| J30 | MINSLK | 158.8 | 57.53 | 100% | 0 | None |
| J30 | Frozen direct PPO | 166.2 | 57.80 | 100% | 0 | None after training |
| J30 | Urgent-CPL | 167.4 | **11.80** | 100% | 0 | New rule code |
| J30 | AGA-Sched | 169.2 | 12.00 | 100% | 0 | Prompt only |
| J60 | CPL | 215.2 | 72.05 | 100% | 0 | None |
| J60 | GRPW | 220.0 | 56.95 | 100% | 0 | None |
| J60 | MINSLK | 215.2 | 72.05 | 100% | 0 | None |
| J60 | Frozen direct PPO | 220.8 | 50.15 | 100% | 0 | None after training |
| J60 | Urgent-CPL | 226.0 | **15.50** | 100% | 0 | New rule code |
| J60 | AGA-Sched | 226.2 | 16.10 | 100% | 0 | Prompt only |

AGA-Sched improves urgent flow over frozen PPO by 79.24%/67.90% and over the best unmodified rule by 79.14%/71.73%. Urgent-CPL is 1.69%/3.87% better only after adding objective-specific executable rule code.

## Table 3. Matched causal rescheduling; five seeds per scale

| Scale | Method | Mean MK | Urgent flow |
|---|---|---:|---:|
| J30 | AGA-Sched | 169.2 | **12.00** |
| J30 | Search, 0.1s | 167.0 | 12.20 |
| J30 | Search, 1.0s | 167.8 | 13.00 |
| J60 | AGA-Sched | 226.2 | 16.10 |
| J60 | Search, 0.1s | 218.2 | 16.90 |
| J60 | Search, 1.0s | 215.6 | **14.90** |

Pooled urgent flow is 14.05 for AGA-Sched, 14.55 for 0.1s search, and 13.95 for 1.0s search. All 30 runs complete with zero violations. Search uses only current state but requires an objective-specific score and rollout.

## Table 4. Qwen-3-32B action provenance over 26 DFJSP runs

| Outcome | Decisions | Rate |
|---|---:|---:|
| Direct, unchanged | 2,228 | 98.11% |
| Job retained, machine corrected | 2 | 0.09% |
| Fallback replacement | 41 | 1.81% |
| Total | 2,271 | 100.00% |

## Table 5. Full AGA-Sched versus fallback-only

| Scale | n | AGA-Sched MK | Fallback MK | AGA-Sched TD | Fallback TD | AGA-Sched TD gain |
|---|---:|---:|---:|---:|---:|---:|
| 10x4 | 6 | 1207.2 | 1211.0 | 363.5 | 704.8 | 48.4% |
| 15x5 | 5 | 1436.4 | 1332.4 | 1660.0 | 2276.8 | 27.1% |
| 20x7 | 5 | 1210.6 | 1201.4 | 632.2 | 1298.8 | 51.3% |
| 25x8 | 5 | 1373.2 | 1337.0 | 362.6 | 1078.6 | 66.4% |
| 30x10 | 5 | 1640.8 | 1640.8 | 472.2 | 828.4 | 43.0% |

Fallback-only completes all 26 scenarios; AGA-Sched lowers scale-normalized TD by 47.29% (paired-bootstrap 95% CI [30.04, 65.16], paired sign-flip p<.0001). The Enforcer explains feasibility; the paired quality comparison measures the LLM-controlled decisions.
