# 天气警报卡片[中文]

一款自定义的Home Assistant Lovelace卡片，用于显示带有严重程度指示器、进度条和可展开详情的天气警报。支持NWS（美国）、BoM（澳大利亚）、MeteoAlarm（欧洲）、DWD（德国）、MeteoSwiss（瑞士）、ECCC（加拿大）、PirateWeather和CAP警报（多地区）。

[![天气警报卡片](https://raw.githubusercontent.com/seevee/weather_alerts_card/main/img/hero-adaptive.svg)](https://raw.githubusercontent.com/seevee/weather_alerts_card/main/img/hero-light.png)

## 功能特性

- **多提供商支持** — NWS（美国）、BoM（澳大利亚）、MeteoAlarm（欧洲）、DWD（德国）、MeteoSwiss（瑞士）、ECCC（加拿大）、PirateWeather和CAP警报（多地区），自动检测
- **颜色主题** — 基于严重程度（默认）、NWS官方事件颜色、MeteoAlarm意识级别颜色或ECCC公共警报颜色
- **时间进度条** — 已过/剩余时间，带有相对和绝对时间戳
- **警报标题** — 来自提供商数据的上下文副标题，可选冗余过滤
- **可展开详情** — 经过清理的描述、说明和来源链接
- **BoM阶段徽章** — 新建、更新、续期的生命周期指示器
- **紧凑布局** — 折叠的单行警报，点击后展开，带有进度条
- **区域过滤（BoM）** — 仅显示特定`area_id`区域的警报
- **可关闭警报** — 可选的单个警报关闭功能（按钮或滑动），带撤销和全部恢复控制，本地浏览器存储
- **排序顺序** — 默认、开始时间或严重程度
- **严重程度阈值** — 显示的最低严重程度（未分类警报始终显示）
- **本地化UI** — 英语、法语、西班牙语、意大利语和德语；从Home Assistant区域设置自动检测
- **可视化配置** — 无需YAML编辑

## 主题

[![严重程度、NWS和MeteoAlarm颜色主题](https://raw.githubusercontent.com/seevee/weather_alerts_card/main/img/themes-adaptive.svg)](https://raw.githubusercontent.com/seevee/weather_alerts_card/main/img/themes-light.png)

## 快速开始

1. 为您的地区安装天气警报集成（参见[支持的提供商](#支持的提供商)）
2. 通过HACS安装此卡片：搜索"Weather Alerts Card"
3. 添加到您的仪表板并选择您的警报实体

## 安装

### HACS（推荐）

[![打开您的Home Assistant实例并在Home Assistant社区商店中打开一个仓库。](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=seevee&repository=weather_alerts_card)

然后点击下载按钮，并在提示时点击重新加载。

### 手动安装

1. 从[最新版本](../../releases/latest)下载`weather-alerts-card.js`
2. 复制到`config/www/weather-alerts-card.js`
3. 作为资源添加：**设置 → 仪表板 → 资源** → URL: `/local/weather-alerts-card.js`，类型: JavaScript模块

## 配置

| 选项 | 默认值 | 描述 |
|--------|---------|-------------|
| `entity` | *(必需，除非设置了`device`)* | 警报传感器实体 |
| `entities` | — | 要合并的额外警报实体（例如DWD当前+预警） |
| `device` | — | HA `device_id` — 自动发现该设备下的所有警报传感器，并在警报出现和消失时重新发现。目前只有CAP警报集成使用此形状。可以与`entity`/`entities`结合使用或单独使用。 |
| `provider` | 自动检测 | `'nws'`, `'bom'`, `'meteoalarm'`, `'dwd'`, `'meteoswiss'`, `'eccc'`, `'pirateweather'`, `'cap'` |
| `title` | — | 卡片标题 |
| `zones` | — | BoM `area_id`代码过滤（例如`NSW_FL049`） |
| `sortOrder` | `'default'` | `'default'`, `'onset'`, `'severity'` |
| `minSeverity` | `'all'` | `'all'`, `'minor'`, `'moderate'`, `'severe'`, `'extreme'`。严重程度未知/未分类的警报始终显示，不受此阈值影响 |
| `colorTheme` | `'severity'` | `'severity'`, `'nws'`, `'meteoalarm'`, `'eccc'` — `'eccc'`使用ECCC发布的`red`/`orange`/`yellow`/`grey`调色板（与weather.gc.ca匹配）；在此主题下显示非ECCC警报时回退到规范严重程度级别 |
| `enhanceContrast` | `'subtle'` | `'off'`, `'subtle'`, `'strict'` — 针对在活动主题的卡片背景上阅读效果不佳的NWS/MeteoAlarm事件原始十六进制值，增强前景色。按事件、按主题模式应用，且仅在失败方向应用。`'subtle'`（默认）使用文本级别（图标/标签约2:1）和更严格的进度级别（进度条填充约1.3:1，可捕捉几乎不可见的色调，如黄色龙卷风观察）。`'strict'`收紧两个级别（文本约3:1，进度约2:1）以接近WCAG AA保证。`'off'`始终渲染原始主题十六进制值。已经清晰可读的事件（如龙卷风警告）在所有模式下保持不变。 |
| `eventCodes` | — | 要包含的事件代码，例如`['SVR', 'TOR']`（NWS）或`['31', '95']`（DWD） |
| `excludeEventCodes` | — | 要排除的事件代码，例如`['SCY']`（NWS）或`['22']`（DWD） |
| `timezone` | `'server'` | `'server'`或`'browser'`（客户端本地时间） |
| `deduplicateHeadlines` | `true` | 抑制重复事件名称的标题 |
| `deduplicate` | `true` | 跨区域和提供商折叠匹配的警报 |
| `animations` | 系统 | `true`, `false`，或遵循`prefers-reduced-motion` |
| `showDetails` | `true` | 显示可展开的详情面板（当`false`时隐藏整个"阅读详情"部分） |
| `expandDetails` | `false` | 始终内联显示详情，无需切换（适合壁挂式显示器） |
| `showProvider` | `false` | 在事件标题上方显示提供商标签（例如NWS） |
| `showMetadata` | `true` | 在详情面板中显示发布/开始/过期/区域网格 |
| `showDescription` | `true` | 在详情面板中显示描述文本 |
| `showInstructions` | `true` | 在详情面板中显示说明文本 |
| `showGeometry` | `false` | 在详情面板中显示受影响区域轮廓的内联SVG迷你地图。仅适用于CAP警报（`cap_alerts`）— 其他提供商没有几何数据。立即绘制边界框框架，并在后台获取后叠加多边形（缓存未命中时回退到框架）。 |
| `geometryStyle` | `'shape'` | 当`showGeometry`开启时的迷你地图渲染。`'shape'`：纯多边形轮廓，完全离线。`'map'`：多边形后面的栅格瓦片底图提供地理上下文 — **需选择加入，获取地图瓦片（在线），并向瓦片主机揭示警报的边界框**。瓦片失败时回退到轮廓。 |
| `geometryTileUrl` | CARTO | 当`geometryStyle: 'map'`时使用的Slippy地图瓦片模板（`{z}/{x}/{y}`，可选`{s}`）。默认是Home Assistant自身地图使用的主题感知CARTO底图（`light_all`/`dark_all`，已启用CORS）。覆盖以指向自托管或代理的瓦片源。 |
| `geometryTileAttribution` | `© OpenStreetMap, CARTO` | 显示在地图上的归属标签。使用自定义`geometryTileUrl`时，请设置此项以感谢您的提供商。 |
| `showSourceLink` | `true` | 显示"打开来源"链接（kiosk模式设为`false`） |
| `hideExpired` | `true` | 隐藏过期警报（设为`false`以灰显显示） |
| `hideNoAlerts` | `false` | 当没有警报时隐藏"无活动警报"横幅 |
| `unavailableBehavior` | `'message'` | 显示损坏警报传感器的方式。`'message'`：完整通知；`'compact'`：静音单行；`'hide'`：当*每个*配置的传感器都不可用时隐藏卡片（**不推荐 — 损坏的传感器不能证明安全**） |
| `fontSize` | `'default'` | `'small'`, `'default'`, `'large'`, `'x-large'` — 缩放文本和图标 |
| `reformatText` | `true` | 从警报文本中删除硬换行（NWS 69字符电传中断），同时保留段落中断 |
| `layout` | `'default'` | `'default'`或`'compact'` |
| `allowDismiss` | `false` | 允许用户关闭单个警报（浏览器本地）。添加×按钮和/或滑动手势 |
| `dismissTrigger` | `'button'` | `'button'`, `'swipe'`或`'both'` — 关闭警报的方式（滑动涵盖触摸+鼠标拖动）。需要`allowDismiss` |
| `dismissButtonStyle` | `'icon'` | `'icon'`或`'labeled'`（图标+"关闭"文本）。当`dismissTrigger: 'swipe'`时无效；紧凑布局始终仅为图标 |
| `showDismissUndo` | `true` | 关闭警报时显示撤销吐司。当`allowDismiss`关闭时无效 |

<details>
<summary><strong>示例</strong></summary>

**基本配置**
```yaml
type: custom:weather-alerts-card
entity: sensor.nws_alerts_alerts
```

**带标题和区域过滤的BoM**
```yaml
type: custom:weather-alerts-card
entity: sensor.sydney_warnings
provider: bom
title: 天气警报
zones:
  - NSW_FL049
```

**NWS官方颜色、紧凑布局、按严重程度排序**
```yaml
type: custom:weather-alerts-card
entity: sensor.nws_alerts_alerts
colorTheme: nws
layout: compact
sortOrder: severity
```

**NWS过滤到特定事件类型，浏览器时区**
```yaml
type: custom:weather-alerts-card
entity: sensor.nws_alerts_alerts
eventCodes:
  - TOR
  - SVR
timezone: browser
```

**欧洲MeteoAlarm警告，使用意识颜色**
```yaml
type: custom:weather-alerts-card
entity: binary_sensor.meteoalarm
colorTheme: meteoalarm
```

**澳大利亚BoM警告**
```yaml
type: custom:weather-alerts-card
entity: sensor.sydney_warnings
provider: bom
```

**DWD（德国）**
```yaml
type: custom:weather-alerts-card
entity: sensor.dwd_weather_warnings_hamburg_current
```

**DWD当前+预警警告合并**
```yaml
type: custom:weather-alerts-card
entity: sensor.dwd_weather_warnings_current
entities:
  - sensor.dwd_weather_warnings_advance
```

**MeteoSwiss（瑞士）**
```yaml
type: custom:weather-alerts-card
entity: sensor.weather_warnings_at_8000
```

**ECCC（加拿大）**
```yaml
type: custom:weather-alerts-card
entity: sensor.marathon_alerts
```

**ECCC使用官方公共警报调色板**
```yaml
type: custom:weather-alerts-card
entity: sensor.marathon_alerts
colorTheme: eccc
```

**PirateWeather警报**
```yaml
type: custom:weather-alerts-card
entity: sensor.pirateweather_alerts
```

**CAP警报 — 自动发现设备下的所有警报传感器**

[CAP警报集成](https://github.com/seevee/cap_alerts)在Home Assistant设备下为每个活动警报创建一个传感器。将卡片指向设备，它会自动获取每个活动警报传感器 — 并在警报发布或清除时重新获取。

```yaml
type: custom:weather-alerts-card
device: 1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d
```

从编辑器的CAP警报设备选择器中选择设备，避免手动输入ID。`device`也可以与`entities:`共存用于混合设置。

**匹配Bubble Card样式（需要[card-mod](https://github.com/thomasloven/lovelace-card-mod)）**

<details>
<summary>显示代码片段</summary>

将此卡片样式化为视觉上匹配[Bubble Card](https://github.com/Clooos/Bubble-Card)的大布局 — 28px圆角，56px行高，42×42图标芯片。注意：下面的选择器深入到此卡片的内部CSS类名，这些不是稳定的公共API，可能在版本之间更改。由[#144](https://github.com/seevee/weather_alerts_card/issues/144)贡献。

```yaml
type: custom:weather-alerts-card
entity: sensor.nws_alerts_alerts
sortOrder: severity
layout: compact
provider: nws
card_mod:
  style: |
    ha-card {
      background: transparent !important;
      border: none !important;
      border-radius: 28px !important;
      box-shadow: none !important;
    }
    .alert-card {
      background: rgb(40, 40, 40) !important;
      border: none !important;
      border-radius: 28px !important;
      box-shadow: none !important;
      overflow: hidden !important;
      min-height: 56px !important;
      margin: 0 0 8px !important;
    }
    .alert-card:last-child {
      margin-bottom: 0 !important;
    }
    .alert-header-row {
      min-height: 0 !important;
      height: 56px !important;
      padding: 0 12px 0 8px !important;
    }
    .icon-box {
      width: 42px !important;
      height: 42px !important;
      flex: 0 0 42px !important;
      --mdc-icon-size: 24px !important;
    }
    .icon-box ha-icon {
      --mdc-icon-size: 24px !important;
      width: 24px !important;
      height: 24px !important;
    }
    .compact-time {
      font-size: 13px !important;
    }
```

</details>

</details>

## 支持的提供商

卡片从实体属性自动检测提供商。任何产生兼容数据形状的集成都可以工作。

| 提供商 | 地区 | 测试过的集成 |
|----------|--------|---------------------|
| NWS | 美国 | [finity69x2/nws_alerts](https://github.com/finity69x2/nws_alerts) |
| BoM | 澳大利亚 | [bremor/bureau_of_meteorology](https://github.com/bremor/bureau_of_meteorology), [safepay/ha_bom_australia](https://github.com/safepay/ha_bom_australia) |
| MeteoAlarm | 欧洲 | 内置[meteoalarm](https://www.home-assistant.io/integrations/meteoalarm/) |
| DWD | 德国 | 内置[dwd_weather_warnings](https://www.home-assistant.io/integrations/dwd_weather_warnings/) |
| MeteoSwiss | 瑞士 | [izacus/hass-swissweather](https://github.com/izacus/hass-swissweather) — 将卡片指向`sensor.weather_warnings_at_<postcode>` |
| ECCC | 加拿大 | [seevee/cap_alerts](https://github.com/seevee/cap_alerts) (`provider: eccc`) — 推荐的ECCC来源；见下方说明 |
| PirateWeather | 全球 | [Pirate-Weather/pirate-weather-ha](https://github.com/Pirate-Weather/pirate-weather-ha) |
| CAP警报 | 多地区（NWS、ECCC、MeteoAlarm、WMO） | [seevee/cap_alerts](https://github.com/seevee/cap_alerts) — 每个活动警报一个传感器；与`device:`配对用于自动发现。摄取任何CAP 1.2源，包括WMO恶劣天气信息中心的多国源 |

> **关于ECCC的说明。** 对于加拿大环境与气候变化警报，请使用[CAP警报](https://github.com/seevee/cap_alerts)（`provider: eccc`）。这是此卡片推荐的唯一ECCC来源 — 既不路由到捆绑的HA核心`environment_canada`集成，也不路由到HACS `environment_canada`分支。见下方[加拿大：通过CAP警报的ECCC](#加拿大通过cap警报的eccc)了解原因。

### 加拿大：通过CAP警报的ECCC

对于加拿大环境与气候变化（ECCC）警报，[CAP警报](https://github.com/seevee/cap_alerts)
（`provider: eccc`）是要使用的来源。它摄取NAAD CAP源，
在Home Assistant设备下为每个活动警报创建一个传感器。这保留了
**原始** CAP严重程度和确定性，保留了原始多区域警报
多边形，并解锁`showGeometry`受影响区域迷你地图。

将卡片指向设备，它会自动发现每个活动警报，
并在警报出现和消失时重新发现：

```yaml
type: custom:weather-alerts-card
device: 1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d
colorTheme: eccc      # ECCC官方红/橙/黄/灰调色板
showGeometry: true    # 受影响区域迷你地图
```

从编辑器的CAP警报设备选择器中选择设备，而不是
手动输入ID。提供商自动检测为`eccc`；仅当您禁用了自动检测时才显式设置`provider: eccc`。

> **为什么不使用其他ECCC集成？** 存在另外两个ECCC集成，
> 但都不是此卡片的正确来源：
> - **HA核心`environment_canada`** 仅通过`get_alerts` **操作**公开丰富的警报详情（[#172393](https://github.com/home-assistant/core/pull/172393)，
>   合并代替了基于属性的[#164481](https://github.com/home-assistant/core/pull/164481)）。
>   Lovelace卡片在渲染期间读取状态和属性，无法消费
>   操作响应，因此核心无法驱动此卡片。
> - HACS **`environment_canada`** 分支确实显示了警报属性，但
>   这是一个其维护者宁愿不继续运行的临时解决方案（见
>   [讨论#3130](https://github.com/orgs/home-assistant/discussions/3130)），
>   并且它覆盖了核心集成的域。出于对此的尊重，我们
>   不将ECCC用户路由到它；CAP警报是卡片
>   作者为丰富警报数据维护的基于实体的路径。

> **如果您还运行`environment_canada`获取天气，请注意。** 它的警报列表
> （GeoMet WFS，通过针对您坐标的点在多边形测试过滤）
> 不会与CAP警报（NAAD CAP多边形，直接摄取）对齐。WFS
> 源滞后并截断NAAD覆盖范围，因此警报可能在一个中显示而不在
> 另一个中显示 — 这是上游行为，不是卡片的问题。

## 数据保真度

严重程度和确定性徽章始终本地化为您配置的语言。当值由卡片的适配器逻辑推断（而非警报源直接提供）时，它以斜体文本和波浪号前缀（`~Moderate`）渲染，以便您一眼就能分辨哪些徽章反映实际提供商数据。

| 提供商 | 严重程度 | 确定性 |
|----------|----------|-----------|
| NWS | 原始（来自`Severity`字段） | 原始（来自`Certainty`字段） |
| BoM | 推断（从标题/类型/组解析） | 无 |
| MeteoAlarm | 原始（来自`awareness_level`或`severity`） | 原始（来自`certainty`） |
| DWD | 原始（来自整数`level`） | 无 |
| MeteoSwiss | 原始（来自整数级别） | 无 |
| ECCC | 派生（`color`、`type`、`impact`的最大值；仅当三者都缺失时加波浪号） | 从`confidence`映射（高→可能，中→可能，低→不太可能） |
| PirateWeather | 原始（来自`severity`字段） | 无 |
| CAP警报 | 原始（来自`severity_normalized` / `severity`） | 原始（来自`certainty`字段） |

## 开发

```bash
npm install
npm run build     # 打包 → dist/weather-alerts-card.js
npm run watch     # 带文件监视的打包
npm run lint      # TypeScript类型检查
```

## 迁移到v3

v3.0.0移除了在v2中已弃用的向后兼容垫片。如果您从v1.x或v2.x升级，请进行以下更改：

**1. 卡片类型重命名**（仅v1.x用户）

更改仪表板YAML中的卡片类型：

```yaml
# 之前
type: custom:nws-alerts-card

# 之后
type: custom:weather-alerts-card
```

**2. `headline`配置键移除**（仅v1.x用户）

`headline`键在v2中被`deduplicateHeadlines`替换。如果您的卡片配置仍有`headline:`，请重命名：

```yaml
# 之前
headline: true   # 或 false

# 之后
deduplicateHeadlines: true   # 或 false
```

**3. 仅手动安装：资源文件名更改**

如果您手动安装（非通过HACS），请在设置 → 仪表板 → 资源中更新资源路径：

| 之前 | 之后 |
|--------|-------|
| `/local/nws-alerts-card.js` | `/local/weather-alerts-card.js` |

HACS用户：无需操作 — HACS自动管理资源路径。

## 支持

如果您觉得此卡片有用，请在[Ko-fi](https://ko-fi.com/seeveezee)给我小费以支持开发，或捐赠给[The Y'all Squad](https://www.theyallsquad.org/donate) — 一个快速响应计划，为受恶劣天气事件影响的家庭提供直接援助、链锯和物资。

---

**资源：** [Home Assistant社区帖子](https://community.home-assistant.io/t/weather-alerts-card)
