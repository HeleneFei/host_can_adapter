# TSMaster（同星）SDK 运行库

## 来源

本机已安装的 TSMaster `2025.8.28.1610`，取自安装目录
`Data\SDK\lib\x64` 与 `Data\SDK\lib\x86`。未包含示例工程、Mini Program SDK
与安装包内其它运行库。

## 文件

| 文件 | 大小 | 说明 |
| --- | ---: | --- |
| `TSMaster.h` | 162,279 B | 主头文件（x86/x64 内容相同） |
| `x64/TSMaster.dll` | 1,585,152 B | 64 位接口运行库，版本 2025.8.28.1610 |
| `x64/TSMaster.lib` | 245,658 B | 64 位导入库（C/C++ 链接用） |
| `win32/TSMaster.dll` | 1,080,320 B | 32 位接口运行库，版本 2025.8.28.1610 |
| `win32/TSMaster.lib` | 265,194 B | 32 位导入库（C/C++ 链接用） |

## 版本

- TSMaster 版本：`2025.8.28.1610`（`TSMaster.dll` 文件版本）。
- SDK 头文件在 x86/x64 两个目录中内容一致，本仓库只保留一份。
- 升级 TSMaster 时，`TSMaster.h`、`TSMaster.lib`、`TSMaster.dll` 必须成套替换，
  并与目标机器上的运行环境版本一致。

## 依赖

- PE 导入表只包含系统库：`kernel32.dll`、`ole32.dll`、`user32.dll`、
  `oleaut32.dll`、`advapi32.dll`，不直接依赖 VC 运行库。
- **仍需 TSMaster 运行环境**：该 SDK 库是接口层，实际枚举/打开硬件依赖已安装的
  TSMaster 运行库（安装目录 `bin`/`bin64` 下的 `libTSMaster.dll`、`libTSH.dll`、
  BPL 运行包及 VC 运行库等）。只把 `TSMaster.dll` 拷到目标机不能保证可访问硬件。
- 部署方式二选一：目标机安装同版本 TSMaster；或把安装目录的完整运行库按相对布局
  随包，并保证版本与 SDK 一致。
- `Interop.TSMasterAPI.dll`（.NET 互操作）未包含，使用 C/C++/ctypes 时不需要。

## 已知行为与接入提示

- 支持经典 CAN 与 CAN FD；具体能力以设备/固件为准。
- 时间戳为 64 位微秒级，提供发送回显；判定层仍应确认单位与基准后再使用。
- TSMaster 应用本身会占用设备：上位机与 TSMaster 同时打开同一通道会失败，
  调试时先退出 TSMaster 或释放通道。
- TSMaster 安装目录自带 `ControlCAN.dll` 用于扫描 ZLG 设备；该文件与创芯 ZHCX 的
  `ControlCAN.dll` 同名。若进程同时加载两者，必须按绝对路径加载并校验版本，
  不要依赖文件名或全局 DLL 搜索路径（见 `references/hard-constraints.md` 第 16 条）。

## 许可

TSMaster SDK 为同星专有软件，使用与再分发受 TOSUN 许可协议约束；
SDK 目录内未附许可证文件，请以安装包 EULA 为准。
