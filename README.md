# MeaningPE

> 一个正在构建中的自定义 WinPE 环境

[![License](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

**MeaningPE** 一个基于 Windows ADK 的 WinPE 构建项目。
 -- 万物之始，一切之初的仓库
A little PE
[![License](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-alpha-yellow)]()
[![ADK](https://img.shields.io/badge/ADK-10.0.26100.1-orange)]()
---

## 📁 结构

```
MeaningPE/
├── .gitignore           # Git 忽略配置
├── LICENSE              # GPL v3 许可证
├── startnet.cmd         # WinPE 启动后自动执行的脚本（默认调用 `wpeinit`）
├── 使用.txt             # 中文使用说明（占位）
├── ShellBase/           # 自定义 Shell 或图形程序存放目录
├── bootbins/            # 启动所需的二进制文件（bootmgr、boot.sdi、BCD 等）
└── media/               # UEFI 引导文件（用于生成可启动介质）
```

---

## 🚀 当前可用内容

- **startnet.cmd**：PE 启动后会自动执行此脚本，目前仅调用 `wpeinit`，你可以自行修改加入命令或工具。
- **ShellBase/**：预留目录，后续可能放置第三方 Shell（如 Explorer++）或启动 GUI 的脚本。
- **bootbins/**：存放 PE 启动所需的核心文件（如 bootmgr、BCD、boot.sdi 等），已保持完整。
- **media/**：使用 ADK 工具生成的 ISO 或 U 盘写入文件会放在这里。

---

## 📝 构建 - 即步骤

### 1. 准备 ADK 环境
- 安装 Windows ADK 和 Windows PE 加载项。
- 以管理员或更高权限（它会弹出UAC）打开“部署和映像工具环境”，您可以在开始菜单里的推荐项目里找到（如果你是刚安装的话），或在旁边的“搜索”里搜索这个名称（请适配你的语言）。
- <img width="724" height="475" alt="image" src="https://github.com/user-attachments/assets/9bb33a96-e8b7-4e22-9c4c-1e9ed005b2c6" />


### 2. 构建基础 WinPE
```cmd
copype amd64 C:\WinPE_temp
```

### 3. 集成 MeaningPE 的自定内容
- 将本项目的 `startnet.cmd` 复制到 `C:\WinPE_temp\media\Windows\System32\` （覆盖原文件）。
- 如需添加 ShellBase 中的文件，可一并复制到 PE 镜像内的合适位置。例如：
  ```cmd
  xcopy /E .\ShellBase C:\WinPE_temp\mount\ShellBase\
  ```

### 4. 生成 ISO
```cmd
oscdimg -m -o -u2 -lMeaningPE E:\MeaningPE\media E:\MeaningPE\MeaningPE_x64.iso
```

---

## 定制说明

### 修改 `startnet.cmd`

默认仅执行 `wpeinit`。可追加自定义命令，例如：

```cmd
@echo off
wpeinit
X:\Tools\your_app.exe
```

### 添加自定义程序

将可执行文件放入 `ShellBase/`，构建时复制到 PE 的 `X:\ShellBase`，并在 `startnet.cmd` 中调用。

### 注入驱动

1. 创建 `drivers/` 目录，放入 `.inf` 等驱动文件。
2. 挂载映像后执行：

   ```cmd
   dism /Image:C:\WinPE_temp\mount /Add-Driver /Driver:.\drivers /Recurse
   ```

### 添加 WinPE 组件包

示例（添加 PowerShell 支持）：

```cmd
dism /Image:C:\WinPE_temp\mount /Add-Package /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-Scripting.cab"
```

---

## 🔮 计划中的功能

- [x] 项目初始化（目录结构、许可证、启动脚本）
- [ ] 一键构建脚本（自动挂载、注入驱动、打包）
- [ ] 可选组件清单（PowerShell、WMI、.NET）
- [ ] 外置工具支持（自动加载 Tools 目录）
- [ ] 图形化 Shell 选项（基于 ShellBase）

---

## 贡献

欢迎 Issue 和 Pull Request。贡献前请确认：

- 新增代码遵循 GPL v3 或兼容许可证
- 不包含未授权的第三方二进制文件
- 同步更新相关文档

## 📄 许可证

**MeaningPE** 使用 **GPL v3** 许可证。  
任何基于本项目的衍生作品必须同样以 GPL v3 开源。

完整许可证见 [LICENSE](LICENSE) 文件。

---

## 致谢

- [Microsoft Windows ADK](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)
- [WinPE 官方文档](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-intro)

---

**这个项目才刚刚开始，欢迎贡献想法或代码！** 
Awa! - 2026.5.2 -- Write by zhdljc & LaoDay-114
```
