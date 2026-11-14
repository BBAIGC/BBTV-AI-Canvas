# BBTV剪辑开源来源与源码

BBTV剪辑基于 [Concat](https://github.com/jub0t/Concat) 开源项目开发。原作者为 Jareer Abdullah 与 Concat 贡献者，版权为 `Copyright (C) 2026 Jareer Abdullah and Concat contributors`，遵循 **AGPL-3.0-or-later**。参考版本为 0.2.11，参考提交为 `6c9558aa2fd1d62ae75e891ef15817b5cd098aa7`。

## 本仓库中的相关代码

- `components/canvas-video-tools.tsx`、`canvas-video-editor-*` 等 React 适配界面参考并移植 Concat 的界面组织和样式，使用 BBTV 的本地媒体服务、可编辑字幕工程与画布节点。
- `editor-native/concat/` 保存 Concat 完整原生源树。原生编辑器的 Rust/Slint 模块、原版权、`LICENSE` 和 `THIRD_PARTY_NOTICES.md` 一并保留，BBTV 在此基础上接入画布会话与素材、窗口启动及结果回传。
- `editor-companion/`、`desktop/editor-*`、`lib/editor-*` 及相应 API 包含 BBTV 的桌面会话和工程集成。各发布版实际包含的分支、依赖及功能以该版本构建清单为准。

Concat 上游功能不声明为 BBTV 原创。其他独立模块继续遵循各自许可。

## 许可和完整对应源码

AGPL 全文见 [Concat-AGPL-3.0.txt](licenses/Concat-AGPL-3.0.txt)，归属记录见 [Concat-NOTICE.txt](licenses/Concat-NOTICE.txt)。原生源树及依赖的版权和第三方说明应随相关发行物保留。

发布派生版本时，源码交付必须对应实际分发的二进制，包含相关完整源代码、BBTV 修改、依赖版本或获取方式，以及构建和安装所需的脚本与材料。原生分支还应保留上游构建文档、Cargo 清单及锁文件、Slint 界面资源；桌面适配分支还应包含服务端、前端、Electron 入口与打包脚本。不得用未公开的仓库链接、单独的上游源码或旧版源码包替代本版完整对应源码。

源码从本仓库检出后，可查看根目录 `package.json`、`scripts/package-editor-companion.cjs`、`editor-companion/electron-builder.config.cjs`，以及原生源树的 `README.md` 和构建文档。每次实际发布还须提供该版本的构建清单和可用源码包获取地址；网络提供修改版本时也应履行适用的源码提供要求。

BBTV剪辑 0.2.0 的[完整对应源码 ZIP](https://bbtv.ainivox.cn/editor/source/BBTV-Editor-0.2.0-source.zip)包含本版原生源码、BBTV修改、桥接服务与构建材料。

以上归属信息显示在 BBTV 应用的“关于”中，并记录于源码仓库和发行许可材料。
