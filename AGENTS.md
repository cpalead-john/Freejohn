# Freejohn 项目代理配置

## 总指挥 (Sisyphus)

**模型**: Kimi K2.5 (阿里云)
**角色**: 全局编排与代码修改总指挥

### 核心职责
- 分析需求并制定执行计划
- 委派专业子代理并行执行任务
- 验证子代理输出并整合结果
- 确保代码质量和项目一致性

### 工作模式
1. **Phase 0 - 意图识别**: 分类请求类型（研究/实现/修复）
2. **Phase 1 - 代码库评估**: 评估项目结构和现有模式
3. **Phase 2 - 执行**:
   - 探索/研究: 使用 explore/librarian 代理
   - 实现: 使用 category + skills 委派
   - 验证: 使用 lsp_diagnostics 和测试
4. **Phase 3 - 完成**: 确保所有验证通过

## 子代理配置

### 1. Explore Agent
**用途**: 代码库内部探索
**触发条件**:
- 查找代码模式
- 理解模块结构
- 搜索特定实现
**执行方式**: `run_in_background=true`，并行执行

### 2. Librarian Agent
**用途**: 外部资源调研
**触发条件**:
- 未知库/框架
- 官方文档查询
- 最佳实践检索
**执行方式**: `run_in_background=true`，并行执行

### 3. Deep Agent
**用途**: 深度实现任务
**触发条件**:
- 复杂功能实现
- 多步骤修改
**分类**: `category="deep"`

### 4. Visual Engineering Agent
**用途**: UI/UX 相关任务
**触发条件**:
- 界面修改
- 样式调整
- 布局优化
**分类**: `category="visual-engineering"`

## 可用技能 (Skills)

| 技能名称 | 用途 | 优先级 |
|---------|------|--------|
| git-master | Git 操作 | 内置 |
| frontend-ui-ux | 前端 UI/UX | 内置 |
| playwright | 浏览器自动化 | 内置 |
| dev-browser | 开发浏览器 | 内置 |

## 项目特定规则

### 品牌一致性
- 应用名称: `Freejohn`
- 包名: `moe.freejohn`
- 禁止出现: `NekoBox`, `MatsuriDayo`

### 代码规范
- 使用 Kotlin 编写 Android 代码
- 遵循现有项目结构
- 修改前检查 lsp_diagnostics

### 文件路径约定
- 源码: `app/src/main/java/`
- 资源: `app/src/main/res/`
- 配置: `app/build.gradle`, `build.gradle.kts`

## 会话记录

| 会话 | 日期 | 总指挥 | 主要内容 |
|------|------|--------|----------|
| Session 1 | 2026-02-25 | Gemini 3 Flash | 品牌重构、UI净化 |
| Session 2 | 2026-02-25 | Kimi 2.5 / Sisyphus | 品牌重构、UI净化（续）、修复 AboutFragment.kt、清理 strings.xml |
| Session 3 | 2026-02-26 | Antigravity / Sisyphus | 编译 libcore.aar、修复编译错误、成功生成并安装 APK、修复 VPN 连接问题 |
| Session 4 | 2026-02-27 | Antigravity / Sisyphus | Oracle 审计修复品牌残留、重命名属性文件、记录应用使用注意事项（备份、路由数据库） |
| Session 5 | 2026-04-06 | Kimi K2.5 / Sisyphus | 全面审计、安全修复（release.keystore 移除、依赖升级、snakeyaml CVE、okhttp alpha→稳定）、错误处理统一（e.printStackTrace→Logs）、上游同步评估（sing-box 1.12.19-neko-1 已同步）、版本号升级至 1.5.0、APK 编译验证通过 |

## 审计与修复记录（Session 5）

### 安全修复
- `release.keystore` 已从 git 追踪中移除，加入 `.gitignore`
- `snakeyaml` 1.30 → 1.33（修复 CVE）
- `okhttp` 5.0.0-alpha.3 → 4.12.0（稳定版）
- 其他依赖升级：kotlinx-coroutines 1.8.1、core-ktx 1.12.0、material 1.11.0、guava 33.0.0 等

### 代码质量修复
- 11 处 `e.printStackTrace()` 统一替换为 `Logs.w(e)` 或 `Logs.INSTANCE.e(e)`
- `okhttp3.internal.closeQuietly` 替换为安全的 try-catch close（OkHttp 4.12 兼容）
- `work-runtime-ktx` 保持 2.8.1（2.9 有破坏性 API 变更）

### 上游同步评估
- 本地 `sing-box` 已是 `1.12.19-neko-1`（最新）
- 本地 `libneko` 已是 `1.12.11-neko-1`
- Freejohn 基础 commit 为上游 1.4.2 tag，所有 1.4.0→1.4.2 的 Kotlin 层改动已包含
- 重新编译 `libcore.aar` 后获得最新 Go 核心功能

### 版本信息
- `VERSION_NAME`: 1.4.2 → 1.5.0
- `VERSION_CODE`: 46 → 48
- `PRE_VERSION_NAME`: pre-1.5.0-20260406-1
- APK 输出: `Freejohn-1.5.0-fdroid-arm64-v8a-debug.apk`

## libcore 编译说明

### 编译环境
- Go: 1.23.0
- GOPATH: `C:\Users\Administrator\go`
- Android SDK: `C:\Users\Administrator\AppData\Local\Android\Sdk`
- Android NDK: `C:\Users\Administrator\AppData\Local\Android\Sdk\ndk\27.0.12077973`
- gomobile: `C:\Users\Administrator\go\bin\gomobile.exe`

### 本地依赖源码位置
- `F:\VPS\Project\sing-box` → 对应 `github.com/sagernet/sing-box`
- `F:\VPS\Project\libneko` → 对应 `github.com/matsuridayo/libneko`
- `libcore/go.mod` 中已配置 `replace` 指令指向上述本地路径

### 编译命令（必须包含所有 tags）
```bash
export ANDROID_HOME="/c/Users/Administrator/AppData/Local/Android/Sdk"
export ANDROID_NDK_HOME="/c/Users/Administrator/AppData/Local/Android/Sdk/ndk/27.0.12077973"
export PATH="/c/Users/Administrator/go/bin:$PATH"
cd /f/VPS/Project/Freejohn/libcore
gomobile bind -v -target=android/arm64 -androidapi 21 -tags "with_gvisor with_utls" -o ../app/libs/libcore.aar .
```

### 必须包含的 Build Tags 及原因
| Tag | 原因 |
|-----|------|
| `with_gvisor` | 缺少时 tun 模式启动失败，报 `gVisor is not included in this build` |
| `with_utls` | 缺少时 VLESS+Reality 协议连接失败，报 `uTLS is not included in this build` |

### APK 输出位置
`app/build/outputs/apk/fdroid/debug/Freejohn-{version}-fdroid-arm64-v8a-debug.apk`

## 应用使用说明（注意事项）

### 1. 配置备份与恢复
备份功能**不在“设置”中**，而在侧边栏菜单的 **“工具” (Tools) -> “备份与恢复”** 中。
导出时文件默认以 `freejohn_backup_` 命名，覆盖安装前建议先导出配置以防数据丢失。

### 2. 路由与地理数据库（geosite/geoip）
当启用“绕过中国大陆IP”或“绕过中国大陆域名”等路由规则时，应用必须加载 `geosite.db` 和 `geoip.db` 文件。
由于这俩文件未打包进 APK，用户在新安装后如果直接连接 VPN，会报 `open .../files/geosite.db: no such file or directory` 错误。
**解决方法**：进入侧边栏的 **“路由” (Route)**，点击右上角的 **“规则资源”** 图标，手动下载/更新这两个文件，下载完成后即可正常连接。
