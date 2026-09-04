# zz_bulk_resettle — Stellaris 批量迁移（补岗 / 超额 / 完全）

适用于 **Stellaris 4.4 (Pegasus)** 的纯增量 mod：通过**星球决策**批量迁移“居民类人口”到当前星球。
Enables three colony decisions that bulk-migrate *civilian-type* pops (Civilian / Maintenance Drone) between your colonies — no vanilla file is overridden.

> 背景：原版“强制迁移”窗口一次点击只迁移 100 人口且无批量手段；而“按修饰键（Ctrl/Alt）点击”与“动态列表/排序取最大”在官方脚本层不受支持，故本 mod 采用「决策 + 全自动脚本」形态实现同等目的。

---

## 1. “平民”指什么

本 mod 全程的“平民/居民类人口”＝pop 分组里带 `is_civilian_job = yes` 的两类（官方定义见 `common/pop_categories/`）：

| 数据类别 | 说明 |
|---|---|
| `civilian` | 有机/机器人帝国的“平民” |
| `maintenance_drone` | 格式塔帝国的“维护子个体” |

**失业人口与在岗人口一律不迁移。**（失业人口按原版机制会逐渐降阶为平民，无需本 mod 处理。）

## 2. 三个决议

决议出现在**目标星球**（被迁入的星球）的「决策」栏：

| 决议 | 行为 | 额外说明 |
|---|---|---|
| **补岗迁移** | 自动从帝国其它殖民地调集平民，迁入以**填满当前星球空缺岗位** | 岗位补满 / 源耗尽 / 钱不够即停 |
| **超额迁移** | = 先执行「补岗迁移」，再额外把**一颗平民较多的源星球**的平民全部迁入 | “较多”= 按平民数量**分档加权随机**（近似最大，见 §4） |
| **完全迁移** | 把**其它所有殖民地**的平民全部迁入当前星球 | 不限岗位/住房（可能造成失业与拥挤），钱不够即停 |

批量逻辑：大数量按 **100 人口一批** 迁移（调用官方 `resettle_pop_group` 原语），尾数逐个迁移。

## 3. 费用

按原版基础价从国库扣除（不含 `pop_resettlement_cost_mult` 等修正系数）：

- 能量 **100 / 人口**，凝聚力 **10 / 人口**；
- 即每 100 人口一批 = 10,000 能量 + 1,000 凝聚力；
- 余额不足时提前停止，并弹窗说明原因。

## 4. 已知限制（重要）

1. **“平民最多的星球”是近似值**：脚本引擎没有排序/取最大原语，跨星球数值也无法互比（作用域间不能读值）。超额迁移使用**分档加权**（人数越多权重越高：>100 ×2、>500 ×3、>2000 ×4、>8000 ×5），体感上几乎总选中最大的一颗，但不是数学保证。权重档位写在 `common/scripted_effects/zz_bulk_resettle_effects.txt` 的 `zz_br_do_excess` 中，可自行调陡。
2. **费用为近似原版**：未叠加政令/权威/特性等 `pop_resettlement_cost_mult` 修正。
3. **迁入人口就职由原版结算**：迁移完成后岗位分配走原版月度重分配，短期内可能显示少量“失业”，属正常节奏。
4. **战争限制**：正被轨道轰炸或发生地面战斗的星球不会作为源星球。
5. **排错**：若决策不显示或点击无反应，查看 `文档\Paradox Interactive\Stellaris\logs\error.log`，把相关报错反馈给作者。

## 5. 文件结构

```
zz_bulk_resettle/
├─ descriptor.mod
├─ common/decisions/zz_bulk_resettle_decisions.txt       # 三个决议（可见/允许条件）
├─ common/scripted_effects/zz_bulk_resettle_effects.txt  # 执行逻辑与费用
├─ events/zz_bulk_resettle_events.txt                    # 结果提示事件
└─ localisation/{simp_chinese,english}/zz_bulk_resettle_l_*.yml
```

全部为**新增文件**，不覆盖任何原版内容，与多数 mod（含 UI mod）天然兼容。

## 6. 安装

1. 把 `zz_bulk_resettle` 文件夹复制到 `文档\Paradox Interactive\Stellaris\mod\zz_bulk_resettle\`；
2. 在上层 `mod\` 目录新建文本文件 `zz_bulk_resettle.mod`：

```
name="zz_bulk_resettle - 批量调集平民填补岗位"
path="mod/zz_bulk_resettle"
tags={
	"Gameplay"
	"Population"
}
supported_version="4.4.*"
```

3. 启动器（Launcher）中勾选启用，进入游戏后在目标星球的「决策」栏使用。

## 7. 开发提示

- 文本文件请保持 **UTF-8 无 BOM**；Windows 下 git 会把 LF 自动转 CRLF，属正常。
- 决策 id 与本地化键同名：改文案只动 `localisation/` 即可；改行为看 `common/scripted_effects/`。
- 目标版本：`supported_version="4.4.*"`（Pegasus）。

---

*Made for fun. 群星 4.4 本地批量迁移 QoL。*
