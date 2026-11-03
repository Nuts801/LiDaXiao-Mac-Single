# 李大霄桌宠 · macOS 单人版

一个使用 Swift、AppKit 和 AVFoundation 制作的透明人物桌宠，内置 10 段原声，支持点击播放、字幕、拖动、音量和大小调整。无接话联动，运行不联网。

> 本仓库目前提供的是**源码 ZIP**，不是可直接运行的 Mac 安装包。素材和脚本检查已完成，尚未在 macOS 上编译或实机验证。

## 下载项目

点击下载：[LiDaXiao-Mac-Single-source.zip](https://github.com/Nuts801/LiDaXiao-Mac-Single/raw/refs/heads/main/LiDaXiao-Mac-Single-source.zip)。

在 Mac 上用 Finder 双击解压，得到 `LiDaXiao-Mac-Single` 文件夹。完整源码、图片、音频、配置、许可及详细说明均在该文件夹中。

## 系统要求

- Apple Silicon Mac（M1、M2、M3 等 M 系列芯片）。
- macOS 14 或以上。
- 从源码构建需要 Xcode Command Line Tools；运行构建好的应用不需要 Python、Node.js 或语音模型。
- 当前构建脚本只面向 Apple Silicon，不支持 Intel Mac。

## 在 Mac 上构建和运行

### 1. 安装编译工具

打开“终端”，执行以下命令并按提示安装。已经安装则跳过。

```bash
xcode-select --install
```

### 2. 进入解压后的项目文件夹

例如将项目放到桌面：

```bash
cd ~/Desktop/LiDaXiao-Mac-Single
```

如果文件夹在“下载”中，则使用：

```bash
cd ~/Downloads/LiDaXiao-Mac-Single
```

请根据实际位置选择路径。不要进入只包含源码 ZIP 的仓库外层目录运行构建命令。

### 3. 构建、自检并打包

```bash
chmod +x build.sh
./build.sh --package
```

脚本会编译应用、复制素材、进行本地签名、运行原生自检并生成分享 ZIP。自检必须输出 `PASS`，且整个脚本无错误结束。

构建输出：

```text
build/李大霄桌宠.app
dist/LiDaXiaoPet-macOS-arm64-1.0.0.zip
dist/LiDaXiaoPet-macOS-arm64-1.0.0.zip.sha256
```

### 4. 启动

```bash
open "build/李大霄桌宠.app"
```

也可以在 Finder 中双击应用，或将它拖进“应用程序”。以后直接打开 `.app` 即可，不必每次重新构建。

## 操作说明

| 操作 | 效果 |
| --- | --- |
| 点击人物 | 随机播放一句，显示字幕和轻微动画 |
| 播放中再次点击 | 停止当前句，播放下一句 |
| 按住拖动 | 移动人物，不触发播放 |
| 右键人物 | 挑选台词、停止播放、调整音量和大小、复位或退出 |
| 点击菜单栏“大霄” | 打开设置菜单 |

应用不占 Dock 图标；位置、音量、大小会自动保存。一轮内台词不重复，相邻轮之间不会连续重复同一句。

## 内置台词

1. 水总水总，我是大霄
2. 国家队在这里，国家队在里面
3. 不要慌，不要慌
4. 先救香港，先救香港，再救内地
5. 放心吧
6. 三千点能守住
7. 要做好人，买好股，得好报
8. 哎呀，心太黑了，那么便宜的东西还往地底下使劲摁
9. 回家做家务
10. 多打几份工

## 分享给别人

在 Mac 上成功构建后，将 `dist/LiDaXiaoPet-macOS-arm64-1.0.0.zip` 发给对方。对方需要符合系统要求的 Mac，完整解压后把“李大霄桌宠.app”拖入“应用程序”并打开，无需安装开发工具。

应用采用临时本地签名，未经 Apple 公证。首次运行如果被系统拦截，可尝试右键应用选择“打开”，或在“系统设置 → 隐私与安全性”中按系统提示允许打开。

## 源码结构与素材替换

```text
Assets/face.png              透明人物图
Assets/AppIcon.icns          Mac 应用图标
Assets/clips.json            音频编号、字幕、文件名、实际时长
Assets/Clips/01.wav–10.wav    十段原声
Sources/main.swift          窗口、交互、播放、字幕、设置、自检
Tools/make-icon.swift       图标生成工具
Tools/validate-assets.py    可选素材检查工具
build.sh                    构建、自检和 ZIP 打包
LICENSE / LICENSE-SunPet    代码许可和署名
MATERIALS.md                素材说明
```

音频统一为 **44.1 kHz、16 位 PCM、单声道 WAV**。替换音频后更新 `clips.json` 中的字幕和实际时长，再重新构建；新增或删除台词时，也需更新源码及检查工具中的数量与编号检查。

重新生成图标：

```bash
swift Tools/make-icon.swift
iconutil -c icns Work/AppIcon.iconset -o Assets/AppIcon.icns
```

只构建、不生成分享包：

```bash
./build.sh
```

## 常见问题

- **Permission denied**：先运行 `chmod +x build.sh`。
- **找不到 swiftc**：完成 `xcode-select --install` 后重试。
- **没有 Dock 图标**：这是正常行为，请查看菜单栏“大霄”。
- **没有声音**：检查桌宠音量、系统音量和输出设备。
- **人物找不到**：菜单栏“大霄”→“移回屏幕右下角”。
- **更新后还是旧内容**：先退出旧程序，再打开新构建的应用。
- **自检失败**：保留完整报错并提交 Issue，不要将失败的构建当作可运行版本分发。

## 来源和许可

基于 [yuhanfu513-ops/FengGe-Desktop-Pet](https://github.com/yuhanfu513-ops/FengGe-Desktop-Pet) 单人物版本修改。原始 MIT 许可与 SunPet 代码署名保留在源码包的 `LICENSE`、`LICENSE-SunPet` 中。

代码许可不包含人物照片、肖像和原声音频的权利；相关权利归相应权利人。详见源码包内 `MATERIALS.md`。
