# ZHCX（创芯）ControlCAN 运行库

## 来源

厂商随附资料包中的“二次开发库文件”（2025-06-24 版）。未包含产品手册、
规格书、示例工程与截图。

## 文件

| 文件 | 大小 | 说明 |
| --- | ---: | --- |
| `ControlCAN.h` | 3,567 B | 接口原型、结构体、常量（VC 版） |
| `x64/ControlCAN.dll` | 2,683,392 B | 64 位运行库，版本 9.1.2.1 |
| `x64/ControlCAN.lib` | 6,956 B | 64 位导入库（C/C++ 链接用） |
| `win32/ControlCAN.dll` | 2,065,408 B | 32 位运行库，版本 9.1.2.1 |
| `win32/ControlCAN.lib` | 7,312 B | 32 位导入库（C/C++ 链接用） |

## 使用要点

- 按目标进程位数选择 `x64/` 或 `win32/`，不要混用。
- 运行库本身不依赖同目录 DLL；设备需要预装厂商 USB 驱动。
- 常见接口族：`VCI_OpenDevice`、`VCI_InitCAN`、`VCI_StartCAN`、
  `VCI_Transmit`、`VCI_Receive`、`VCI_CloseDevice` 等。
- 已知行为（详见仓库 `references/hard-constraints.md`）：
  - `SendType=0` 在无 ACK 时会持续重发，造成洪泛；
  - `DataLen>8` 会发出越界内存数据；
  - 只监听通道发送会返回“成功”但实际不上总线，并可能闩死设备；
  - 时间戳单位 0.1 ms，32 位，约 4.97 天回绕，基准为设备上电。

## 许可

厂商专有运行库。随包或再分发前请确认厂商许可条款；本目录仅用于接入参考。
