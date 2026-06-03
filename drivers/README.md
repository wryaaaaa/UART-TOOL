# 串口驱动下载指引

此文件夹用于存放 USB 转串口驱动。由于驱动为芯片厂商闭源软件，请自行下载。

## 常见芯片驱动下载

| 芯片 | 官方下载 | 备用链接 |
|------|----------|----------|
| CH340 / CH341 | [WCH 官网](http://www.wch.cn/downloads/CH341SER_EXE.html) | 百度搜索 "CH340驱动" |
| CP210x | [Silicon Labs 官网](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) | 百度搜索 "CP2102驱动" |
| FT232 | [FTDI 官网](https://ftdichip.com/drivers/vcp-drivers/) | 百度搜索 "FT232驱动" |

## 安装后验证

1. 打开设备管理器（Win+X → 设备管理器）
2. 插上 USB 转串口设备
3. 展开「端口 (COM 和 LPT)」，应看到 `USB-SERIAL CH340 (COMx)`
4. 没有 → 驱动未安装成功，换个版本重试

## 常见问题

**Q: 安装驱动后还是看不到 COM 口？**
A: 换个 USB 口重新插拔，或重启电脑。部分 CH340 芯片（特别是 CH340G）需要外接晶振，检查硬件。

**Q: Windows 提示"驱动签名无效"？**
A: Win10/11 可能需要禁用驱动签名强制：设置 → 更新和安全 → 恢复 → 高级启动 → 重启 → 疑难解答 → 高级选项 → 启动设置 → 重启 → 按 7 禁用驱动程序强制签名。
