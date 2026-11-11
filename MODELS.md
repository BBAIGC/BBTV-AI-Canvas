# 模型与配套资源

[返回主页](README.md) · [统一资源库](https://pan.baidu.com/s/1DNzXSlrRB0bNPPT78RdrRQ?pwd=rpmx)（提取码 **rpmx**）· [完整文件清单](docs/model-distribution-inventory.json)

## 先选功能，再准备依赖

桌面安装包、推理引擎与模型权重分别准备。已存在的 ComfyUI 可以通过额外模型路径复用权重；同一 SHA256 文件无需重复下载或移动原文件。

文件清单日期：2026-11-12。以下大小按十进制 GB 统计，是磁盘文件大小，不是显存需求。完整功能还需推理引擎、Python 依赖、运行缓存与解压空间。

| 层级 | 文件数 | 文件合计 |
|---|---:|---:|
| 基础必需 | 28 | 103.87 GB |
| 可选扩展 | 115 | 251.77 GB |
| 合计 | 143 | 355.64 GB |

## 当前主要路线：含配套依赖

| 应用中的模型路线 | 所需文件合计 | 必要文件与模型内相对位置 |
|---|---:|---|
| Qwen Image 2.1 · Q6_K | 15.90 GB | `diffusion_models/qwen-image-2.1-UC-Q6_K.gguf`<br>`text_encoders/qwen3vl_8b_int8_convrot.safetensors`<br>`vae/qwen_image_2.1_vae_bf16.safetensors` |
| Qwen Image 2.1 Turbo | 17.28 GB | `diffusion_models/qwen_image_2.1_turbo_int8_convrot.safetensors`<br>`text_encoders/qwen3vl_8b_int8_convrot.safetensors`<br>`vae/qwen_image_2.1_vae_bf16.safetensors` |
| Krea 2 Turbo | 18.64 GB | `diffusion_models/krea2_turbo_fp8_scaled.safetensors`<br>`text_encoders/qwen3vl_4b_fp8_scaled.safetensors`<br>`vae/qwen_image_vae.safetensors` |
| MiniMax H3 · 基础视频 | 42.03 GB | `diffusion_models/minimax_h3_fl2va_pruned_int8_convrot.safetensors`<br>`loras/minimax_h3_fl2v_turbo_8step_v1.0_comfyui_bf16.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`<br>`vae/minimax_h3_audio_vae_fp32.safetensors`<br>`vae/minimax_h3_video_vae_int8_convrot.safetensors` |
| MiniMax H3 · 参考视频 | 40.07 GB | `diffusion_models/minimax_h3_ref2va_pruned_int8_convrot.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`<br>`vae/minimax_h3_audio_vae_fp32.safetensors`<br>`vae/minimax_h3_video_vae_int8_convrot.safetensors` |
| VOSR2 | 6.97 GB | `vosr2/VOSR2/Qwen-Image-vae-2d/config.json`<br>`vosr2/VOSR2/Qwen-Image-vae-2d/diffusion_pytorch_model.safetensors`<br>`vosr2/VOSR2/args.json`<br>`vosr2/VOSR2/checkpoints/ema_model.safetensors`<br>`vosr2/VOSR2/dinov2_vitl14.safetensors`<br>`vosr2/VOSR2/source_revision.json` |
| MOSS Image · Jib V2 INT8 | 24.97 GB | `diffusion_models/jibMixMossRealistic_v2RealityRenew.safetensors`<br>`text_encoders/qwen3.5_9b_bf16.safetensors`<br>`vae/flux2-vae_small.safetensors` |
| MOSS Image · 官方 SFT 生图 | 29.72 GB | `diffusion_models/moss_image_sft_bf16.safetensors`<br>`text_encoders/qwen3.5_9b_bf16.safetensors`<br>`vae/flux2-vae_small.safetensors` |
| MOSS Image · 官方 SFT 原生编辑 | 30.02 GB | `diffusion_models/moss_image_sft_bf16.safetensors`<br>`text_encoders/qwen3.5_9b_bf16.safetensors`<br>`vae/flux2-vae_small.safetensors`<br>`loras/MOSS-Image/moss_image_edit_lora_rank_64_bf16.safetensors` |
| MOSS（小红书特调 V1） | 24.97 GB | `diffusion_models/QAQMoss_merging_8steps_int8_v1.safetensors`<br>`text_encoders/qwen3.5_9b_bf16.safetensors`<br>`vae/flux2-vae_small.safetensors` |
| MOSS（小红书特调 V2） | 24.97 GB | `diffusion_models/QAQ_MOSS_v2_8steps_int8.safetensors`<br>`text_encoders/qwen3.5_9b_bf16.safetensors`<br>`vae/flux2-vae_small.safetensors` |
| MiniMax H3 Image · 图片生成与编辑 | 29.41 GB | `diffusion_models/minimax_h3_image_pruned_w4a8.safetensors`<br>`vae/minimax_h3_image_vae_fp16.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` |
| Qwen 2.1 相机控制 | 16.22 GB | `diffusion_models/qwen-image-2.1-UC-Q6_K.gguf`<br>`text_encoders/qwen3vl_8b_int8_convrot.safetensors`<br>`vae/qwen_image_2.1_vae_bf16.safetensors`<br>`loras/qwen21_multiple_angles_v2_step1500.safetensors`<br>`licenses/Qwen21-Multiple-Angles-LICENSE.txt`<br>`licenses/Qwen21-Multiple-Angles-NOTICE.txt` |
| H3 人物替换 | 40.23 GB | `diffusion_models/minimax_h3_ref2va_pruned_int8_convrot.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`<br>`vae/minimax_h3_audio_vae_fp32.safetensors`<br>`vae/minimax_h3_video_vae_int8_convrot.safetensors`<br>`loras/h3_character_swap_v1.5.safetensors`<br>`licenses/Character-Swap-H3-LICENSE.txt`<br>`licenses/Character-Swap-H3-NOTICE.txt` |
| H3 人物替换官方8步 | 42.19 GB | `diffusion_models/minimax_h3_ref2va_pruned_int8_convrot.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`<br>`vae/minimax_h3_audio_vae_fp32.safetensors`<br>`vae/minimax_h3_video_vae_int8_convrot.safetensors`<br>`loras/h3_character_swap_v1.5.safetensors`<br>`loras/minimax_h3_fl2v_turbo_8step_v1.0_768p_comfyui_bf16.safetensors`<br>`licenses/Character-Swap-H3-LICENSE.txt`<br>`licenses/Character-Swap-H3-NOTICE.txt` |
| FastH3 Omni 8步 | 41.32 GB | `diffusion_models/fasth3_omniref_8step_pruned_int8_convrot.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`<br>`vae/minimax_h3_audio_vae_fp32.safetensors`<br>`vae/minimax_h3_video_vae_int8_convrot.safetensors`<br>`licenses/MiniMax-H3-Community-LICENSE.txt` |
| H3 动作控制 | 44.61 GB | `diffusion_models/minimax_h3_fl2va_pruned_int8_convrot.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`<br>`vae/minimax_h3_audio_vae_fp32.safetensors`<br>`vae/minimax_h3_video_vae_int8_convrot.safetensors`<br>`model_patches/minimax_h3_fun_controlnet_union_2.0_pruned_int8_convrot.safetensors`<br>`licenses/MiniMax-H3-Community-LICENSE.txt` |
| SeedVR2 官方视频 | 3.96 GB | `diffusion_models/seedvr2_3b_int8_convrot.safetensors`<br>`vae/seedvr2_ema_vae_fp16.safetensors` |
| 视频人脸预修复 | 0.35 GB | `facerestore_models/GFPGANv1.4.pth`<br>`facedetection/detection_mobilenet0.25_Final.pth`<br>`licenses/GFPGAN-LICENSE.txt`<br>`licenses/facexlib-LICENSE.txt` |
| DWPose 骨架视频 | 0.35 GB | `dwpose/dw-ll_ucoco_384.onnx`<br>`dwpose/yolox_l.onnx`<br>`licenses/DWPose-LICENSE.txt`<br>`licenses/YOLOX-LICENSE.txt` |
| Pika H3 Flex 6步 | 42.03 GB | `diffusion_models/minimax_h3_fl2va_pruned_int8_convrot.safetensors`<br>`text_encoders/qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`<br>`vae/minimax_h3_audio_vae_fp32.safetensors`<br>`vae/minimax_h3_video_vae_int8_convrot.safetensors`<br>`loras/pika_h3_flex_fl2va_v1_bf16.safetensors`<br>`licenses/MiniMax-H3-Community-LICENSE.txt` |

共享编码器、VAE 只需安装一次；基础与可选目录可以共同引用。H3 基础路线的合计包含当前默认 8 步加速 LoRA；参考视频路线与基础路线需要不同的主模型，不能只下载其中一个权重就认为两条路线都可用。

可选 **FastH3 Omni** 的 22.2 GB 文件超过网盘单文件限制，正在后台分包同步；这部分尚未计入已同步的 142 文件。可从清单中的官方固定版本直链安装，或复用已有本地模型；未安装时应用会明确显示不可用。

## 全部功能组合

| 模型或功能 | 组合文件大小 | 依赖说明 |
|---|---:|---|
| Qwen Image 2.1 | 15.90 GB | 图片生成或编辑；按选择安装，非全部必需 |
| Qwen Image 2.1 Turbo | 17.28 GB | 图片生成或编辑；按选择安装，非全部必需 |
| MOSS Image · Jib V2 | 24.97 GB | Jib V2 INT8 图片生成；不冒充官方 SFT 原生编辑路线 |
| MOSS Image · 官方 SFT | 29.72 GB | 官方 SFT 图片生成；原生编辑另需编辑 LoRA，见对应完整组合 |
| Z-Image Turbo | 20.69 GB | 图片生成或编辑；按选择安装，非全部必需 |
| Z-Image | 20.69 GB | 图片生成或编辑；按选择安装，非全部必需 |
| Krea 2 Turbo | 18.64 GB | 图片生成或编辑；按选择安装，非全部必需 |
| MiniMax H3 基础/参考视频 | 61.04 GB | 基础视频需fl2va；参考视频需ref2va；VAE/编码器为配套 |
| H3 加速/延长 LoRA | 3.98 GB | 按实际开启的8步、4步或时长扩展安装；参考模式默认标准采样 |
| OmniSR X2 | 0.00 GB | 轻量图片超分 |
| SeedVR2 3B | 7.85 GB | SeedVR2超分路线的模型及VAE |
| VOSR2 | 6.97 GB | VOSR2完整模型目录（不含.cache），使用目录内args/checkpoints/VAE/特征权重 |
| Qwen3-TTS-12Hz-0.6B-CustomVoice | 3.18 GB | 本地配音/声音克隆；上传完整模型文件夹并包含speech_tokenizer，排除.cache |
| Qwen3-TTS-12Hz-0.6B-Base | 1.83 GB | 本地配音/声音克隆；上传完整模型文件夹并包含speech_tokenizer，排除.cache |
| SenseVoice Small | 0.94 GB | 语音输入/字幕识别，三个文件均需 |
| Demucs HTDemucs | 0.08 GB | 视频人声/伴奏分离 |
| SAM ViT-B / ViT-H | 2.94 GB | 图层分离/视频抠像默认优先B，小模型足够时无需H |
| U2Net / U2NetP | 0.18 GB | 去背景质量/快速模式 |
| Depth Anything V2 Small | 0.10 GB | 视频深度图需要的权重；插件代码属于引擎组件 |
| 人脸替换依赖 | 0.19 GB | 仅对应视频主体替换功能可选，不列为图片/视频基础生成必需 |
| uncensored-gemma4:12b-balanced | 7.56 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-gemma4:e2b-aggressive | 6.02 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-gemma4:e4b-aggressive | 7.24 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3-vl:8b-aggressive | 7.89 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3-vl:8b-balanced | 7.89 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3.5:27b-aggressive-q3_k_m | 14.22 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3.5:2b-aggressive | 2.01 GB | 文字提示词优化；无图像投影器不用于图片理解；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3.5:4b-aggressive | 4.14 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3.5:9b-aggressive | 8.28 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3.6:27b-aggressive-q3_k_p | 15.23 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| uncensored-qwen3.8:27b-aggressive-q3_k_p | 14.37 GB | 视觉理解/图片反推/提示词优化；按用户选择安装，manifest及所有引用blob一并上传，保持manifests/blobs相对结构 |
| MOSS Image · 官方 SFT 原生编辑 | 30.02 GB | 官方SFT配合编辑LoRA、原生参考编码与第22层引导；Jib V2不冒充原生编辑 |
| MOSS（小红书特调 V1） | 24.97 GB | 社区融合版图片生成；共享既有编码器与 VAE |
| MOSS（小红书特调 V2） | 24.97 GB | 社区融合版图片生成；共享既有编码器与 VAE |
| MiniMax H3 Image · W4A8 | 29.41 GB | 图片生成与编辑 |
| Real-ESRGAN x4plus | 0.07 GB | 原生4倍快速放大；可作为独立后处理；动漫版本适用于插画/动漫 |
| Real-ESRGAN x4plus 动漫 6B | 0.02 GB | 原生4倍快速放大；可作为独立后处理；动漫版本适用于插画/动漫 |
| Qwen 2.1 相机控制 V2 | 16.22 GB | Qwen 2.1 相机控制 V2；效果按实际任务核对。 |
| H3 人物替换 V1.5 | 40.23 GB | H3 人物替换 V1.5；效果按实际任务核对。 |
| H3 人物替换 V1.5 官方8步 | 42.19 GB | H3 人物替换 V1.5 官方8步；效果按实际任务核对。 |
| FastH3 Omni 8步 | 41.32 GB | FastH3 Omni 8步；效果按实际任务核对。 |
| H3 FunControl Union 2.0 | 44.61 GB | H3 FunControl Union 2.0；效果按实际任务核对。 |
| SeedVR2 官方视频时序放大 | 3.96 GB | SeedVR2 官方视频时序放大；效果按实际任务核对。 |
| GFPGAN 视频人脸预修复 | 0.35 GB | GFPGAN 视频人脸预修复；效果按实际任务核对。 |
| DWPose 动作骨架 | 0.35 GB | DWPose 动作骨架；效果按实际任务核对。 |
| Pika H3 Flex 6步 | 42.03 GB | Pika H3 Flex 6步；效果按实际任务核对。 |

组合之间存在共享文件，不能把每行直接相加当成整库体积。SAM B/H、U2Net/P 等替代项按所选模式安装，不必同时加载全部模型。

## 官方模型、量化版与本地名称

Qwen Image 2.1 当前普通路线使用 **Warumauchimmer/Qwen-Image-2.1-Uncensored-Quantized 的 Q6_K GGUF**，主模型约 5.88 GB；它与官方 BF16（约 14.23 GB）及官方 INT8（约 7.26 GB）不同，不能只看显示名称替换。当前 Turbo 路线使用官方 Turbo INT8 权重，约 7.26 GB；完整依赖大小见上表。

MOSS Image 的 **Jib V2** 使用 J1B 在 Civitai 发布的 INT8 版本。清单中的本地文件名为 `jibMixMossRealistic_v2RealityRenew.safetensors`，已经按 SHA256 对应到原始文件 `Jib_Mix_Moss_V2_H_int8_00001_.safetensors`，不是官方 SFT。精确来源：[模型版本 3435334](https://civitai.com/models/3016339?modelVersionId=3435334)，[下载文件 3332190](https://civitai.com/api/download/models/3435334?fileId=3332190)。

MOSS 官方 SFT 原生编辑还需要约 **297.38 MB** 的 `moss_image_edit_lora_rank_64_bf16.safetensors`。最初的 114 文件清单漏掉过此适配器；请以本页链接的最新 JSON 为准核对它是否已列入资源库，不能只下载主模型、编码器与 VAE。附加资源及是否已纳入分享清单也记录在 JSON 中。

每个文件的来源、SHA256、大小、推荐安装相对路径和配套关系见[完整清单](docs/model-distribution-inventory.json)。部分引擎或本地资源仅有已核验文件证据，没有确认的公开原始直链；请使用真实分享包或已有文件，不把网盘分享页当作权重文件直链。

## 安装核对

1. 按本机硬件选择需要的功能组合。
2. 对照 JSON 核对每个文件的大小与 SHA256，同时核对依赖。
3. 把模型放入对应相对目录，或配置 ComfyUI 的 `extra_model_paths.yaml` 复用现有目录。
4. 在 BBTV 中选择引擎，刷新模型列表，完成该功能的最小验证。

三套完整智能体安装提示词见 [AGENT_INSTALL.md](AGENT_INSTALL.md)。模型和组件各自遵循原作者许可证，下载前可在清单的来源链接查看。
