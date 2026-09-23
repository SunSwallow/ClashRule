# 上游同步与个人定制

本项目采用“最新上游 + SunSwallow 个人定制”的同步方式。

- 上游：<https://github.com/cutethotw/ClashRule>
- 本次上游基准：`cfc84be863e9ea46e70d371d20d2659199fa727d`（2026-08-30）
- 同步日期：2026-09-22
- 同步前版本：`881d5d29cb093473533e27c6f83dd3724cef889c`
- 回退分支：`backup/before-upstream-sync-20260922`

## 配置入口与文件

个人订阅继续使用 `ClashRule.ini`；`GeneralClashRule.ini` 保留上游默认配置供对照，其远程规则仍指向上游，不是个人订阅入口。

个人直连和代理列表采用上游的新文件名 `Rule/Custom_Direct.list` 和 `Rule/Custom_Proxy.list`。`Rule/Inside.list`、`Rule/Outside.list` 是内容相同的旧 URL 兼容副本；修改主文件时必须同步副本。

`ClashRule.ini` 中的个人列表、ChatGPT、Claude 和聚合 AI 规则均引用本仓库的 `main`。本地验证使用工作区对应文件；将最终版本合入并推送 `main` 后，远程订阅才能读取本次新增文件。

## 保留的个人定制

- “🧚 AI节点”组手动选择香港、日本、美国节点及全部符合自建筛选条件的节点；AI 策略可选择该组或自建节点。
- 自建节点使用原有表达式并加入 `rak`，完整关键词为 `自建|Craig|hk-vmess|HY|海晏河清|backup|_|sunswallow|lightsail|rak`。这是名称匹配，带下划线的节点也归入自建。
- “🌐 仅非自建节点”组承接 `Rule/NotSelf.list` 的四个站点，使用自建表达式的反向筛选，实际排除符合自建条件的节点。以后修改自建关键词时，必须同步 AI 节点与仅非自建节点的筛选。
- 全局自动选择、故障转移的筛选保持不变：名称不含电信、联通、移动的自建节点（包括 rak）原本就会参与；AI 节点和自建节点组仍为手动选择。
- “漏网之鱼”首选国内流量；国内流量组不包含国外流量选项。
- 保留 HBOGO/HBOMAX 分组及规则源、YouTube 排除印度的筛选、巴哈姆特仅筛选台湾。
- 保留个人规则的优先级：GitHub、个人直连、个人代理、非自建、ChatGPT/Claude/AI；国外媒体位于中国 GEOIP 之后、FINAL 之前。
- 保留 `ChatGPT.list` 的 poecdn.net、x.ai、grok.com 等个人补充和原有引用。
- 直连列表加入 sunswallow.org，将 p.once.im/g.once.im 替换为 sptv.ii00.cc/sgtv.ii00.cc。
- 直连列表包含 `DOMAIN-SUFFIX,tailf695a8.ts.net`，覆盖该域名及其所有子域名。
- 继续从个人直连列表排除 tagss 关键词、central-world.org、imgse.com、cdnlz22.com、metaglide.org、nxonearth.com、y-too.com、embyvip.org；imgse.com 在个人代理列表中。
- 保留 PT、科研、工具等个人代理规则；继续从个人代理列表排除 nodeseek.com 和裸域名 bangumi.moe。删除自定义规则不代表强制直连，仍由后续规则决定。

## 本次明确接受的上游更新

- 恢复并保留 SteamCN 专用直连规则，在通用游戏规则之前匹配。以后同步不要再次删除。
- 引入 ToDesk 直连域名及进程规则。
- 接受完整的上游 `AI.yaml` 和 `Claude.list`，包括 ASN 14061、Claude/Anthropic 关键词；不按之前的逐条筛选建议排除它们。
- 保留上游其余新增文件和清理结果；新增文件是否生效取决于配置是否引用，例如 Steam_Download、Google、QUIC 列表未额外启用。

此次同时把个人列表中 `DomainKeyword,m-team` 规范为 `DOMAIN-KEYWORD,m-team`，清理 sunswallow.org 前的空格和重复的 openreview.net 条目，保留原分流意图。

## 后续同步

1. 获取上游最新提交并保留当前版本回退点，通过合并保留双方历史。
2. 以上游为基础按以上清单恢复个人定制；同时处理新增、删除、替换和规则顺序，不直接用旧文件整体覆盖。
3. 原样更新上游默认配置及通用规则；个人变更应用在个人入口和列表中，保留旧 URL 兼容副本。
4. 检查策略组引用、规则文件路径、SteamCN 顺序、个人域名去向和兼容副本一致性；验证实际客户端与订阅转换器的兼容性。
5. 更新本文件中的基准提交和变更说明。

已知的优先级行为保持不变：gstatic.com 先匹配个人代理列表，Claude 中的 t0.gstatic.com 等更晚规则不会改变这一结果；非自建列表优先于 AI 列表。`IP-ASN` 等规则的运行支持仍取决于订阅转换器和客户端内核。

## 本次验证

已检查 36 个策略组、44 条规则集引用、19 个仅针对本地规则的代表性域名匹配用例，以及个人排除项和兼容副本一致性。36 个外部规则源均可读取，引用的 YAML 规则源可解析。未运行实际订阅转换器或 Clash/Mihomo 客户端；这些检查不等同于完整的运行时分流测试。
