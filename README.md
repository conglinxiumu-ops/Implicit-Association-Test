# 🧭 THE QUIET VERDICT · 内隐联想测验

> 一个用于实验心理学课堂演示的经典内隐社会认知实验，单文件部署，开箱即用。

---

## 📖 关于本实验

**内隐联想测验（Implicit Association Test, IAT）** 由 Greenwald、McGhee 与 Schwartz 于 1998 年提出，是测量内隐社会态度的经典研究范式。通过比较被试在"相容任务"（概念与属性存在内隐关联）和"不相容任务"（概念与属性内隐冲突）中的反应时差异，IAT 能间接揭示个体意识层面之外的态度、偏好与刻板印象。

自提出以来，IAT 已被广泛应用于种族态度、性别刻板印象、年龄偏见、政治态度、自尊、消费者偏好等数十个研究领域，是 **社会认知心理学史上最具影响力的实验范式之一**。

本程序在经典 1998 范式基础上，进一步采用 **Greenwald, Nosek, & Banaji (2003) 改进算法（D2）** 与 **被试间反平衡（counterbalancing）** 设计，以消除 block 顺序效应、提升数据稳健性。视觉上以 **"极简学术 · Minimal Academic"** 设计语言构建——白底单色调、衬线刺激词、目标黑/属性蓝的双色编码——还原科研级实验程序的克制与专注感。

---

## 🎮 实验设计

### 流程

```
欢迎页（输入被试编号 · 随机分配反平衡顺序）
    ↓
任务说明页
    ↓
Block 1 过渡 → Block 1 · 目标分类（20 次）
    ↓
Block 2 过渡 → Block 2 · 属性分类（20 次）
    ↓
Block 3 过渡 → Block 3 · 联合任务练习（20 次）
    ↓
Block 4 · 联合任务正式（40 次）
    ↓
Block 5 过渡 → Block 5 · 反转目标分类（40 次）
    ↓
Block 6 过渡 → Block 6 · 联合任务反转练习（20 次）
    ↓
Block 7 · 联合任务反转正式（40 次）
    ↓
结果汇总页（D 值与数据导出）
```

### 7-Block 标准设计（以 `compatible_first` 顺序为例）

| Block | 试次 | 左键 (E) | 右键 (I) | 阶段类型 |
|:-----:|:----:|:--------:|:--------:|:--------:|
| **1** | 20 | 花 🌸 | 虫 🪲 | 目标分类练习 |
| **2** | 20 | 好词 ☀️ | 坏词 🌑 | 属性分类练习 |
| **3** | 20 | 花 / 好词 | 虫 / 坏词 | 相容任务（练习） |
| **4** | 40 | 花 / 好词 | 虫 / 坏词 | **相容任务（正式）** |
| **5** | 40 | 虫 | 花 | 反转目标（练习） |
| **6** | 20 | 虫 / 好词 | 花 / 坏词 | 不相容任务（练习） |
| **7** | 40 | 虫 / 好词 | 花 / 坏词 | **不相容任务（正式）** |

> **关键设计**：Block 3/4 与 Block 6/7 的差异在于目标与属性的配对方式。若被试对"花"持有内隐积极态度，则在相容任务中的反应时应显著快于不相容任务，D 值正基于此差异计算。

> **反平衡（Counterbalancing）**：每位被试在欢迎页随机分配 `compatible_first`（按上表顺序）或 `incompatible_first`（B1–B4 与 B5–B7 整体翻转）。CSV 中记录该信息，便于在群体分析中消除顺序效应。这是相对于经典 1998 流程的重要改进。

> **B5 设为 40 试次**：依 Greenwald, Nosek, & Banaji (2003) 的建议，反转目标练习需要更多试次以充分"解除学习"，避免前向干扰污染 B6/B7。

### 实验材料（默认主题）

| 类别 | 刺激词（各 8 个） |
|:----|:----|
| **花** (target1) | 玫瑰、百合、郁金香、雏菊、兰花、水仙、牡丹、莲花 |
| **虫** (target2) | 蜘蛛、蟑螂、蚊子、马蜂、苍蝇、毛毛虫、蜈蚣、臭虫 |
| **好词** (attribute1) | 快乐、和平、欢笑、友谊、礼物、温暖、阳光、甜蜜 |
| **坏词** (attribute2) | 痛苦、仇恨、悲伤、灾难、丑陋、邪恶、可怕、肮脏 |

> 选用"花 vs 虫 + 好 vs 坏"作为默认配置，对应 Greenwald (1998) Study 1 的标准范式：主题中性、伦理无争议、IAT 效应稳健，最适合教学演示与方法验证。

### 试次生成逻辑

- **单一分类 Block**（B1, B2, B5）：左右两侧各占 50% 试次，全部试次经 Fisher-Yates 算法全局打乱。
- **联合任务 Block**（B3, B4, B6, B7）：目标试次与属性试次分别独立打乱后**严格交替排列**（T-A-T-A-...），既保证两侧分布均衡，又避免连续同类型试次造成预期反应。
- **错误处理**：采用 "built-in penalty" 模式——按错时显示红色 ✕，必须按正确键才能进入下一试次。修正后的反应时（rt_correct）作为 D 计算的输入。

### D 值计算与解释

本程序采用 **Greenwald, Nosek, & Banaji (2003) 改进计分算法 D2**：

1. 仅保留 B3/B4/B6/B7 的正确试次（rt ≤ 10 000 ms）
2. 计算 D₁ = (M_B6 − M_B3) / SD_pooled(B3+B6)（练习阶段）
3. 计算 D₂ = (M_B7 − M_B4) / SD_pooled(B4+B7)（正式阶段）
4. 综合 **D = (D₁ + D₂) / 2**

**D 值符号约定**：D > 0 ⇒ 对 target1（默认：花）的内隐偏好更强；D < 0 ⇒ 对 target2（默认：虫）的内隐偏好更强。**反平衡顺序在算法中已校正**，D 值的方向意义不受被试分配顺序影响。

| \|D\| 范围 | 强度 | 说明 |
|:--------|:----|:----|
| < 0.15 | 几乎无偏好 | 相容与不相容任务无显著反应时差异 |
| 0.15 – 0.35 | 轻度偏好 | 存在可探测的内隐态度倾向 |
| 0.35 – 0.65 | 中等偏好 | 中等强度的内隐态度联结 |
| ≥ 0.65 | 强烈偏好 | 强烈的内隐态度联结 |

---

## ✨ 功能特性

- **零依赖**：单个 `index.html` 文件，无外部资源、无网络请求，双击即可运行
- **被试基本信息**：欢迎页可填写编号、性别、年龄、利手、教育程度，**所有字段均可选**，留空跳过；ID 留空时自动生成 `ANON_<时间戳>` 用于文件命名
- **被试间反平衡**：每位被试随机分配 `compatible_first` / `incompatible_first` 顺序，自动校正 D 值方向
- **标准 7-Block 设计**：严格遵循 Greenwald (1998) + Greenwald (2003) 改进流程，共 200 试次
- **Built-in Penalty 错误纠正**：错按显红 ✕ 反馈，必须按正确键才能继续，反应时自动包含修正过程
- **双色刺激编码**：目标词黑色、属性词蓝色，混合 block 中作为类别提示降低工作记忆负担
- **🔊 音效合成**：使用 Web Audio API 实时合成——错误音（锯齿波下行）、block 完成音（向上和声），无需音频文件，按 `M` 键可静音
- **实时 HUD**：顶部信息栏显示被试编号、当前试次/总试次、当前 Block、累计错误数
- **Block 间过渡页**：每个 block 前清晰展示按键映射，按 `SPACE` 进入正式试次
- **结果可视化**：D 值卡片 + 相容/不相容反应时对比 + 各 block 平均 RT 折线图（含相容/不相容色带标注）
- **D 值自动计算**：基于 Greenwald 2003 改进算法，自动剔除超时试次、计算合并 SD
- **CSV 导出**：一键导出完整数据（UTF-8 含 BOM），含 5 个区段：主试次表、Block 摘要、D 值、元信息、原始按键日志
- **键盘 + 触摸双模**：桌面端 `E` / `I` 键操作；移动端自动启用左右半屏触摸区
- **响应式布局**：桌面 / 平板 / 手机均可正常使用，960px 与 640px 双断点适配

---

## 📊 数据格式

点击结果页的 **"下载 CSV"** 按钮，将下载一个 UTF-8（含 BOM）的 CSV 文件，文件名格式为：

```
IAT_<participant_id>_<datetime>.csv
```

CSV 包含 **5 个区段**：

### 1️⃣ 主试次表（每行对应一个试次，22 字段）

| 字段 | 说明 |
|:-----|:-----|
| `participant_id` | 被试编号 |
| `trial_index` | 全局试次编号（1–200） |
| `block_index` | 所属 Block（1–7） |
| `block_name` | Block 内部名称（target_practice / combined_critical 等） |
| `block_compatibility` | `compatible` / `incompatible` / `—` |
| `block_order` | 该被试的反平衡顺序 |
| `trial_in_block` | Block 内试次编号 |
| `stimulus` | 呈现的刺激词 |
| `stimulus_type` | `target` / `attribute` |
| `stimulus_category` | 具体类别（target1 / target2 / attr1 / attr2） |
| `correct_side` | 正确反应（left / right） |
| `left_targets`, `left_attributes` | 当前 block 左键所属类别 |
| `right_targets`, `right_attributes` | 当前 block 右键所属类别 |
| `first_response_side` | 被试首次按键 |
| `first_response_correct` | 是否首次正确（0 / 1） |
| `first_response_rt_ms` | 首次反应时（毫秒） |
| `n_errors_before_correct` | 修正前的错误次数 |
| `rt_correct_ms` | 修正后正确反应时（D2 算法用此字段） |
| `trial_total_duration_ms` | 试次总时长（含修正过程） |
| `trial_start_unix_ms` | 试次起始的 UNIX 毫秒时间戳 |

### 2️⃣ Block 摘要

每个 block 的均值 RT、准确率、错误数、试次数。

### 3️⃣ D 值结果

`D_practice`（B3 vs B6）、`D_critical`（B4 vs B7）、`D_overall`（综合 D 值）。

### 4️⃣ 元信息

被试 ID、反平衡顺序、起止时间戳、用户代理字符串、屏幕尺寸、所有刺激词内容。

### 5️⃣ 原始按键日志

每一次按键事件的精细记录（含错误按键）：试次编号、按键值、左/右、是否正确、相对试次起点的反应时、UNIX 时间戳。可用于反应时分布建模、连续错误分析、决策动力学研究等。

---

## 📁 文件结构

```
.
├── README.md
└── index.html    # 全部代码：HTML + CSS + JavaScript，约 1,940 行，含详细中文注释
```

---

## 🚀 快速开始

### 方式 1：本地直接打开

下载 `index.html`，双击即可在浏览器中运行。

### 方式 2：本地服务器（推荐）

```bash
# 任意一种方式
python3 -m http.server 8000
# 或
npx serve
```

然后访问 `http://localhost:8000`。

### 方式 3：免费在线部署

直接上传到 [Vercel](https://vercel.com) / [Netlify](https://netlify.com) / GitHub Pages 即可。

---

## 🛠️ 自定义与二次开发

如果您具有基础的 HTML 与 JavaScript 知识，可轻松修改实验材料与参数。**所有可配置项已集中在 `<script>` 顶部的 `CONFIG` 对象中**，无需深入业务逻辑代码。用任意文本编辑器打开 `index.html`：

### 修改实验材料（最常见）

定位到 `CONFIG.TOPIC` 对象：

```javascript
CONFIG.TOPIC = {
  target1:    { name: '花',   items: ['玫瑰','百合','郁金香','雏菊','兰花','水仙','牡丹','莲花'] },
  target2:    { name: '虫',   items: ['蜘蛛','蟑螂','蚊子','马蜂','苍蝇','毛毛虫','蜈蚣','臭虫'] },
  attribute1: { name: '好词', items: ['快乐','和平','欢笑','友谊','礼物','温暖','阳光','甜蜜'] },
  attribute2: { name: '坏词', items: ['痛苦','仇恨','悲伤','灾难','丑陋','邪恶','可怕','肮脏'] },
};
```

替换任意数组中的词语即可更改实验主题。例如，将"花-虫" IAT 改为"年轻-年老" IAT：

```javascript
CONFIG.TOPIC = {
  target1:    { name: '年轻', items: ['青春','学生','朝阳','活力','新潮','儿童','少年','蓬勃'] },
  target2:    { name: '年老', items: ['晚年','老年','夕阳','皱纹','拐杖','退休','白发','沧桑'] },
  attribute1: { name: '好词', items: ['快乐','和平','欢笑','友谊','礼物','温暖','阳光','甜蜜'] },
  attribute2: { name: '坏词', items: ['痛苦','仇恨','悲伤','灾难','丑陋','邪恶','可怕','肮脏'] },
};
```

> 解读时注意：D > 0 始终代表 **对 target1 的内隐偏好**。若你关心的是 target2（如"对老年人的内隐偏好"），看 D < 0 方向。

### 修改试次数量

定位到 `CONFIG.BLOCKS` 数组，修改每个 Block 的 `trials` 字段：

```javascript
CONFIG.BLOCKS = [
  { name: 'target_practice',            trials: 20 },
  { name: 'attribute_practice',         trials: 20 },
  { name: 'combined_practice',          trials: 20 },
  { name: 'combined_critical',          trials: 40 },  // 关键 block 1
  { name: 'reversed_target_practice',   trials: 40 },
  { name: 'reversed_combined_practice', trials: 20 },
  { name: 'reversed_combined_critical', trials: 40 },  // 关键 block 2
];
```

> ⚠️ D 值依赖 B3/B4/B6/B7，建议关键 block（B4/B7）至少保持 40 试次。

### 修改时间参数

```javascript
CONFIG.ITI_MS    = 300;    // 试次间隔
CONFIG.MAX_RT_MS = 10000;  // 反应时上限（D 计算时剔除超过此值的试次）
CONFIG.MIN_RT_MS = 200;    // 过快反应下限
```

### 修改主题颜色

定位到 `<style>` 块开头的 `:root` CSS 变量区域：

```css
:root {
  --bg:              #FAFAFA;   /* 整体背景 */
  --surface:         #FFFFFF;   /* 卡片/HUD 背景 */
  --text:            #1F2937;   /* 主文字 */
  --accent:          #2563EB;   /* 强调色（按键提示、关键 UI） */
  --target-color:    #1F2937;   /* 目标词颜色（黑） */
  --attribute-color: #2563EB;   /* 属性词颜色（蓝） */
  --error:           #DC2626;   /* 错误标记红 */
  --success:         #16A34A;   /* 正确反馈绿 */
}
```

### 修改按键映射

如果您希望使用其他按键（如 F 和 J），可在 `handleKey()` 函数中将 `'e'` 与 `'i'` 替换为对应按键，并同步修改过渡页与按键提示栏的显示文字。

### 关闭反平衡

若需固定 block 顺序（如调试或特殊设计），可在 `state.blockOrder = ...` 处直接赋值为 `'compatible_first'` 或 `'incompatible_first'`，跳过随机分配。**正常实验请勿关闭反平衡**。

---

## 🖥️ 浏览器兼容性

支持所有现代浏览器（Chrome / Firefox / Safari / Edge 最近 2 年版本）。不支持 IE。

推荐分辨率：≥ 1280×720。在 960px 与 640px 处自动切换响应式布局，移动端启用触摸操作。

---

## 📚 参考文献

Greenwald, A. G., McGhee, D. E., & Schwartz, J. L. K. (1998). Measuring individual differences in implicit cognition: The Implicit Association Test. *Journal of Personality and Social Psychology, 74*(6), 1464–1480. <https://doi.org/10.1037/0022-3514.74.6.1464>

Greenwald, A. G., Nosek, B. A., & Banaji, M. R. (2003). Understanding and using the Implicit Association Test: I. An improved scoring algorithm. *Journal of Personality and Social Psychology, 85*(2), 197–216. <https://doi.org/10.1037/0022-3514.85.2.197>

Nosek, B. A., Greenwald, A. G., & Banaji, M. R. (2007). The Implicit Association Test at age 7: A methodological and conceptual review. In J. A. Bargh (Ed.), *Social psychology and the unconscious: The automaticity of higher mental processes* (pp. 265–292). Psychology Press.

---

## 🤝 Skill与其他实验项目

本项目的设计与文档结构运用到  [psychology-experiment-builder](https://github.com/conglinxiumu-ops/psychology-experiment-builder) Skill 。其他心理学实验项目陆续更新中，可在 [conglinxiumu-ops](https://github.com/conglinxiumu-ops/) Github仓库中查询下载。

---

## 📄 许可证

本项目采用 **MIT License** 开源，您可以自由地使用、修改和分发，无论是用于教学演示还是学术研究。

---

<div align="center">

**🧭 THE QUIET VERDICT**

*Made for the curious minds of Experimental Psychology.*

</div>
