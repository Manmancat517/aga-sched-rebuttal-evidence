# 审稿人 HQFL14 实验证据

## Table 1. DFJSP prompt-only 目标适配

| Scale | 相对 fixed no-LLM 的 TD 改善 | 相对 structured goal rule 的 TD 改善 | AGA-Sched 适配方式 |
|---|---:|---:|---|
| 25x8 | 23.62% | 17.69% | 仅 prompt |
| 30x10 | 42.83% | 29.27% | 仅 prompt |

StateBuilder、合法动作生成、约束逻辑、可执行策略代码和模型权重均不变。No-LLM 在已经编码的固定目标上可以很强，但无法解释自由语言目标；每个新目标或组合仍需要 parser、score 或 switching code。

## Table 2. RCPSP urgent 目标比较

| Scale | 方法 | Mean MK | Urgent flow | Completion | Violations | 适配 |
|---|---|---:|---:|---:|---:|---|
| J30 | CPL/GRPW/MINSLK | 158.8/159.8/158.8 | 57.53/60.73/57.53 | 100% | 0 | 无 |
| J30 | Frozen PPO | 166.2 | 57.80 | 100% | 0 | 训练后无 |
| J30 | Urgent-CPL | 167.4 | **11.80** | 100% | 0 | 新增规则代码 |
| J30 | AGA-Sched | 169.2 | 12.00 | 100% | 0 | 仅 prompt |
| J60 | CPL/GRPW/MINSLK | 215.2/220.0/215.2 | 72.05/56.95/72.05 | 100% | 0 | 无 |
| J60 | Frozen PPO | 220.8 | 50.15 | 100% | 0 | 训练后无 |
| J60 | Urgent-CPL | 226.0 | **15.50** | 100% | 0 | 新增规则代码 |
| J60 | AGA-Sched | 226.2 | 16.10 | 100% | 0 | 仅 prompt |

AGA-Sched 相对 frozen PPO 改善 79.24%/67.90%，相对最佳未修改规则改善 79.14%/71.73%。Urgent-CPL 仅在增加目标专用规则代码后优于 AGA-Sched 1.69%/3.87%。

## Table 3. 因果重调度

| Scale | 方法 | Mean MK | Urgent flow |
|---|---|---:|---:|
| J30 | AGA-Sched / Search 0.1s / Search 1.0s | 169.2/167.0/167.8 | **12.00**/12.20/13.00 |
| J60 | AGA-Sched / Search 0.1s / Search 1.0s | 226.2/218.2/215.6 | 16.10/16.90/**14.90** |

AGA-Sched、0.1s search、1.0s search 的 pooled urgent flow 为 14.05/14.55/13.95。30 次运行均完成且零违规；search 需要目标专用评分和 rollout。

## Table 4. 2,271 个 DFJSP 决策的动作来源

| 结果 | 决策数 | 比例 |
|---|---:|---:|
| Direct unchanged | 2,228 | 98.11% |
| Job retained, machine corrected | 2 | 0.09% |
| Fallback replacement | 41 | 1.81% |

## Table 5. 完整 AGA-Sched 与 fallback-only

| Scale | n | AGA-Sched TD | Fallback TD | AGA-Sched TD 改善 |
|---|---:|---:|---:|---:|
| 10x4 | 6 | 363.5 | 704.8 | 48.4% |
| 15x5 | 5 | 1660.0 | 2276.8 | 27.1% |
| 20x7 | 5 | 632.2 | 1298.8 | 51.3% |
| 25x8 | 5 | 362.6 | 1078.6 | 66.4% |
| 30x10 | 5 | 472.2 | 828.4 | 43.0% |

Fallback-only 全部完成；AGA-Sched 将 scale-normalized TD 降低 47.29%（95% CI [30.04, 65.16], paired sign-flip p<.0001）。
