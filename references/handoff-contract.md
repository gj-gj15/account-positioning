# 交接契约 · account-positioning → content-factory（双通道内容工厂）

> 本文件定义起号定位技能的机读产物如何被双通道内容工厂直接消费。工厂侧事实来自 content-factory v2.2（`plugins/marketplaces/my-experts/plugins/content-factory/`）：其 01-discover 阶段读取 `outputs/factory-config.json` 获取平台/行业默认值，再走 12 心法预筛 → 8 维打分 → 标题公式。

## 一、account-positioning.json 完整 Schema

```json
{
  "account_name": "账号名（可空，起号前可后补）",
  "created_at": "ISO8601，含时区",
  "positioning": {
    "statement": "我为【受众】提供【核心结果】，区别于【替代方案】，优势是【独特资源】",
    "target_audience": "谁，什么处境（6 字内能说清）",
    "core_pain": "受众反复遇到的问题",
    "why_you": "不可复制的优势（过复制测试）",
    "why_now": "窗口判断：红利还有多宽"
  },
  "platforms": {
    "primary": "主平台代号",
    "secondary": ["辅平台代号"],
    "roles": {
      "douyin": "一句话分工，如：拉新曝光"
    }
  },
  "persona": {
    "identity_label": "受众第一眼记住的一个词",
    "memory_hook": "口头禅/视觉符号/固定开场",
    "tone_do": ["语言风格宜"],
    "tone_dont": ["语言风格忌"]
  },
  "content_pillars": [
    { "name": "支柱名", "solves": "替受众解决什么", "share": 0.4 }
  ],
  "monetization": {
    "primary": "主变现模式",
    "backup": "备选模式",
    "tone_alignment": "内容调性如何服务变现"
  },
  "opportunity_score": {
    "audience_demand": 0, "leverage_density": 0, "window_bonus": 0,
    "resource_fit": 0, "standardization": 0, "monetization_clarity": 0, "total": 0
  },
  "factory_handoff": {
    "factory_config_patch": {
      "platforms": ["douyin", "wechat"],
      "industry": "科技",
      "keywords": []
    },
    "topic_scope": "内容支柱一句话汇总，供 01-discover 收窄找热点范围",
    "inject_target": "outputs/factory-config.json"
  }
}
```

## 二、平台代号对照表

**账号发布平台**（platforms 字段）与**工厂热搜平台**（factory_config_patch.platforms）使用同一套代号，二者语义不同、取值规则不同：

| 代号 | 平台 | 作为账号平台 | 作为热搜来源 |
|---|---|---|---|
| weibo | 微博 | 辅（扩散） | ✅ 微博热搜 |
| douyin | 抖音 | 常作主 | ✅ 抖音热点 |
| baidu | 百度 | 极少 | ✅ 百度热搜 |
| zhihu | 知乎 | 辅（深度长文） | ✅ 知乎热榜 |
| toutiao | 今日头条 | 辅 | ✅ 头条热榜 |
| bilibili | B站 | 可作主（中长视频） | ✅ B站热榜 |
| xiaohongshu | 小红书 | 可作主（图文/短视频） | ✅ 小红书热点 |
| kuaishou | 快手 | 辅（下沉） | ✅ 快手热榜 |
| wechat | 微信公众号 | 辅（私域沉淀，无公开热搜） | ❌ 不可作热搜来源 |
| shipinhao | 微信视频号 | 可作主（信任转化） | ❌ 不可作热搜来源 |
| all | 全网聚合 | — | ✅ 兜底 |

**映射规则**：`factory_config_patch.platforms` = 账号主平台的同平台热搜代号；主平台无公开热搜（wechat/shipinhao）时，补 `all` 兜底。

## 三、合并规则（写入工厂 factory-config.json 时）

工厂 `outputs/factory-config.json` 结构：

```json
{
  "platforms": ["weibo", "douyin"],
  "industry": "科技",
  "keywords": [],
  "updatedAt": "ISO8601"
}
```

1. **先读后写**：读取现有 config，仅当字段缺失或用户明确要求覆盖时才写
2. **不覆盖已有默认值**：定位产出的是「建议值」，工厂已有配置优先
3. **口头确认**：写入前必须问用户「是否把平台/行业默认值写进工厂的 outputs/factory-config.json？」
4. **写后更新** `updatedAt` 为当前时间

## 四、交接动作清单

1. 定位确认 → 落盘 `account-positioning/` 三件套（positioning-statement.md / analysis.md / account-positioning.json）
2. 提示用户：「召唤 content-factory 总控时，把 `account-positioning.json` 的路径给它，或授权把 factory_config_patch 合入工厂配置」
3. 工厂 01-discover 的找热点范围 = factory_config_patch.platforms + industry + keywords；选题池收窄 = topic_scope

## 变更记录

- 2026-10-16：初版。契约对齐 content-factory v2.2 的 01-discover 入口与 factory-config.json 字段。（account-positioning v1.0 配套 references）
