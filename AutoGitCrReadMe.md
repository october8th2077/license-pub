# 📋 Git CR Plugin - 优雅的代码评审报告

<div align="center">

![Version](https://img.shields.io/badge/version-1.0.2-blue)
![Status](https://img.shields.io/badge/status-active-success)
![IntelliJ](https://img.shields.io/badge/IntelliJ-2022.3+-orange)

**自动化、智能化、可视化的代码评审插件**

</div>

---

### 🚀 核心功能

- ✅ **自动触发** - Git提交后自动执行代码评审
- ✅ **多模型支持** - 支持自定义模型 Gemini、OpenAI、Claude、Qwen 等各类主流模型
- ✅ **智能重试** - 支持配置多套模型，失败时后台自动切换尝试下一套配置，优化Token不足或网络原因导致的CR失败问题, 保障稳定性
- ✅ **纯 Kotlin 实现** - IDEA原生推荐开发语言，开箱即用
- ✅ **Git信息获取** - 自动提取仓库、分支、作者等信息
- ✅ **智能分析** - 提取重点问题和改进建议
- ✅ **IDEA原生通知** - 评审完成自动发送通知查看评审摘要
- ✅ **MD文档持久化存档** - 自动生成MD文档留档
- ✅ **多语言支持** - 支持切换中英文

### 🧠 智能 Diff 处理机制

#### 智能分片处理 (Smart Splitting)
插件不会简单粗暴地截断 Diff，而是采用 **File-Level Splitting** 策略：

1. **单文件熔断保护**：
    - 对单个文件的变动实施 **3000字符** (默认) 限制。
    - **场景**：删除了一个 2万行的 `doc.md`？插件会保留文件头和前3000字符，后续截断。
    - **效果**：大文件变动仅占用约 4% 的 Token，确保核心业务代码 (如 `*.kt`, `*.java`) 能够完整保留。

2. **全局智能豁免**：
    - 如果总 Diff 大小未超过全局限制 (默认 50000 字符)，则**不进行任何截断**，保留完整上下文。

3. **Diff 上下文模式**：
    - **Context Lines**: 仅包含变动行周围的 N 行 (默认 3 行)，节省 Token。
    - **Full Method**: 使用 `git diff -W`，包含完整的函数/方法上下文，让 AI 更精准理解修改意图 (消耗更多 Token)。

## ✨ 特性亮点

### 🎨 优雅美观的报告设计

采用现代化的Markdown设计，生成专业级的代码评审报告：

- 📊 **表格化元数据** - 清晰展示项目信息
- 🎯 **重点问题前置** - 快速了解核心问题 (支持 🛑/⚠️/🔧/✅ 严重程度分级)
- 💡 **智能建议提取** - 自动整理改进方案
- 🌈 **色彩分级显示** - 分数用不同颜色标识
- 📦 **可折叠Diff** - 节省空间,按需展开

### 🎮 游戏化评审体验 (情绪价值拉满)

| 分数区间 | 颜色 | 评语 |
|---------|------|------|
| 100 | 👑 金色 | 💯 最强王者 |
| 90-99 | 🟢 绿色 | ✨ 至尊星耀 |
| 80-89 | 🔵 蓝色 | 💎 永恒钻石 |
| 70-79 | 🟡 黄色 | 🔥 荣耀黄金 |
| 60-69 | 🟠 橙色 | 💡 秩序白银 |
| 0-59 | 🔴 红色 | 🚨 塑料代码 |
代码提交不再是枯燥的任务！插件引入了**段位评级系统**，让每一次 Commit 都像是在打排位赛：

- 🏆 **段位机制** - 引入 MOBA 风格段位（从`倔强青铜`到`最强王者`），让写代码像打游戏一样上瘾。
- 🎭 **趣味反馈** - 拒绝机械回复！高分代码给予 `💯 最强王者` 的至高荣耀，低分代码则有 `🚨 塑料代码` 的幽默鞭策。
- 🔋 **情绪价值** - 看着评分从红色变为绿色，获得满满的即时正向反馈，不仅是评审，更是激励。

---

## 📖 报告结构

### 完整的五大模块

```
1. 📦 项目信息表格
   - 仓库名称、分支、提交人、评审模型、耗时等

2. 🔍 重点问题简述  
   - 自动提取核心问题，按严重程度排序
   - 🛑 严重问题
   - ⚠️ 警告问题
   - 🔧 改进建议

3. 🔬 详细分析
   - AI生成的完整分析内容
   - 包含具体的代码位置和修复建议

4. 🛠 改进建议
   - 通用的代码质量提升建议

5. 📦 Diff详情
   - 可折叠的代码变更展示 (含截断警告)
```
---

## ⚙️ 配置说明

### 配置面板

**Settings → Tools → Auto Git CR**

![Settings](https://github.com/october8th2077/license-pub/blob/main/pic/auto-git-cr-cfg-demo.png?raw=true)

### 1. 配置管理 (Profile)
- **多配置支持**：可以保存多套配置 (如 "Gemini Pro", "GPT-4", "Company Internal")。
- **自动排序**：最近使用的配置会自动置顶。
- **Auto Retry**：勾选 "Auto Retry Next Configuration on Failure" 后，如果当前配置调用失败，插件会自动尝试列表中的下一个配置。

### 2. 模型设置
- **Conf Name**: 配置别名
- **LLM Model**: 模型名称 (如 `gemini-1.5-pro-latest`, `gpt-4o`)
- **API Key**: 对应的 API Key
- **Base URL**:
    - **Gemini**: 自动识别，无需填写 (除非使用代理)
    - **Dify**: 填写 `http://.../v1` 或 `http://.../chat-messages`，插件会自动适配 Dify 协议。
    - **OpenAI/Compatible**: 填写完整 API Endpoint (如 `https://api.openai.com/v1/chat/completions`)

### 3. 高级设置 (Advanced Settings)

| 设置项 | 说明 | 默认值 |
|-------|------|-------|
| **Git Diff Context Type** | **Lines**: 仅看前后几行 (省Token)<br>**Full Method**: 查看完整函数 (-W) (更精准) | Lines (3行) |
| **Simple Class Diff Limit** | 单个文件的最大字符数，防止大文件刷屏 | 3000 |
| **Total Diff/Prompt Limit** | 发送给 AI 的最大总字符数 | 50000 |
| **Ignore Projects** | 忽略的项目列表 (逗号分隔)，支持多行输入 | (空) |

---

## 📊 报告示例简要预览

---

---

# 📋 AI代码评审报告

| **代码仓库** | `auto-git-cr` / `main`                                                                  |
|:---|:----------------------------------------------------------------------------------------|
| **提交作者** | wk                                                                            |
| **提交说明** | 代码优化                                                                                    |
| **评审模型** | sehai                                                                                   |
| **评审时间** | 2026-01-09 11:54:22 (耗时17秒)                                                             | 
| **代码评分** | <span style="color: #17a2b8; font-size: 1.2em;">**85/100**</span> &nbsp;&nbsp; 💍️ 尊贵铂金 |

---

## 🔍 重点问题简述

> 💡 快速了解本次代码评审发现的核心问题

- ⚠️ **错误处理不规范**：直接返回字符串错误信息未遵循统一异常处理机制
- 🔧 **顶层常量缺少线程安全注释**：JSON_MEDIA_TYPE未声明为不可变常量
- 🔧 **冗余空指针检查**：candidates.size() > 0与get(0)存在冗余判断
- ✅ **空指针防护增强**：新增candidates非空校验提升稳定性
- ✅ **常量复用优化**：将mediaType提升为顶层常量避免重复创建

---

## 🔬 详细分析

#### ⚠️ 错误处理不规范：Gemini响应解析
**问题描述**: 异常处理分支直接返回字符串错误信息，未通过统一的异常封装或日志记录机制处理
**代码位置**: `GitCrService.kt:442`
**影响**: 可能导致错误信息丢失上下文/堆栈跟踪，维护日志时难以定位问题根源
**修复建议**:
```kotlin
// 建议改用异常包装
throw ResponseParsingException("Gemini response parsing error: $bodyString", e)
```

#### 🔧 顶层常量缺少线程安全注释：JSON_MEDIA_TYPE
**问题描述**: 虽然OkHttpClient线程安全，但未通过注释明确声明常量的线程安全特性
**代码位置**: `GitCrService.kt:366`
**影响**: 其他开发者可能误认为该常量存在线程安全风险导致重复创建
**修复建议**:
```kotlin
/**
 * Thread-safe media type for JSON requests
 * 由OkHttpClient保证线程安全
 */
private val JSON_MEDIA_TYPE = "application/json; charset=utf-8".toMediaType()
```

#### 🔧 冗余空指针检查：candidates数组判断
**问题描述**: 在已确认candidates不为null的情况下，size()检查与get(0)存在逻辑冗余
**代码位置**: `GitCrService.kt:439-440`
**影响**: 增加代码复杂度但未提供额外防护，Gson的size()检查已隐含非空状态
**修复建议**:
```kotlin
// 简化为直接使用elementAtOrNull
val candidate = candidates.elementAtOrNull(0)?.asJsonObject 
```



---

## 🛠 改进建议

- ✅ 建议增加响应结构契约验证（如使用JSON Schema校验）
- ✅ 在异常分支添加日志记录以便追踪错误上下文
- 🔧 将错误提示字符串改为常量定义提升复用性
- ✅ 使用OkHttpClient内置的JSON解析扩展函数优化代码
- 🔧 对JSON_MEDIA_TYPE添加@JvmStatic注解提升Java互操作性




---

## 📦 Diff详情

<details>
<summary>点击展开查看完整代码变更</summary>

```diff
diff --git a/src/main/kotlin/com/wankai/gitcr/GitCrService.kt b/src/main/kotlin/com/wankai/gitcr/GitCrService.kt
index 9ec3496..3c9eaab 100644
--- a/src/main/kotlin/com/wankai/gitcr/GitCrService.kt
+++ b/src/main/kotlin/com/wankai/gitcr/GitCrService.kt
@@ -24,6 +24,8 @@ import java.util.regex.Pattern
 object GitCrService {
 
     private val client: OkHttpClient
+    private val gson = Gson()
+    private val JSON_MEDIA_TYPE = "application/json; charset=utf-8".toMediaType()
 
     init {
         // Configure Unsafe SSL (Match Python's ssl._create_unverified_context)
@@ -364,9 +366,6 @@ $diff
                 prompt
             }
 
-            val gson = Gson()
-            val mediaType = "application/json; charset=utf-8".toMediaType()
-
             val jsonBody = when {
                 isDify -> {
                     val map = mapOf(
@@ -400,7 +399,7 @@ $diff
 
             val requestBuilder = okhttp3.Request.Builder()
                 .url(apiUrl)
-                .post(jsonBody.toRequestBody(mediaType))
+                .post(jsonBody.toRequestBody(JSON_MEDIA_TYPE))
 
             if (!isGemini) {
                 requestBuilder.addHeader("Authorization", "Bearer ${profile.apiKey}")
@@ -438,10 +437,15 @@ $diff
                     } else if (isGemini) {
                         try {
                             val resp = JsonParser.parseString(bodyString).asJsonObject
-                            val text = resp.getAsJsonArray("candidates").get(0).asJsonObject
-                                .getAsJsonObject("content").getAsJsonArray("parts").get(0).asJsonObject
-                                .get("text").asString
-                            text to isTruncated
+                            val candidates = resp.getAsJsonArray("candidates")
+                            if (candidates != null && candidates.size() > 0) {
+                                val text = candidates.get(0).asJsonObject
+                                    .getAsJsonObject("content").getAsJsonArray("parts").get(0).asJsonObject
+                                    .get("text").asString
+                                text to isTruncated
+                            } else {
+                                "Error: No candidates returned from Gemini.\nBody: $bodyString" to false
+                            }
                         } catch (e: Exception) {
                             "Error parsing Gemini response: ${e.message}\nBody: $bodyString" to false
                         }

```

</details>

---

<div align="center">

**📌 评审完成时间**: 2026-01-09 11:54:22  
**🤖 评审引擎**: Auto Git CR Plugin (公司的dify)

---

*本报告由 AI 自动生成，建议结合人工评审使用*

</div>

---

---

## 🔧 技术实现

### 核心流程

```mermaid
graph LR
    A[Git Commit] --> B[触发钩子]
    B --> C[获取 & 优化 Diff]
    C --> D{主要配置可用?}
    D -- No --> E[尝试备用配置 (Retry)]
    D -- Yes --> F[调用 AI 接口]
    E --> F
    F --> G[解析结果 (Score/Issues)]
    G --> H[生成 MD 报告]
    H --> I[保存并通知]
```

### JSON 协议适配
插件内置了针对不同平台的适配逻辑：
- **Gemini**: 原生 `generateContent` 格式
- **Dify**: 适配 `inputs`, `query`, `response_mode` 格式，支持 Streaming 解析
- **OpenAI**: 标准 `chat/completions` 格式

---

## 📝 更新日志

### v1.0.2
- 🌍 **多语言支持** - 新增中英文多语言支持 (English/Chinese)。
- 🧪 **连接测试** - 新增测试按钮，快速验证模型配置。
- 🟢 **状态指示** - 下拉菜单显示配置连接状态 (红/绿)。
- 🐛 **兼容性修复** - 修复 IDE 版本兼容性，支持 2025.1+。

### v1.0.1
- 🔄 **智能重试** - 失败自动切换下一套配置。
- ⚙️ **多配置管理** - 支持多套配置并自动排序。
- 🛠 **高级设置** - 支持 Diff 上下文行数与完整方法切换。

### v1.0.0
- 🚀 **自动化评审** - Git 提交后自动触发 AI 代码评审。
- 🤖 **模型支持** - 支持 Gemini 和 OpenAI/Compatible 模型。
- 📝 **Markdown 报告** - 生成包含评分与建议的详细 Markdown 报告。
- 🔔 **即时通知** - 评审完成后发送包含摘要和评分的通知。

---

## ⚠️ 注意事项

1. **Git 环境**：确保系统 PATH 中包含 `git` 命令。
2. **Dify 用户**：请确保 URL 正确指向 Dify 的 API 端点 (通常包含 `/v1` 或 `/chat-messages`)。
3. **Token 消耗**：开启 `Full Method` 模式会显著增加 Token 消耗，请根据模型额度谨慎使用。

---

## 🤝 贡献

欢迎提交 Issue 和 PR 来改进插件！

---

## 📄 许可证

MIT License

<div align="center">

**Made with ❤️ for better code quality**

</div>
