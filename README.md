# MeaningPE

> 一个正在构建中的自定义 WinPE 环境

[![License](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

**MeaningPE** 是一个基于 Windows ADK 的 WinPE 构建项目。  
A little PE
---

## 📁 当前项目结构

```
MeaningPE/
├── .gitignore           # Git 忽略配置
├── LICENSE              # GPL v3 许可证
├── startnet.cmd         # PE 启动脚本（可自定义）
├── 使用.txt             # 中文使用说明（占位）
├── ShellBase/           # 自定义 Shell 相关文件（待补充）
├── bootbins/            # 启动必备二进制文件（bootmgr 等）
└── media/               # 生成的 ISO 镜像输出目录
```

---

## 🚀 当前可用内容

- **startnet.cmd**：PE 启动后会自动执行此脚本，目前仅调用 `wpeinit`，你可以自行修改加入命令或工具。
- **ShellBase/**：预留目录，后续可能放置第三方 Shell（如 Explorer++）或启动 GUI 的脚本。
- **bootbins/**：存放 PE 启动所需的核心文件（如 bootmgr、BCD、boot.sdi 等），已保持完整。
- **media/**：使用 ADK 工具生成的 ISO 或 U 盘写入文件会放在这里。

---

~~把大象装进冰箱需要几步？~~

## 📝 如何使用现在的内容？

### 1. 准备 ADK 环境
- 安装 Windows ADK 和 Windows PE 加载项。
- 以管理员身份打开“部署和映像工具环境”。

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

## 🔮 计划中的功能

- [ ] 一键构建脚本（自动挂载、注入驱动、打包）
- [ ] 可选组件清单（PowerShell、WMI、.NET）
- [ ] 外置工具支持（自动加载 Tools 目录）
- [ ] 图形化 Shell 选项（基于 ShellBase）

---

## 📄 许可证

**MeaningPE** 使用 **GPL v3** 许可证。  
任何基于本项目的衍生作品必须同样以 GPL v3 开源。

完整许可证见 [LICENSE](LICENSE) 文件。

---

## 🙏 致谢

- [Windows ADK](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)
- WinPE 社区

---

**项目才刚刚开始，欢迎贡献想法或代码！** 
Awa! - 2026.5.2 -- Write by zhdljc & LaoDay-114
```

这个 README 不过度承诺，清晰说明了目前只有哪些文件、如何使用它们、以及未来计划。你可以直接复制使用，并根据项目进展随时更新。
