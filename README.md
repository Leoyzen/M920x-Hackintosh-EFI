# M920x-Hackintosh-EFI
Hackintosh Opencore EFIs for M920x

# 机器配置

* Lenovo M920x
* CPU: i7-9700T
* Motherboard: Q370
* BIOS: [M1UKT50A](https://pcsupport.lenovo.com/jp/en/products/desktops-and-all-in-ones/thinkcentre-m-series-desktops/thinkcentre-m920x/downloads/ds503907)
* GPU: UHD 630/AMD Radeon RX560 4G
* Mem: Lenovo 8G*2
* NVMe: SN750
* Wifi/Bluetooth: 1820A (08PKF4)

# 黑苹果状态
![System Info](https://github.com/Leoyzen/M920x-Hackintosh-EFI/raw/master/docs/sysinfo.png)
* Opencore：0.5.6 offcial
* MacOS: Catalina 10.15.3
* CFG Lock：已解锁
* 变频：理论上应该是正常的
* 硬解
  * H264：正常
  * H265：正常
  * 4k HDR 解码：QuickTime 播放正常，集显、核显都有负载；IINA 有卡顿，原因未知
* 睡眠：不正常，会睡死，原因未知
* 显卡
  * RX560
    * AGPM：正常
    * Metal：正常
    * mini-dp*4 接口：正常
  * UHD 630
    * 接口：主板两个接口(HDMI/DP)不可用 
* Wifi/蓝牙：正常
* Handoff：正常
* 内置音频：正常
* USB：已映射，正常显示 5Gbps 和电流
* NVMe：已内建

# 已知问题
已知问题和讨论见：[issues](https://github.com/Leoyzen/M920x-Hackintosh-EFI/issues)

# 使用说明
## 主板配置

1. 关闭安全模式
2. 升级 BIOS 到最新（下面的 DVMT 修复基于最新的 BIOS）
3. 使用内置的 EFI 中的 setup_var 关闭 CFG_Lock
    * 如果无法关闭/不会操作 setup_vars，需要勾选上 opencore 中以下的选项。可能会影响变频
        1. Kernel-Quirks-AppleCpuPmCfgLock: yes
        2. Kernel-Quirks-AppleXcpmCfgLock: yes


## EFI 配置
1. 下载 zip 包并解压
2. 修改 config.plist，添加 SMBios 信息（有集显选择 iMac19,1，无集显选择 macmini8,1）
3. 重启

## 如何关闭 CFG_Lock(谨慎操作！)
Q370 中没有关闭 CFG_Lock 的选项，所以需要按照修复 DVMT 的方式关闭锁
请确定你的 BIOS 版本是 M1UJY50USA!

1. 下载 EFI 配置
2. 修改 config.plist，打开 show_picker 选项
3. 重启进入 opencore 选择页面
4. 选择 setup_var 进入 EFI 命令行
5. 关闭 CFG_Lock：`setup_var 0x721 0x00`
6. 修复 DVMT
    1. 将 Pre-Allocate Mem 修改为 64M（最大可选项）：`setup_var 0xA44 0x2`
    2. 将 Allocate Mem 修改为 Max：`setup_var 0xA45 0x3`
7. 重启，在 Intel Power Widgets 中看 CFG_Lock 是否已关闭

详细信息参照的 BIOS 选项参照：[docs/m1ujy50usa.txt](https://github.com/Leoyzen/M920x-Hackintosh-EFI/raw/master/docs/m1ujy50usa.txt)
