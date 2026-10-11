# zz_bulk_resettle — Stellaris 批量迁移（补岗 / 超额 / 完全）

适用于 **Stellaris 4.4 (Pegasus)** 的批量迁移 QoL mod：在目标星球的「决策」栏一键完成三个不同力度的“强制迁移”，告别原版迁移窗口一次点击只搬 100 人口的重复操作。
Enables three colony decisions that bulk-migrate *civilian-type* pops (Civilian / Maintenance Drone) between your colonies.

**Steam 创意工坊**：https://steamcommunity.com/sharedfiles/filedetails/?id=3795384348
（若条目页暂不可访问，说明作者尚未把条目设为公开，公开后即可订阅。）

---

## 三个决议（在“被迁入”的目标星球 → 决策）

| 决议 | 作用 |
|---|---|
| **补岗迁移** | 自动从帝国其它殖民地调集平民迁入，填满本星球的空缺岗位（岗位补满 / 源耗尽 / 钱不够即停） |
| **超额迁移** | 有空岗时先补岗（同补岗迁移），再把平民人口最多的源星球的平民**全部**迁入；无空岗时直接迁入 |
| **完全迁移** | 把其它所有殖民地的平民**全部**迁入本星球（不限岗位/住房，可能失业拥挤，钱不够即停） |

批量逻辑：大数量按 100 人口一批迁移，尾数逐个迁移。

## “平民”指什么

= pop 分组中 `is_civilian_job` 类的两类：有机/机器人帝国的 **平民（Civilian）** 与格式塔帝国的 **维护子个体（Maintenance Drone）**。
**失业人口与在岗人口永远不会被迁移。**

## 费用

按原版基础价从国库扣除：每 100 人口 = 100 能量 + 10 凝聚力（即每 1 人口 = 1 能量 + 0.1 凝聚力）；余额不足自动提前停止并弹窗说明原因。

## 使用方法

1. 在创意工坊页点「订阅」；
2. Steam 自动下载；Paradox Launcher 的模组列表会自动出现本 mod；
3. 在 Launcher 中勾选启用，进入游戏后在目标星球的「决策」栏使用。

> 更新也是自动的：作者发布新版本后，Steam 会为订阅者自动同步。
> 非 Steam / 离线玩家：把本仓库 `zz_bulk_resettle` 文件夹放入 `文档\Paradox Interactive\Stellaris\mod\zz_bulk_resettle\`，并在 `mod\` 目录放置同名 `.mod` 描述文件，再在 Launcher 中启用。

## 兼容性与说明

- 仅适用于 **4.4.\***（Pegasus）；纯新增文件，不覆盖任何原版内容，与绝大多数模组（含 UI mod）兼容。
- 简体中文 / English 界面均可用。
- 「超额迁移」会准确选择可迁移的平民/维护子个体数量最多的来源星球；数量相同的星球按引擎顺序选择。
- 正被轨道轰炸或发生地面战斗的星球不会作为迁移来源。
- 迁入人口会在决议结束时触发一次原版岗位重新分配；仍无法胜任剩余岗位的人口会保持失业。

## 反馈

- 创意工坊条目「讨论」区
- GitHub Issues：https://github.com/ohhhniceeeeee/stellaris-bulk-resettle/issues

---

*Made for fun. 群星 4.4 批量迁移 QoL。*
