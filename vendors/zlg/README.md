# ZLG（周立功/致远）zlgcan 运行库

## 来源

本机 ZLG `zlgcan` SDK 包（`zlgcan_x64` / `zlgcan_x86`），随包接口手册为
《ZLGCAN 接口函数使用手册 V1.21》。未包含手册 PDF、示例工程与截图。

## 文件

| 文件 | 大小 | 说明 |
| --- | ---: | --- |
| `zlgcan.h` | 50,738 B | 主头文件（x86/x64 相同） |
| `canframe.h` | 5,609 B | 帧结构定义，被 `zlgcan.h` 引用 |
| `config.h` | 2,899 B | 配置/常量定义，被 `zlgcan.h` 引用 |
| `typedef.h` | 275 B | 基础类型定义，被 `zlgcan.h` 引用 |
| `x64/zlgcan.dll` | 310,272 B | 64 位运行库 |
| `x64/zlgcan.lib` | 13,730 B | 64 位导入库（C/C++ 链接用） |
| `win32/zlgcan.dll` | 231,936 B | 32 位运行库 |
| `win32/zlgcan.lib` | 14,728 B | 32 位导入库（C/C++ 链接用） |
| `x64/kerneldlls/` | 179 个文件 / 29,531,934 B | 设备内核驱动与描述文件（64 位，运行期必需） |
| `win32/kerneldlls/` | 178 个文件 / 23,226,941 B | 设备内核驱动与描述文件（32 位，运行期必需） |
| `LICENSE.txt` | 2,792 B | zlgcan 许可证原文 |

## 版本

- 接口手册：V1.21。
- `zlgcan.dll` 未写入文件版本号；许可证标注版权年份 2012–2025。
- 头文件本身不含 SDK 版本宏，升级时请以厂商包内 `zlgcan.h` 与 DLL 成对替换。

## 依赖

- **VC++ 2013 运行库**：PE 导入表为 `KERNEL32.dll`、`MSVCR120.dll`、`MSVCP120.dll`；
  目标机器需安装对应位数的 VC++ 2013 运行库，或随包这两个 DLL。
- **`kerneldlls/` 目录**：`zlgcan.dll` 运行期从同目录（或 DLL 搜索路径）加载设备内核驱动。
  缺少该目录时 `LoadLibrary` 仍会成功，但枚举/打开设备会失败。
  **已随仓库收录**：64 位位于 `x64/kerneldlls/`（179 个文件 / 29,531,934 B），
  32 位位于 `win32/kerneldlls/`（178 个文件 / 23,226,941 B）。
  部署时必须与对应位数的 `zlgcan.dll` 整目录同放，不能只拷主 DLL。
- 设备本身仍需预装 ZLG USB 驱动。

## 已知行为与接入提示

- zlgcan 同时覆盖经典 CAN 与 CAN FD（取决于具体型号）；能力必须以设备信息为准，
  不要按 DLL 名称推断。
- 时间戳的单位、基准与回绕周期未在本仓库实测，接入时必须用固定周期报文反推，
  不要直接套用其它厂商的结论（见 `references/hard-constraints.md` 第 5、6 条）。
- 线程安全未在头文件中声明；在自测确认前按“每设备一把锁”串行调用。
- **同名 DLL 风险**：ZLG 旧版 `ControlCAN.dll` 与创芯 ZHCX 的 `ControlCAN.dll` 同名，
  且后者兼容前者的导出名。本仓库不收录旧 ControlCAN，统一使用 zlgcan 接口；
  若目标机器同时存在两者，必须按绝对路径加载并校验文件版本/厂商，禁止按文件名加载。

## 许可

见 `LICENSE.txt`（zlgcan 原许可证）。再分发二进制时需在文档或附属材料中保留
版权声明、条件列表与免责声明。
