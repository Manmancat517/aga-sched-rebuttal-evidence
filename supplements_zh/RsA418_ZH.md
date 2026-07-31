# 审稿人 RsA418 实验证据

DFJSP 由脱敏的汽车机加工/装配流程简化得到，RCPSP 由某重大工业装置安装排程简化得到；保留工艺路线、前置关系、资源、工时和交期关系。扰动由脚本生成，所有方法面对相同 seed 和因果事件流。实验验证 MES/CPS 调度层，不把原始 IIoT 识别或现场闭环部署作为已完成评价。

## Table 1. 调度层接口鲁棒性

| 范围 | Runs/decisions | 完成率 | 执行动作失败/违规 | Direct/corrected/fallback |
|---|---:|---:|---:|---:|
| 全部条件 | 70/8,606 | 100% | 0/0 | 95.04%/0.10%/4.86% |
| Clean input | — | 100% | 0/0 | 98.37%/0.17%/1.46% |

测试事件缺失、延迟、矛盾、malformed input、10% API outage 和 burst outage。故障相对 clean 的 pooled MK/TD 最大变化为 2.75%/9.50%。

## Table 2. 论文 DFJSP 30x10 组件消融

| Variant | MK | TD |
|---|---:|---:|
| Full AGA-Sched | 1454 | **349** |
| w/o Meta-Agent | 1454 | 1092 |
| w/o State Summary | 1461 | 965 |

## Table 3. 语义标签与状态信息

| 扰动 | 25x8 TD 变化 | 30x10 TD 变化 |
|---|---:|---:|
| 中性标签名，保留数值状态 | 无稳定损失 | 无稳定损失 |
| 标签与数值状态打乱 | +16.69% | +16.81% |
| 删除 slack | +308.90% | +140.44% |
| 删除 bottleneck | +44.91% | +22.90% |

## Table 4. 状态特征消融

| Scale | Variant | MK | TD | Completion |
|---|---|---:|---:|---:|
| 25x8 | Full | 1367.80±226.99 | 296.60±258.81 | 100% |
| 25x8 | No bottleneck | 1391.00±217.36 | 429.80±331.60 | 100% |
| 25x8 | No slack | 1407.80±215.30 | 1212.80±461.96 | 100% |
| 30x10 | Full | 1641.40±252.31 | 481.20±459.57 | 100% |
| 30x10 | No bottleneck | 1640.80±253.25 | 591.40±530.87 | 100% |
| 30x10 | No slack | 1638.20±251.49 | 1157.00±755.58 | 100% |

这些结果说明 state feature 的作用，不说明某几个括号标签字符串不可替代。

## Table 5. 单因素事件权重

| Scale | Profile | Mean MK | Mean TD | Route change |
|---|---|---:|---:|---:|
| 25x8 | Nominal | 1357.2 | 415.2 | 0.00% |
| 25x8 | Urgent high/low | 1381.2/1381.4 | **361.6**/509.2 | 6.31%/2.88% |
| 30x10 | Nominal | 1641.0 | **360.0** | 0.00% |
| 30x10 | Urgent high/low | 1639.6/1639.6 | 363.2/530.2 | 0.00%/0.00% |

完整 70 次运行均完成且零可行性失败；所有权重 profile 的 mean MK 相对 nominal 最大偏差 2.00%。降低 urgent weight 使 TD 上升 22.64%/47.28%。

## Table 6. 联合权重缩放

| Scale | 0.8 MK/TD | Nominal MK/TD | 1.2 MK/TD |
|---|---:|---:|---:|
| 25x8 | 1375.2/421.6 | 1379.8/324.6 | 1398.2/492.8 |
| 30x10 | 1640.2/526.8 | 1640.8/476.2 | 1639.4/487.0 |

Mean MK 最大变化 1.33%。H(t) 表述为 Disturbance Intensity Score，而不是信息论 entropy。

## Table 7. 联合阈值扰动

| Profile | Util. | Recovery | Balanced | Gap | 25x8 MK/TD | 30x10 MK/TD |
|---|---:|---:|---:|---:|---:|---:|
| Low | 0.40 | 40 | 80 | 16 | 1379.4/453.2 | 1637.2/487.4 |
| Nominal | 0.50 | 50 | 100 | 20 | 1350.8/395.2 | 1640.8/323.4 |
| High | 0.60 | 60 | 120 | 24 | 1397.4/492.2 | 1640.8/553.6 |

联合 ±20% 变化下 mean MK 最大变化 3.33%；nominal 参数在测试前固定。

## Table 8. 完整 AGA-Sched 与 fallback-only

| Controller | Completion | Scale-normalized TD |
|---|---:|---:|
| Full AGA-Sched | 100% | Reference |
| Fallback-only | 100% | +47.29% |
