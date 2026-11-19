# 社区回收统计的相交披露控制与定版发布资料契约

以下为项目本地合成资料，不代表真实居民、机构、金额或政策。所有数据为 UTF-8 JSON Lines，每行是一个完整对象；金额采用整数分，null 表示未知或不适用，不能自动等同于零。时间戳含 +08:00 偏移，日期为当地业务日。known_at 或 reported_at 控制资料何时可得，业务发生时间不意味着当时已经获知。

## 业务含义

发布请求采用requests的对象格式，statistics仅支持participant_count、deposit_count、amount_sum、amount_histogram，group_by仅支持community、building、category，月份不能跨月相加去重户数。participant_count为至少一笔完整事实的家庭数；不完整事实对应范围不得视为确定零。家庭月金额为完整事实合计，分箱左闭右开。公开名册的归属、每户上下限以及已发布的具体范围均为受众背景知识。保护目标针对每个家庭每月的参与指示与返利合计：联合已有公开事实和待发事实后，参与指示可行范围不能由{0,1}缩成单值，月金额可行区间宽度不能小于策略给定值；已经被背景知识完全确定的项单列不再声称保护。精确值、区间边界、分箱计数及公开补集均属于已知约束。没有证明可安全发布时可拒绝或给受支持的聚合/区间方案，但应能实际发布资料支持且满足上述保护的宽口径方案，不能把所有请求一律拒绝。历史公开值不得因撤回链接被遗忘，受众组彼此独立，只有显式同组才合并知识。

## 原始记录

### households

| 字段 | 含义 |
| --- | --- |
| id | 合成家庭身份 |
| community | 公开社区归属 |
| building | 公开楼栋归属 |
| public_amount_upper_cents | 受众已知每户月金额上限 |
| public_count_upper | 受众已知每户月次数上限 |

### deposits

| 字段 | 含义 |
| --- | --- |
| id | 事实身份 |
| household | 家庭 |
| month | 统计月份 |
| category | 品类 |
| cents | 返利分 |
| session | 投放会话 |
| complete | 该事实是否完成核对 |

### requests

| 字段 | 含义 |
| --- | --- |
| id | 请求身份 |
| audience | 受众知识共享组 |
| month | 月份 |
| community | 社区筛选，空表示全部 |
| buildings | 楼栋集合，空列表表示不限 |
| categories | 品类集合 |
| statistics | 所需统计项 |
| group_by | 分组字段 |

### policy

| 字段 | 含义 |
| --- | --- |
| id | 策略身份 |
| min_households | 最小直接披露住户数 |
| amount_width_floor_cents | 保护的单户金额可推断区间最小宽度 |
| count_width_floor | 保护的单户参与指示区间最小宽度 |
| histogram_edges_cents | 允许的月金额分箱 |
| known_membership | 受众已知名册归属 |
| audiences | 共享历史的受众组 |

### prior_releases

| 字段 | 含义 |
| --- | --- |
| id | 历史投递身份 |
| audience | 共享受众 |
| households | 确切作用家庭集合 |
| month | 月份 |
| statistic | 统计事实类型 |
| value | 实际已公布值或范围 |
| delivered_at | 实际公开时点 |

### corrections

| 字段 | 含义 |
| --- | --- |
| id | 更正身份 |
| deposit | 事实身份 |
| known_at | 到达时间 |
| household | 更正后的家庭 |
| supersedes | 明确更正引用 |

