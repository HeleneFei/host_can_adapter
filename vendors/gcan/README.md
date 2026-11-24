# GCAN（广成）ECanVci 运行库

## 来源

厂商随附资料包中的“二次开发 DLL / ECANVCI 库文件（32 位、64 位）”。
未包含产品手册、规格书、示例工程与截图。

## 文件

| 文件 | 大小 | 说明 |
| --- | ---: | --- |
| `ECanVci.h` | 4,336 B | 接口原型、结构体、错误码、常量 |
| `x64/ECanVci64.dll` | 111,616 B | 64 位主运行库，版本 3.0 |
| `x64/CHUSBDLL64.dll` | 18,944 B | 64 位 USB 依赖库，必须与主库同目录 |
| `x64/ECanVci64.lib` | 4,694 B | 64 位导入库（C/C++ 链接用） |
| `win32/ECanVci.dll` | 49,152 B | 32 位主运行库 |
| `win32/CHUSBDLL.dll` | 12,288 B | 32 位 USB 依赖库，必须与主库同目录 |
| `win32/ECanVci.lib` | 4,916 B | 32 位导入库（C/C++ 链接用） |

## 使用要点

- 主 DLL 与 `CHUSBDLL*.dll` 必须放在同一目录；只拷主 DLL 会报 `WinError 126`。
- 厂商资料中另有一套“新 ECanVci.dll”版本，依赖 `MSVCR80.dll`；
  为保证干净机器可用，这里选用依赖更少的“ECANVCI 库文件”一套。
- 设备需要按厂商说明预装 USB 驱动。
- 常见接口族：`OpenDevice`、`InitCAN`、`StartCAN`、`Transmit`、`Receive`、
  `GetReceiveNum`、`ReadErrInfo`、`CloseDevice` 等。
- 已知行为（详见仓库 `references/hard-constraints.md`）：
  - 没有枚举 API，只能按“类型 × 索引”盲开；
  - 板卡信息里的通道数字段可能无效（实测返回 50）；
  - 接收失败返回 `0xFFFFFFFF`，等待参数默认可能阻塞；
  - 时间戳单位 µs、32 位、约 71.6 分钟回绕，基准为设备上电；
  - 运行库不是线程安全的，跨通道也必须串行调用。

## 许可

厂商专有运行库。随包或再分发前请确认厂商许可条款；本目录仅用于接入参考。
