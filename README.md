# Overlayer（简体中文汉化版）

一个 Windows 小工具，可以把任意窗口变成游戏/程序的**叠加层（Overlay）**。通过给窗口开启
**置顶（Topmost）**、**穿透点击（Clickthrough）**、**透明（Transparency）** 三种效果，
并组合使用，就能把音乐播放器、聊天软件、笔记等窗口“贴”在游戏画面上。

## 项目来源与致谢（重要）

- **上游原始项目**：<https://github.com/PsychedelicShayna/overlayer>
- **原作者**：**PsychedelicShayna**（GitHub 主页：<https://github.com/PsychedelicShayna>）
- **原项目许可证**：GPL-3.0（见 [LICENSE](LICENSE)）

本仓库是上述项目的**非官方简体中文汉化版（fork）**，代码逻辑、架构与功能均归原作者所有，
本项目**仅对界面文本进行了简体中文翻译**，未改动任何功能逻辑。

> 上游项目还包含三个子模块，同样由 PsychedelicShayna 编写，本仓库已将其源码一并内置：
> - <https://github.com/PsychedelicShayna/indigo-stylesheet>
> - <https://github.com/PsychedelicShayna/qt-hotkey-recorder-widget>
> - <https://github.com/PsychedelicShayna/qt-process-scanner-widget>

## 汉化说明

- **汉化方式**：由 **AI 编程助手 TraeCode（Trae CN IDE）** 完成，使用模型 **DeepSeek-V4.1-Flash**。
- **汉化范围**：
  - 主窗口界面文本（`source/main_window_dialog.ui`）、按钮与提示文本（`source/main_window_dialog.cxx`）
  - 进程/窗口扫描器界面文本（`submodules/qt-process-scanner-widget/process_scanner_dialog.ui`）
  - 扫描器运行状态、右键菜单、可见性状态（`process_scanner_dialog.cxx`、`process_scanner.cpp`）
- **保持不变**：窗口标题 `Overlayer`、程序名称、`Ctrl + Shift + ...` 等键盘快捷键写法（通用约定）。
- 汉化由 AI 自动翻译，**可能存在措辞偏差**，欢迎提交 Issue 或 Pull Request 指正。

## 功能说明

窗口列表分为**非活动窗口**与**活动窗口**两栏，只有“活动窗口”才会被应用修改：

1. **置顶（Enable / Disable Topmost）**：让窗口始终保持在其他窗口之上。
2. **穿透点击（Enable / Disable Clickthrough）**：鼠标点击会穿透窗口，窗口无法获得焦点。
   可配合热键临时开关，方便临时操作该窗口。
3. **透明（Enable / Disable Transparency）**：按下方滑杆（0-255 Alpha）设置窗口不透明度。

其他操作：

- **搜索窗口**：打开进程/窗口扫描器，从列表中选择目标窗口。
- **抓取窗口**：点击后切换到目标窗口，即可自动抓取该窗口。
- **设为活动 / 设为非活动**：在两个列表之间移动窗口，临时启用/停用修改。
- **重置所选窗口**：把窗口恢复为程序启动时记录的原始状态。
- 程序退出时会自动还原所有窗口修改，无需担心“改坏了”无法恢复。
- 列表中选中窗口后按 `Delete` 键可快速移除。

## 构建方法（Windows + Qt 6）

### 环境要求

- Qt 6（含 `core gui widgets`），推荐 6.8.x
- 编译器：MinGW 13.1（与 Qt 的 MinGW 版本匹配）或 MSVC 2019/2022
- qmake（随 Qt 提供）

### 使用 qmake 构建

```powershell
# 将 Qt 与 MinGW 的 bin 目录加入 PATH（请改成自己的安装路径）
$env:PATH = "C:\Qt\Tools\mingw1310_64\bin;C:\Qt\6.8.3\mingw_64\bin;" + $env:PATH

mkdir build
cd build
qmake ..\overlayer.pro -spec win32-g++ -config release
mingw32-make -f Makefile.Release -j8
```

生成的程序位于 `build\release\overlayer.exe`。

### 部署运行库与样式表

```powershell
windeployqt --release build\release\overlayer.exe

# 程序启动时会读取 ./styles/indigo.qss，并可选加载 indigo.rcc 图标资源
mkdir build\release\styles
rcc ..\submodules\indigo-stylesheet\indigo.qrc -o build\release\styles\indigo.rcc
copy ..\submodules\indigo-stylesheet\indigo.qss build\release\styles\
```

> 程序运行时的工作目录必须是包含 `styles/` 的那一层（即 exe 所在目录）。

## 与上游的差异

| 改动 | 说明 |
| --- | --- |
| 界面文本汉化 | 主窗口 + 进程扫描器 + 状态提示 |
| `overlayer.pro` | `/std:c++17` 改为仅 MSVC 生效，使 MinGW 可以编译 |
| `overlayer.pro` | 热键控件路径修正为实际子模块目录 `submodules/qt-hotkey-recorder-widget/source` |
| 子模块 | 由 git submodule 改为内置源码，以便把汉化后的文件一起发布 |

功能逻辑与上游保持一致。

## 许可证

本项目沿用上游的 **GNU General Public License v3.0**，详见 [LICENSE](LICENSE)。
由于使用了 GPL-3.0，分发时请保留原始版权声明与许可证文本。

## 免责声明

本项目与 PsychedelicShayna 官方无关，属社区汉化版本。若原项目作者有异议，
可联系删除本仓库。
