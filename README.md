# 🔌 UART 串口调试助手

纯浏览器端专业串口调试工具 — **单文件 HTML，零外部依赖，双击即用**。

支持 2D 趋势图 + 3D 空间可视化，专为嵌入式开发调试设计。

---

## ✨ 功能特性

### 串口通信
- [x] Web Serial API，Chrome/Edge 原生支持
- [x] 波特率 / 数据位 / 停止位 / 校验位 / 流控 全配置
- [x] 串口热插拔自动检测
- [x] 多串口同时连接（标签页模式）

### 数据收发
- [x] 接收区 ASCII / HEX 双模式切换
- [x] 毫秒级时间戳
- [x] 自动滚动 / 暂停 / 自动换行
- [x] 发送区 ASCII / HEX 输入切换
- [x] 手动发送 + 定时发送（可配间隔）
- [x] 发送历史记录（点击复用）
- [x] 清空接收区

### 变量解析 & 趋势图
- [x] 多格式自动检测：`key=value` | `key:value` | `{key:value}`
- [x] 自定义变量名绑定
- [x] 纯 Canvas 实时折线图（暗黑主题，零依赖）
- [x] 多变量多线同时显示
- [x] **滚轮缩放 / 拖拽平移 / 双击重置**
- [x] 右键菜单快捷添加变量
- [x] 数据点上限自动滚动（1000 点）

### 三维空间调试 🧊
- [x] 2D / 3D 一键切换
- [x] 三色坐标轴 + 网格参考面
- [x] 姿态模式（旋转/倾斜）+ 位置模式（位移）
- [x] **立方体、电路板(PCB)、坐标点、轨迹线** 四种模型
- [x] 轨迹渐隐效果（最近 500 点）
- [x] 变量轴自由绑定（X/Y/Z 轴各自指定变量）
- [x] 倍乘功能（适配不同量级的数据）
- [x] 鼠标拖拽旋转 / 右键平移 / 滚轮缩放

### 数据导出
- [x] TXT 纯文本日志（含 HEX dump）
- [x] JSON 结构化数据（含趋势图数据）
- [x] 文件名自动带时间戳

### 布局
- [x] 标签页模式 + 可拖拽分屏
- [x] 分割线拖拽调整大小 / 双击合并
- [x] 趋势图面板可折叠

### 其他
- [x] AI Agent 可读 DOM（`data-*` 语义化属性）
- [x] 键盘快捷键
- [x] 暗黑高级风 UI
- [x] **完全离线可用，无需任何网络**

---

## 🚀 快速开始

### 1. 打开页面
用 **Chrome** 或 **Edge** 浏览器打开 `serial-monitor.html`：
```
双击 serial-monitor.html
```
或者用本地服务器（可选）：
```bash
python -m http.server 8080
# 然后访问 http://localhost:8080/serial-monitor.html
```

> ⚠️ 必须使用 Chromium 内核浏览器（Chrome / Edge / Opera），Firefox 和 Safari 不支持 Web Serial API。

### 2. 连接串口
1. 在顶部工具栏点击「选择串口」
2. 选择你的设备（如 `COM1 (USB 1a86:7523)`）
3. 配置波特率等参数（默认 115200-8-N-1）
4. 点击「**连接**」

### 3. 收发数据
- 接收区实时显示串口数据
- 在底部输入框输入内容，点击「发送」或按 `Enter`
- 切换 ASCII/HEX 模式查看十六进制数据

---

## 📊 趋势图使用

### 配置变量追踪
1. 在趋势图右侧面板输入变量名，如：`Err, Out, SET`
2. 点击 `+` 添加
3. 收到数据后折线图自动更新

### 操作方式
| 操作 | 效果 |
|------|------|
| 滚轮 | 缩放 X 轴（时间） |
| Shift + 滚轮 | 缩放 Y 轴（数值） |
| 拖拽 | 平移视图 |
| 双击图表 | 重置缩放 |
| 右键数据行 | 快捷添加变量 |

### 支持的解析格式
| 格式 | 示例 |
|------|------|
| `key=value` | `Err=-0.53 P=0.34 RP1=2972` |
| `key:value` | `H:15 T:12` |
| `{key:value}` | `{H:15, T:12}` |
| 自动检测 | 同时尝试三种，取匹配最多者 |

---

## 🧊 三维空间调试

### 进入 3D 模式
点击工具栏 `🧊 3D` 按钮切换到三维视图。

### 绑定变量
1. 在右侧配置面板填入变量名：
   ```
   X 轴: Ax    (红色)
   Y 轴: Ay    (绿色)
   Z 轴: Az    (蓝色)
   ```
2. 选择数据模式：
   - **姿态模式**：X/Y/Z 值控制模型旋转角度（适合陀螺仪、角度传感器）
   - **位置模式**：X/Y/Z 值控制模型空间位置（适合加速度计）
3. 选择显示模型：立方体 / 电路板 / 坐标点 / 轨迹线 / 全部
4. 设置倍乘（如数据太小可放大）

### 3D 操作
| 操作 | 效果 |
|------|------|
| 左键拖拽 | 旋转视角 |
| 右键拖拽 | 平移视角 |
| 滚轮 | 缩放远近 |
| 切换 2D/3D | 工具栏按钮 |

### MPU6050 调试示例
传感器输出 `Ax=0.12 Ay=-9.81 Az=0.05 Gx=1.5 Gy=-0.2 Gz=0.1`：

**加速度计**（位置模式）：
- X 绑 `Ax`，Y 绑 `Ay`，Z 绑 `Az` → 选择「轨迹线」→ 看到重力方向大约在 (0, -9.8, 0)

**陀螺仪**（姿态模式）：
- X 绑 `Gx`，Y 绑 `Gy`，Z 绑 `Gz` → 选择「立方体」→ 旋转设备时立方体倾斜

---

## ⌨️ 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Enter` | 发送数据（发送区聚焦时） |
| `Shift + Enter` | 发送区换行 |
| `Ctrl + Shift + C` | 连接 / 断开串口 |
| `Ctrl + L` | 清空接收区 |
| `Esc` | 关闭右键菜单 |

---

## 🤖 AI Agent 接口

页面内置语义化 DOM 数据接口，AI Agent 可通过浏览器自动化工具直接读取：

```javascript
// 获取接收区最新数据
document.querySelectorAll('.receive-line[data-ascii]')

// 获取指定变量最新值
document.querySelector('.trend-latest[data-varname="H"]')

// 获取串口连接状态
document.querySelector('[data-port-status]').dataset.portStatus

// 获取完整数据存储
document.querySelector('#ai-data-store')
```

每条数据行包含 `data-timestamp`、`data-hex`、`data-ascii` 属性，可被外部工具直接解析。

---

## 📁 文件结构

```
UART/
├── serial-monitor.html              ← 主程序（单文件，核心）
├── README.md                        ← 本文件
├── .gitignore
├── screenshots/                     ← 截图文件夹
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-06-03-uart-serial-monitor-design.md  ← 设计文档
```

---

## 🔧 技术栈

| 层 | 技术 |
|----|------|
| 结构 | HTML5 |
| 样式 | CSS3 (CSS 自定义属性暗黑主题) |
| 逻辑 | 原生 ES6+ JavaScript，无框架 |
| 2D 图表 | 纯 Canvas 2D 自绘 |
| 3D 渲染 | 纯 Canvas 2D 软件渲染（透视投影 + 画家算法） |
| 串口 | Web Serial API |
| 字体 | 系统等宽字体栈 |

---

## 📸 截图

### 主界面（接收数据 + 发送区）
![主界面](screenshots/主界面.png)

### 串口连接配置
![连接窗口](screenshots/链接窗口.png)

### 三维空间调试
![3D功能](screenshots/3D功能.png)

### JSON 数据导出
![导出Json文件](screenshots/导出Json文件.png)

---

## 🤖 AI Agent 联合调试指南

### 工作流程

将本工具与 AI Agent（Claude Code、Cursor、Copilot 等）配合使用：

```
串口设备 → serial-monitor.html → AI Agent 读取 DOM 数据 → 分析调试
```

### 步骤 1：打开串口助手

在 Chrome/Edge 中打开 `serial-monitor.html`，连接串口设备。

### 步骤 2：AI Agent 读取数据

AI Agent 通过浏览器自动化工具（如 Chrome DevTools Protocol、Puppeteer、Playwright）读取页面数据：

```javascript
// === 获取接收区最新 10 条数据 ===
const lines = document.querySelectorAll('.receive-line[data-ascii]');
const recent = Array.from(lines).slice(-10).map(l => ({
  timestamp: l.dataset.timestamp,
  ascii: l.dataset.ascii,
  hex: l.dataset.hex
}));
console.table(recent);

// === 获取趋势图变量最新值 ===
const vars = document.querySelectorAll('[data-varname]');
vars.forEach(el => {
  console.log(`${el.dataset.varname}: ${el.dataset.latestValue}`);
});

// === 检查串口连接状态 ===
const status = document.querySelector('[data-port-status]').dataset.portStatus;
console.log('串口状态:', status); // 'connected' | 'disconnected'

// === 导出完整数据存储 ===
const dataStore = document.querySelector('#ai-data-store').innerHTML;
```

### 步骤 3：典型调试场景

**场景 A：PID 参数调优**
```
1. 设备输出: [PID] Err=-0.53 P=-0.39 I=12.10 D=0.00 Out=11.7
2. 串口助手自动解析: Err, P, I, D, Out 五个变量
3. 趋势图绑定 Err, Out → 实时看响应曲线
4. AI Agent 读取数据 → 分析超调量、稳态误差 → 建议 Kp/Ki/Kd 调整
```

**场景 B：传感器异常检测**
```
1. MPU6050 输出: Ax=0.12 Ay=-9.81 Az=0.05
2. 3D 模式绑 Ax/Ay/Az → 姿态模式 → 观察立方体方向
3. AI Agent 读取最近 100 个数据点 → 检测异常抖动 → 定位硬件问题
```

**场景 C：通信协议调试**
```
1. 切换到 HEX 模式查看原始字节
2. AI Agent 读取 data-hex → 解析协议帧 → 校验 CRC → 找出错误帧
```

### 在 Claude Code 中直接使用

如果你用 VSCode + Claude Code 扩展，可以在对话中让我通过 `agent-browser` 工具直接操作页面：

> "打开串口助手，读取最近 20 条接收数据，帮我分析 PID 响应有没有超调"

---

## 📝 License

MIT
