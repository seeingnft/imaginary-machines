# 想象机器 · 数字艺术先驱档案

一份关于计算机艺术与数字创作史的中文在线档案，收录从 1920 年代至今约 300 位艺术家。

---

## 快速开始

### 环境要求
- Node.js 16+（无需安装任何 npm 包，纯内置模块）
- Anthropic API Key（用于中文翻译）

### 文件结构

```
imaginary-machines/
├── run.js          ← 主脚本（抓取 + 翻译 + 生成网站）
├── template.html   ← 网站模板（CSS + JS 壳层）
├── data.json       ← 数据文件（运行后自动生成，增量保存）
└── index.html      ← 生成的网站（运行后自动生成）
```

### 第一步：设置 API Key

```bash
export ANTHROPIC_API_KEY=sk-ant-xxxxx
```

Windows PowerShell：
```powershell
$env:ANTHROPIC_API_KEY = "sk-ant-xxxxx"
```

### 第二步：运行

**完整流程（推荐）：**
```bash
node run.js
```

约 300 位艺术家，全程约 60–90 分钟（大部分时间在翻译，每位约 1–2 秒）。

**测试用（先跑 20 位看看效果）：**
```bash
node run.js --sample 20
```

**只抓取不翻译（快速预览英文版）：**
```bash
node run.js --no-translate
```

**已有 data.json，只重新生成网站：**
```bash
node run.js --build-only
```

---

## 中途可以安全中断

按 `Ctrl+C` 随时中断，已处理的数据自动保存到 `data.json`。

下次运行时自动从断点续跑，**已翻译的艺术家不重复消耗 API**。

---

## 网站功能

打开 `index.html` 即可浏览：

| 功能 | 说明 |
|------|------|
| 卡片视图 | 按姓氏首字母分组，支持搜索 |
| 时间轴视图 | 按出生年排列，可视化生命跨度 |
| 时代筛选 | 先驱期 / 数字化时代 / AI 时代 |
| 国籍筛选 | 自动提取所有国籍，多选叠加 |
| 详情弹窗 | 点击卡片查看完整传记 |
| 小红书导出 | 一键调用 Claude API 生成「未来已有人住」系列稿件 |

---

## 数据说明

`data.json` 中每位艺术家的字段：

```json
{
  "artist-slug": {
    "name": "Vera Molnár",
    "nationality": "Hungarian",
    "birthYear": "1924",
    "deathYear": "2023",
    "intro": "英文简介...",
    "fullBio": "英文完整传记...",
    "nationality_cn": "匈牙利",
    "intro_cn": "中文简介...",
    "fullBio_cn": "中文完整传记..."
  }
}
```

---

## 常见问题

**Q: 翻译失败了几位怎么办？**  
A: 直接再跑一次 `node run.js`，只会重试 `intro_cn` 为空的艺术家。

**Q: 想更新某位艺术家的翻译？**  
A: 打开 `data.json`，把该艺术家的 `intro_cn` 和 `fullBio_cn` 改为空字符串 `""`，再运行 `node run.js --build-only --fetch-only` 不对，直接运行 `node run.js` 即可重新翻译。

**Q: 网络超时怎么办？**  
A: 正常现象，脚本会跳过并保留空数据，`Ctrl+C` 后重新运行会自动补抓。

**Q: 如何修改网站样式？**  
A: 编辑 `template.html`，然后运行 `node run.js --build-only`。
