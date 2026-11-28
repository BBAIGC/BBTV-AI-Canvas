# BBTV 实时画布 MCP

MCP 操作的是正在打开的 BBTV 窗口：节点、连线、提示词和生成队列实时显示在画布中，并显示操作光标。所有修改都经页面原有操作执行，页面确认后才返回结果。没有可见窗口时拒绝操作，不会建立后台隔离画布。

## 开启与连接

在“设置 → MCP”中开启，复制连接配置。默认关闭；开关、光标偏好、操作速度和随机令牌保存在本机数据目录的 `canvas-mcp.json`。关闭即停止接收新操作；“断开并更换令牌”同时撤销旧连接。已经开始的模型任务仍应在画布队列中检查、取消。

操作速度提供九档：极速、很快、快速、流畅、自然、稍慢、慢速、演示、逐步，默认“快速”。速度调整随心跳立即生效，不更换令牌，也不取消已领取的命令。操作期限根据速度与批量步数计算，最长15分钟；逐步展示的长批次可继续执行，超过期限仍按未知结果处理，不伪报成功或自动重试。

原生 HTTP 地址是当前 BBTV 服务地址加 `/api/mcp`，例如 `http://127.0.0.1:<设置中的实际端口>/api/mcp`，实际端口以设置中的地址为准。使用 MCP Streamable HTTP 的 JSON 响应模式，初始化返回 `Mcp-Session-Id`，后续请求须保留该头。请求携带 `Authorization: Bearer <设置中复制的令牌>`；不提供单独的服务器 SSE 流。

支持 stdio 的智能体也可使用随软件附带的桥。安装包附带桥入口、运行 helper、本文档、`resources/mcp/skill/` 通用 Skill 与 SHA256 清单，不附带模型或额外依赖。安装版示例：

```json
{
  "mcpServers": {
    "bbtv": {
      "command": "<BBTV安装目录>/BBTV.exe",
      "args": ["--max-old-space-size=128", "<BBTV安装目录>/resources/mcp/bbtv-canvas-mcp.cjs", "--base-url", "http://127.0.0.1:<设置中的实际端口>"],
      "env": {
        "ELECTRON_RUN_AS_NODE": "1",
        "NODE_OPTIONS": "",
        "BBTV_MCP_TOKEN": "设置中复制的MCP连接令牌"
      }
    }
  }
}
```

也可用 `--token-file` 或环境变量 `BBTV_MCP_TOKEN_FILE` 指向本机 `canvas-mcp.json`（或只含令牌的文本文件）。桥只读取其中 `token` 字段，令牌轮换后重新连接。源码方式运行：

```powershell
node scripts/bbtv-canvas-mcp.cjs --base-url http://127.0.0.1:<设置中的实际端口> --token-file <用户目录>/.bbtv/runtime/data/canvas-mcp.json
```

服务只接受本机地址和合法来源。stdio 桥禁止远端地址、URL 凭据和重定向，标准输出仅承载 MCP JSON-RPC。无需向智能体提供模型 API 密钥；生成沿用 BBTV 已保存的接口和模型设置。不会自动安装或修改智能体的全局配置。

## 工具与操作

| 工具 | 用途 |
| --- | --- |
| `bbtv_live_control` | 完整任务开始时 `begin` 并声明真实 `agentName`，全部工作完成后 `end`；思考、等待生成期间保持控制 |
| `bbtv_live_status` | 查看已连接、可见窗口及正在执行的命令 |
| `bbtv_live_read` | 读取真实画布摘要；指定 `nodeIds` 返回对应节点完整数据 |
| `bbtv_live_actions` | 发现图片/视频工具栏、右键、设置与历史入口、参数和当前可用状态 |
| `bbtv_live_invoke` | 使用目录中的 `actionId` 调用原界面功能；区分打开面板与加入队列 |
| `bbtv_live_image` | 返回真实图片节点当前所选图片的 JPEG 缩略图，供智能体直接查看结果 |
| `bbtv_live_batch` | 顺序执行多个真实画布操作；用 `requestId` 防止同一批重复提交 |
| `bbtv_live_import` | 将指定本地普通媒体文件上传到软件媒体库，由可见页面创建参考节点；必须提供 `requestId` |
| `bbtv_live_ui` | 读取可见控件，再用快照短 ID 点击、填写、选择、按键、拖动、滚动 |

顶部控制状态统一显示实际名称，例如“MiniMax Agent 正在操作画布”。初始化可用 `clientInfo.name` 或 `clientInfo.title` 提供真实名称；采用 local/runtime 等通用握手名的客户端必须在 `bbtv_live_control begin` 中传实际 `agentName`。思考和等待模型期间保留本次任务会话，空闲时控制标记可自动隐藏；完成后 `end`，手动“停止控制”或关闭 stdio 连接也会释放。

多个可见窗口同时连接时，先用 `bbtv_live_status` 选择 `clientId`，避免操作错误窗口。批量操作支持：

- `create_canvas`、`open_canvas`、`canvas_list`：创建、打开、查看画布。
- `models`：读取实际模型目录；保留用户指定的 `provider`、`model`、`connectionId` 和参数，不猜测模型标识。
- `add_node`、`update_node`、`move_node`、`delete_node`、`duplicate_node`：操作真实节点。
- `connect`、`disconnect`：连接或断开引用；`select_nodes`、`viewport`、`fit_view` 操作选择和视图。
- `import_asset`：引用软件内现有素材链接或明确的 HTTP(S) 素材。导入成品是参考节点，不应伪装成当前节点生成结果。不能通过 MCP 任意读取本机文件。
- `generate`：加入软件可见的原有队列；`queue` 的 `start`、`pause`、`cancel` 控制队列，`cancel_run` 取消对应节点运行。
- `save`、`read`、`ui_snapshot`、`ui`：保存、检查画布或操作其他可见功能。
- `discover_actions`、`invoke_action`：在批次中发现和调用界面能力，参数与上述两个专用工具一致。

### 0.2 模板与任务操作

先读取 `tools/list`、`bbtv_live_status` 与 `bbtv_live_actions`。0.2 返回实际软件版本和功能版本 2；旧版缺少模板动作时停止该调用，或使用其已发现的原界面入口。只更新 Skill 不会升级旧软件。

使用 `bbtv_live_invoke`，为每次写调用传独立 `requestId`：

- `template.list` / `template.status`：只读模板、素材与模型状态，不自动获取或下载模型。状态包括 `ready`、`loading`、`missing-assets`、`missing-models`、`needs-acquire`、`preview-only`。
- `template.sync`：明确请求后启动原共享素材同步；继续用状态查询确认完成。
- `template.acquire {templateId, revision?}`：获取指定已发布模板素材，不创建节点。
- `template.import {path}`：桌面窗口通过原导入服务读取用户指定的本地模板 ZIP。
- `template.apply {templateId, revision?, position?}`：就绪后通过原模板实例化加入当前可见画布、保存并返回真实 `targetNodeId`、`referenceNodeIds` 与 `nodeIds`；不自动生成。用 `update_node` 调整目标提示词、模型、比例、清晰度或 `inputJson` 中 LoRA，再读回确认。
- `queue.status {nodeId?}`：只读取当前画布任务；`node.focus` 用指定节点 ID 定位结果。取消仍使用原 `queue/cancel` 或 `cancel_run`，不得用重提任务代替查询。

模板同步与获取沿用软件原服务、锁和缓存。模型或 LoRA 缺失时如实返回缺项，不默认代替用户下载。打开模板库面板用 `template.open`；弹窗后续操作遵守可见控件快照。

本地测试 Skill ZIP 与安装包内 `skill/` 的文件指纹应一致；公开下载入口只有发布后才会更新，已下载的旧 Skill 需要重新导入新版。

### 替换模板输入素材

`update_node` 只能编辑输入参数，不能直接覆盖媒体。用 `bbtv_live_import` 导入新文件并保留原参考节点；读取模板回执中的引用顺序、目标完整提示词及实际入边。只替换指定位置，断开该目标待重排的整组图像边，再按原语义顺序连接全部参考，并保留原handle及其它用途连线。不要简单把新图追加到末尾。

例如H3替换图1后应为“新姿势、原主体”；替换图2后应为“原姿势、新主体”。仅将提示词中相应旧节点的完整 `@{{节点ID}}` token 换成新节点token，保留其它正文、模型、LoRA、比例与清晰度。重新读取目标和参考节点，核对有序入边、素材URL与绑定后，才按相同顺序提交 `referenceNodeIds`。原图节点不删除，目标旧成图不作为额外参考。

现有读取工具直接返回节点与边，不暴露内部解析器数组；不能把推算说成已读到该数组。通用Skill的操作参考提供可执行的断开、重连和绑定示例，源码回归用真实解析器覆盖了两种替换。

### 工具栏、右键、高清与设置

不要把“操作节点”理解成只能创建节点。先调用 `bbtv_live_actions`，可选 `nodeIds` 指定图片、视频等节点；默认检查所选节点。目录包括每项功能的 `id`、入口、参数定义和执行方式，并按节点媒体、运行/锁定状态与实际模型给出可用状态。返回的 `ui` 是当前真实控件快照；面板打开或菜单展开后重新快照，使用 `bbtv_live_ui` 操作其中的按钮、开关、下拉和输入框。

图片能力包括编辑、反推、高清、H3 细节增强、复刻、资产图/生成宫格、多角度/角度分离、图层分离、修脸、灯光、去水印/背景、裁剪/旋转/标注、对比、预览和下载。视频能力包括复刻、截帧、放大、抠图、替换、字幕、剪辑、音轨、重拍/接续、连续长视频、二次采样及更多视频工具。`node.context-menu` / `canvas.context-menu` 打开原右键菜单；`node.media-select` 切换原结果历史。

传统高清必须读取目录中的 `upscaleModels`，再把用户选择的放大模型和参数放进真实高清请求。不能创建普通生图节点并用“放大”提示词代替，也不能偷偷更换用户选择的模型。示例仅加入原可见队列：

```json
{"clientId":"实际窗口ID","requestId":"原图高清-唯一值","actionId":"image.upscale","nodeId":"原图节点ID","params":{"mode":"queue","model":"从upscaleModels选择的实际ID","options":{"scale":2,"upscaleProfile":"high-fidelity"}}}
```

`image.upscale` 不带参数时打开原高清界面；`image.detail-enhance` 是独立的 H3 Image 细节增强方式，可传预设、提示词和采样参数。`queue` 只表示入队，之后按用户授权启动 `queue/start` 并检查实际结果。原图和上游引用保留，传统放大参数不会被源图的普通生成参数覆盖。

`settings.open` 的 `params.tab` 可选择画布、MCP、前置提示词、本地模型、环境与模型准备、API 接口、素材存储、运行日志、内测交流和关于；画布内页可用 `canvasTab:general/agent/shortcuts`。设置打开后通过真实控件继续操作；`settings.preferences` 和 `settings.shortcuts` 也可读取或更新现有偏好与快捷键存储。API 密钥继续使用专用 `bbtv_configure_api`，普通发现不会回显密钥。`canvas.history` 打开生成历史，`canvas.assets` / `canvas.resources` 打开原资产及资源面板。

可在节点操作中传 `alias`，后续用 `$alias` 引用该批次新节点。调用示例：

```json
{
  "clientId": "从live_status返回复制",
  "requestId": "本次任务唯一标识",
  "actions": [
    { "type": "create_canvas", "title": "雨夜车站" },
    { "type": "add_node", "nodeType": "text", "alias": "story", "position": { "x": 0, "y": 0 }, "data": { "title": "故事主题", "plainText": "雨夜车站，一次迟来的重逢。" } },
    { "type": "add_node", "nodeType": "image", "alias": "shot", "position": { "x": 520, "y": 0 }, "data": { "title": "镜头一", "prompt": "中景，主角望向站台，远处灯光穿过细雨。", "provider": "从models读取", "model": "从models读取", "connectionId": "从models读取" } },
    { "type": "connect", "source": "$story", "target": "$shot" },
    { "type": "save" }
  ]
}
```

上述示例只准备节点。实际生成时另发 `generate`（`intent` 为 `new`、`regenerate` 或 `variant`），再发 `{ "type": "queue", "operation": "start" }` 开始队列。普通重新生成仅适用于当前节点真实生成的结果；参考成品的生成和 `variant` 遵循软件原有新建结果节点规则。

本机已有的参考素材可以直接用 `bbtv_live_import`，无需模拟系统文件选择窗口：

```json
{
  "clientId": "从live_status返回复制",
  "requestId": "本次素材导入唯一标识",
  "path": "<用户指定的原片绝对路径.mp4>",
  "nodeType": "video",
  "alias": "reference-video",
  "title": "参考原片",
  "position": { "x": 0, "y": 0 }
}
```

只接受图片、视频和音频的绝对本地文件路径，大小沿用软件上限（图片/视频 200MB、音频 50MB），同时校验扩展名、文件头和图片像素限制。Windows 还检查本地盘及逐级文件属性，拒绝 UNC、网络映射盘、设备路径、目录、符号链接、junction 和其他重解析点。不执行源文件，不返回文件正文。读取前必须有可见的目标窗口；保存到媒体库后，仍由该窗口创建真实参考节点并确认。相同 `requestId` 的重复调用复用整次导入结果，不再次上传或创建节点。

生成或导入后，可调用 `bbtv_live_image`，传入 `clientId`、真实 `nodeId` 和可选 `maxEdge`（512～1536，默认1024）查看当前选中的图片。它先让页面确认该节点和所选媒体，只读取节点指向的本机 UUID 图片，不接受任意文件路径、外网 URL 或不存在的节点。返回标准 MCP 图片内容及节点 ID、原尺寸、缩略尺寸和读取时间，JPEG 不超过384KB；大段 base64 只出现在图片内容中，不重复放入结构化元数据。外部素材须先导入本机媒体库。

## 结果、失败与重试

页面按顺序执行，第一次失败时停止，错误回传已完成步骤和实际状态。每条命令只投递给选中的窗口一次；只有该窗口确认才返回成功。一个窗口同时只执行一条命令。心跳返回仍有效的命令 ID 和执行代次；关闭、令牌轮换、取消或超时会撤销许可，页面在后续步骤前检查许可并停止继续操作。

`generate` 的返回仅代表进入队列，`queue/start` 也不代表媒体已经完成。必须继续用 `bbtv_live_read` 查看节点 `status`、进度、真实 `lastRunId` 和素材结果。生成可能占用 GPU 或消耗用户选择的云端额度。

调用超时、页面断线或智能体关闭时，已投递操作的结果可能未知，不会重新投递，也不会自动重试收费生成。先读取真实画布状态再决定下一步。相同 `requestId` 和相同参数在当前服务进程中复用原结果；同一键配不同参数会被拒绝。幂等记录不跨软件重启，不能代替真实节点/运行检查。

MCP 支持 `2025-11-25`、`2025-06-18` 和 `2025-03-26`，包括初始化、工具发现/调用和取消。单次输入最多 1MB，每批最多 128 个动作；stdio 输出最多 2MB。默认读取摘要减少传输，必要时指定节点读取完整数据。MCP 结构化操作通常比反复截图识别更节省上下文，具体消耗取决于智能体和操作内容。

旧版 `bbtv_apply_graph`、`bbtv_execute_node` 等后台写入工具不再开放；智能体应迁移到上述实时工具。

## 验证

```powershell
node scripts/verify-bbtv-mcp.cjs
```

聚焦回归使用临时目录、真实 stdio 子进程和页面协议夹具，覆盖默认关闭、令牌/来源校验、会话、一次投递、跨窗口确认拒绝、幂等、部分失败、撤销令牌及轻量打包完整性。不读取用户真实画布，不提交模型生成。实际软件中操作与生成的验收需另行检查真实节点和运行状态。

`scripts/verify-mcp-live.cjs` 可针对正在运行的真实可见窗口做连接烟测。使用 `BBTV_MCP_BASE_URL`、`BBTV_MCP_TOKEN_FILE`（或 `BBTV_MCP_TOKEN`），多窗口时另设 `BBTV_MCP_CLIENT_ID`。默认只读，不改画布；显式设置 `BBTV_MCP_VERIFY_CREATE_CANVAS=1` 时，通过页面新建一个含两个文字节点和连线的验证画布。两种模式均不启动模型生成。
