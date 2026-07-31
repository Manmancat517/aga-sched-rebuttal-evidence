# 审稿人 A3SE03 实验证据

## 能力边界

LLM 的贡献是运行时语言条件化适配：解释自然语言目标，并在合法动作集中形成状态相关的动作偏好。StateBuilder 提供调度特征，router 提供安全策略 scaffold，Enforcer 保证可行性。JSON 是有意选择的、便于模型生成和机器校验的序列化格式；它不是性能增益来源。

## Table 1. 语言理解与同状态适配

| 指标 | 25x8 | 30x10 / probe |
|---|---:|---:|
| Keyword parser 隐含目标正确解析 | 0/20 | 0/20 |
| AGA-Sched 相对 keyword controller 的 TD 改善 | 54.24% | 58.87% |
| Reference-action agreement 改善 | — | 40 states 上 +21.88 个百分点 |

## Table 2. 26 个 matched 历史 DFJSP 场景的绝对均值

| Scale | n | AGA-Sched MK | No-LLM MK | AGA-Sched TD | No-LLM TD | Fallback TD |
|---|---:|---:|---:|---:|---:|---:|
| 10x4 | 6 | 1207.2 | 1231.3 | 363.5 | 475.7 | 704.8 |
| 15x5 | 5 | 1436.4 | 1349.2 | 1660.0 | 1367.0 | 2276.8 |
| 20x7 | 5 | 1210.6 | 1194.6 | 632.2 | 555.2 | 1298.8 |
| 25x8 | 5 | 1373.2 | 1431.8 | 362.6 | 500.2 | 1078.6 |
| 30x10 | 5 | 1640.8 | 1650.6 | 472.2 | 502.2 | 828.4 |

No-LLM 保留相同 StateBuilder、router context、合法动作、事件流和 Enforcer，仅以确定性模板规则替代 LLM。在固定目标上，它被有意设计为强对照：26 个场景上 AGA-Sched 相对 no-LLM 的 MK/TD 差异均不显著。其缺陷是规则目录封闭，无法解释自由语言目标、同义改写或新目标组合；每新增目标仍需增加 parser、score 或 switching rule。

## Table 3. 10 个 paired scenarios 的策略内目标响应

| 方法 | Throughput MK 改善 | Due-date TD 改善 | 双向响应 |
|---|---:|---:|---:|
| AGA-Sched | 0.31% (p=.4062) | **33.14%** (p=.0234) | 3/10 |
| Frozen no-LLM | 0.00% | 0.00% | 0/10 |
| 人工 goal rule | 0.74% (p=.3750) | 57.72% (p=.0020) | 3/10 |

实验只改变运行时指令。No-LLM 没有语言条件通道，因此两个 prompt 下轨迹相同；AGA-Sched 的 due-date effect 的 paired-bootstrap 95% CI 为 [10.92, 57.12]。

## Table 4. RCPSP 目标适配

| Scale | 方法 | MK | Urgent flow | 目标适配 |
|---|---|---:|---:|---|
| J30 | Frozen PPO | 166.2 | 57.80 | 训练后无适配 |
| J30 | 最佳未修改规则 | 158.8 | 57.53 | 无 |
| J30 | Urgent-CPL | 167.4 | **11.80** | 新增规则代码 |
| J30 | AGA-Sched | 169.2 | 12.00 | 仅 prompt |
| J60 | Frozen PPO | 220.8 | 50.15 | 训练后无适配 |
| J60 | 最佳未修改规则 | 220.0 | 56.95 | 无 |
| J60 | Urgent-CPL | 226.0 | **15.50** | 新增规则代码 |
| J60 | AGA-Sched | 226.2 | 16.10 | 仅 prompt |

AGA-Sched 相对 frozen PPO 改善 79.24%/67.90%，相对最佳未修改规则改善 79.14%/71.73%。Urgent-CPL 在人工编码目标后优于 AGA-Sched 1.69%/3.87%，说明固定目标专用规则很强；AGA-Sched 的优势是无需新增可执行规则即可进入相同目标性能区间。

## Table 5. 因果在线搜索

| Scale | 方法 | MK | Urgent flow |
|---|---|---:|---:|
| J30 | AGA-Sched | 169.2 | **12.00** |
| J30 | Search, 0.1s | 167.0 | 12.20 |
| J30 | Search, 1.0s | 167.8 | 13.00 |
| J60 | AGA-Sched | 226.2 | 16.10 |
| J60 | Search, 0.1s | 218.2 | 16.90 |
| J60 | Search, 1.0s | 215.6 | **14.90** |

## Table 6. 目标适配前沿

| Controller | Pooled urgent flow | 新目标适配 |
|---|---:|---|
| 最佳未修改规则 | 57.24 | 无 |
| Frozen PPO | 53.98 | 训练后无 |
| Urgent-CPL | 13.65 | 新增规则代码 |
| Causal search 0.1/1.0s | 14.55/13.95 | 新增 score/rollout |
| AGA-Sched | **14.05** | 仅运行时语言 |

## Table 7. 动作来源与 fallback

| 结果/对照 | 决策或完成率 | 比例或 TD 差异 |
|---|---:|---:|
| Direct unchanged | 2,228 | 98.11% |
| Machine corrected | 2 | 0.09% |
| Fallback replacement | 41 | 1.81% |
| Fallback-only | 100% completion | +47.29% scale-normalized TD |

安全性归因于 Enforcer，而不是 LLM。本文的 unseen 仅指已支持事件类型的 held-out realizations、参数和组合。
