# AI Agent 联合调试指南

本工具内置了语义化 DOM 数据接口，可以与 AI Agent（Claude、Cursor、Copilot 等）配合使用，实现串口数据的自动读取和分析。

---

## 工作原理

```
串口设备 → serial-monitor.html → AI Agent 读取页面数据 → 分析诊断
```

串口助手页面的每条数据都带有 `data-*` 属性标记，AI Agent 可通过浏览器自动化工具直接读取，无需手动复制粘贴。

---

## 方法一：Claude Code + agent-browser（推荐）

如果你使用 VSCode + Claude Code 扩展：

### 1. 启动串口助手

在 Chrome/Edge 中打开 `serial-monitor.html`，连接设备确认数据正常。

### 2. 让 AI Agent 读取数据

在 Claude Code 对话中直接发送指令：

```
读取 http://localhost:8080/serial-monitor.html 页面上的最近 30 条串口数据，帮我分析
```

Agent 会通过浏览器自动化打开页面，执行以下查询获取数据：

```javascript
// 获取最近 N 条接收数据
document.querySelectorAll('.receive-line[data-ascii]')

// 每条数据行包含:
//   data-timestamp  → 时间戳
//   data-ascii      → ASCII 内容
//   data-hex        → HEX 内容
```

### 3. 典型调试对话

**调试 PID 参数：**
```
我的设备正在通过串口输出 PID 数据，格式是:
[PID] Err=-0.53 P=-0.39 I=12.10 D=0.00 Out=11.7

请打开串口助手页面，读取最近 50 条数据，分析:
1. 是否存在超调
2. 稳态误差是多少
3. 建议如何调整 Kp/Ki/Kd
```

**调试传感器数据：**
```
MPU6050 输出 Ax=0.12 Ay=-9.81 Az=0.05，串口助手已绑定变量。
请读取趋势图最新值，判断传感器是否正常工作。
```

**调试通信协议：**
```
切换到 HEX 模式，读取最近 20 帧原始数据，帮我检查 CRC 校验是否正确。
```

---

## 方法二：Chrome DevTools Console

直接在浏览器中按 F12，在 Console 中执行：

### 读取最新数据
```javascript
// 最新 10 条 ASCII 数据
Array.from(document.querySelectorAll('.receive-line[data-ascii]'))
  .slice(-10)
  .map(l => ({ time: new Date(+l.dataset.timestamp).toLocaleTimeString(), data: l.dataset.ascii }))
```

### 导出全部数据为 JSON
```javascript
// 全部接收数据
JSON.stringify(
  Array.from(document.querySelectorAll('.receive-line[data-ascii]'))
    .map(l => ({ ts: +l.dataset.timestamp, ascii: l.dataset.ascii, hex: l.dataset.hex })),
  null, 2
)
// 复制输出的 JSON，粘贴给 AI Agent 分析
```

### 检查串口状态
```javascript
document.querySelector('[data-port-status]').dataset.portStatus
// 返回 "connected" 或 "disconnected"
```

### 读取变量追踪数据
```javascript
// 隐藏数据存储区包含所有追踪变量的结构化数据
document.querySelector('#ai-data-store').innerHTML
```

---

## 方法三：Playwright / Puppeteer 脚本

将数据读取集成到自动化测试流程：

```javascript
// Playwright 示例
const { chromium } = require('playwright');

async function readSerialData() {
  const browser = await chromium.launch();
  const page = await browser.newPage();
  await page.goto('file:///C:/work_app/UART/serial-monitor.html');

  // 等待数据积累
  await page.waitForTimeout(5000);

  // 读取数据
  const data = await page.evaluate(() => {
    return Array.from(document.querySelectorAll('.receive-line[data-ascii]'))
      .map(el => ({
        timestamp: Number(el.dataset.timestamp),
        ascii: el.dataset.ascii,
        hex: el.dataset.hex,
      }));
  });

  console.log(`读取到 ${data.length} 条数据`);
  await browser.close();
  return data;
}
```

---

## 数据接口速查

| 目标数据 | 查询方式 |
|----------|----------|
| 最新 N 条接收数据 | `document.querySelectorAll('.receive-line[data-ascii]')` |
| 单条数据时间戳 | `el.dataset.timestamp` |
| 单条数据 ASCII | `el.dataset.ascii` |
| 单条数据 HEX | `el.dataset.hex` |
| 串口连接状态 | `document.querySelector('[data-port-status]').dataset.portStatus` |
| 收发字节统计 | `document.querySelector('[data-stats]').dataset` |
| 趋势图变量值 | `document.querySelectorAll('.trend-latest[data-varname]')` |
| 完整数据存储 | `document.querySelector('#ai-data-store')` |
