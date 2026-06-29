
# 🫏 驴拉磨 · 最小任务拆解

## Task01 · 项目脚手架初始化

| 项 | 内容 |
|---|---|
| 模块 | 根目录 (package.json, .gitignore, README.md) |
| 职责 | 初始化 npm 项目、配置 Tauri CLI 为 devDependency、编写 gitignore 规则与 README 骨架 |
| 产出 | 项目可执行 npm install，目录结构就位 |
| 依赖 | 无 |

## Task02 · Tauri 后端初始化

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/Cargo.toml` + `src-tauri/build.rs` + `src-tauri/src/main.rs` |
| 职责 | Tauri 2 Rust 项目初始化、最小 main.rs 启动空窗口、声明依赖 |
| 产出 | cargo build 通过，Tauri 空窗口可弹出 |
| 依赖 | Task01 |

## Task03 · 窗口配置：悬浮/透明/无边框

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/tauri.conf.json` |
| 职责 | 配置 alwaysOnTop、transparent、decorations: false、窗口尺寸 280×200、禁用音频相关权限、应用标识符 |
| 产出 | 窗口以无边框透明置顶形态显示 |
| 依赖 | Task02 |

## Task04 · Tauri 权限声明

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/capabilities/default.json` |
| 职责 | 声明最小权限集：窗口控制、托盘交互、AppData 文件读写 |
| 产出 | Tauri 2 权限系统通过校验 |
| 依赖 | Task03 |

## Task05 · 应用图标

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/icons/icon.png` |
| 职责 | 制作像素风驴头图标，生成各平台尺寸（使用 tauri icon 命令） |
| 产出 | 任务栏与托盘可显示图标 |
| 依赖 | Task03 |

## Task06 · Rust 配置模块

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/src/config.rs` |
| 职责 | 定义配置结构体（上下班时间、提醒间隔、透明度、位置）、实现 JSON 序列化/反序列化、读写 AppData 路径 |
| 产出 | Rust 侧可独立完成配置文件的持久化 |
| 依赖 | Task02 |

## Task07 · Rust IPC 命令

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/src/commands.rs` |
| 职责 | 实现 get_config、save_config、start_drag 三个 Tauri command，注册到 main |
| 产出 | 前端可通过 invoke 调用配置读写与窗口拖拽 |
| 依赖 | Task06 |

## Task08 · 系统托盘

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/src/tray.rs` |
| 职责 | 创建系统托盘图标、构建右键菜单（设置上下班时间 / 暂停提醒 / 退出）、绑定事件回调 |
| 产出 | 右键托盘图标可见菜单且退出可关闭应用 |
| 依赖 | Task05, Task07 |

## Task09 · 前端页面骨架

| 项 | 内容 |
|---|---|
| 模块 | `src/index.html` |
| 职责 | 编写 HTML：全屏 `<canvas id="scene">`、叠加层 `<div id="countdown">`、护眼提醒弹窗 `<div id="reminder">`、引入 CSS 与 JS |
| 产出 | 页面可加载，各容器元素就位 |
| 依赖 | Task03 |

## Task10 · 全局样式

| 项 | 内容 |
|---|---|
| 模块 | `src/css/style.css` |
| 职责 | body 透明、canvas 铺满、@font-face 注册像素字体、倒计时数字排版、提醒弹窗居中样式、鼠标穿透/拖拽区域 |
| 产出 | 页面视觉基础完成，透明背景生效 |
| 依赖 | Task09 |

## Task11 · 像素字体资源

| 项 | 内容 |
|---|---|
| 模块 | `src/assets/fonts/pixel.ttf` |
| 职责 | 选取开源像素字体（如 Press Start 2P 或 Zpix），放置到目标路径 |
| 产出 | CSS 可引用该字体渲染倒计时数字 |
| 依赖 | 无 |

## Task12 · 前端配置桥接

| 项 | 内容 |
|---|---|
| 模块 | `src/js/config.js` |
| 职责 | 封装 invoke('get_config') / invoke('save_config')；提供默认值兜底；导出 loadConfig() / saveConfig() |
| 产出 | 前端可异步获取与保存用户配置 |
| 依赖 | Task07, Task09 |

## Task13 · 全局状态机

| 项 | 内容 |
|---|---|
| 模块 | `src/js/state.js` |
| 职责 | 定义三态枚举（WORKING / REMINDING / OFF_WORK）；实现 transition()、current() 方法；发布状态变更事件 |
| 产出 | 各模块可订阅状态切换做出响应 |
| 依赖 | Task09 |

## Task14 · 倒计时引擎

| 项 | 内容 |
|---|---|
| 模块 | `src/js/countdown.js` |
| 职责 | 从 config 取下班时间；每秒计算剩余 HH:MM；归零时通知 state 切换到 OFF_WORK；提供 getRemainingSeconds() 供渲染层调用 |
| 产出 | 倒计时逻辑可独立运行并驱动状态切换 |
| 依赖 | Task12, Task13 |

## Task15 · 护眼提醒模块

| 项 | 内容 |
|---|---|
| 模块 | `src/js/reminder.js` |
| 职责 | setInterval(30min) 定时器；触发时切换状态到 REMINDING；显示提醒弹窗 DOM；用户点击确认后恢复 WORKING；全程纯视觉无音频 |
| 产出 | 每 30 分钟弹出静默护眼提示 |
| 依赖 | Task13, Task10 |

## Task16 · 精灵表加载器

| 项 | 内容 |
|---|---|
| 模块 | `src/js/sprites.js` |
| 职责 | 通用工具：加载图片 → 按帧宽/帧高切割 → 返回帧数组；缓存已加载资源 |
| 产出 | 其他动画模块可调用 loadSprite(path, frameW, frameH) 获取帧 |
| 依赖 | Task09 |

## Task17 · 驴行走精灵资源

| 项 | 内容 |
|---|---|
| 模块 | `src/assets/sprites/donkey-walk.png` |
| 职责 | 绘制 32×32 像素风格驴行走 spritesheet（8 帧横排，总 256×32） |
| 产出 | 行走动画帧可被 sprites.js 加载 |
| 依赖 | 无 |

## Task18 · 驴眨眼精灵资源

| 项 | 内容 |
|---|---|
| 模块 | `src/assets/sprites/donkey-blink.png` |
| 职责 | 绘制 32×32 驴停下眨眼 spritesheet（4 帧横排，用于提醒态） |
| 产出 | 提醒态动画帧就位 |
| 依赖 | 无 |

## Task19 · 驴庆祝精灵资源

| 项 | 内容 |
|---|---|
| 模块 | `src/assets/sprites/donkey-celebrate.png` |
| 职责 | 绘制 32×32 驴撒欢 spritesheet（6 帧横排，用于下班态） |
| 产出 | 下班庆祝动画帧就位 |
| 依赖 | 无 |

## Task20 · 磨盘精灵资源

| 项 | 内容 |
|---|---|
| 模块 | `src/assets/sprites/mill.png` |
| 职责 | 绘制 64×64 磨盘旋转 spritesheet（6 帧横排，俯视视角） |
| 产出 | 磨盘动画帧可被加载 |
| 依赖 | 无 |

## Task21 · 扬尘粒子资源

| 项 | 内容 |
|---|---|
| 模块 | `src/assets/sprites/dust.png` |
| 职责 | 绘制 8×8 灰尘粒子 spritesheet（4 帧横排） |
| 产出 | 粒子动画帧就位 |
| 依赖 | 无 |

## Task22 · 磨盘动画模块

| 项 | 内容 |
|---|---|
| 模块 | `src/js/mill.js` |
| 职责 | Mill 类：加载磨盘帧、以画布中心为锚点旋转绘制、转速从 countdown 取值（剩余时间越少转越快）；对外暴露 update(dt) / draw(ctx) |
| 产出 | 磨盘可在 Canvas 上独立旋转渲染 |
| 依赖 | Task16, Task20, Task14 |

## Task23 · 驴动画模块

| 项 | 内容 |
|---|---|
| 模块 | `src/js/donkey.js` |
| 职责 | Donkey 类：加载三套帧（走/眨眼/庆祝）；椭圆路径绕磨行走；监听 state 切换对应帧集；对外暴露 update(dt) / draw(ctx) |
| 产出 | 驴可在 Canvas 上绕磨行走并响应状态变化 |
| 依赖 | Task16, Task17, Task18, Task19, Task13 |

## Task24 · 粒子系统

| 项 | 内容 |
|---|---|
| 模块 | `src/js/particles.js` |
| 职责 | Particles 类：管理粒子池；WORKING 态在驴脚下喷尘；OFF_WORK 态全屏撒花；控制生命周期与回收 |
| 产出 | 粒子效果可叠加渲染 |
| 依赖 | Task16, Task21, Task13 |

## Task25 · Canvas 渲染主循环

| 项 | 内容 |
|---|---|
| 模块 | `src/js/renderer.js` |
| 职责 | 获取 Canvas context；requestAnimationFrame 循环：清空 → mill.draw() → donkey.draw() → particles.draw() → 绘制倒计时文字；计算 deltaTime 传入各模块 |
| 产出 | 所有视觉元素合成到一帧画面 |
| 依赖 | Task22, Task23, Task24, Task14 |

## Task26 · 前端入口整合

| 项 | 内容 |
|---|---|
| 模块 | `src/js/main.js` |
| 职责 | DOMContentLoaded 后：loadConfig() → 初始化 state/countdown/reminder/renderer → 启动主循环；绑定窗口拖拽事件调用 start_drag |
| 产出 | 整个前端完整运转 |
| 依赖 | Task12, Task13, Task14, Task15, Task25 |

## Task27 · Rust main.rs 整合注册

| 项 | 内容 |
|---|---|
| 模块 | `src-tauri/src/main.rs` |
| 职责 | 将 commands.rs、tray.rs、config.rs 模块注册到 Tauri Builder；确保编译通过并完整启动 |
| 产出 | 后端所有模块挂载完毕，应用可 cargo tauri dev 运行 |
| 依赖 | Task07, Task08 |

## Task28 · 端到端联调验证

| 项 | 内容 |
|---|---|
| 模块 | 全项目（只验证，不新增文件） |
| 职责 | npm run tauri dev 启动验证：窗口悬浮透明 ✓、倒计时走字 ✓、驴绕磨走 ✓、30min 提醒弹出 ✓、托盘菜单可用 ✓、全程无音频 ✓ |
| 产出 | 确认所有功能闭环 |
| 依赖 | Task26, Task27 |

---

## 依赖关系总览

```text
无依赖（可并行）
├── Task01   项目脚手架
├── Task11   像素字体资源
├── Task17   驴行走帧
├── Task18   驴眨眼帧
├── Task19   驴庆祝帧
├── Task20   磨盘帧
└── Task21   扬尘粒子帧

Task01
└── Task02  Tauri Rust 初始化
    └── Task03  窗口配置
        ├── Task04  权限声明
        ├── Task05  应用图标
        └── Task09  前端 HTML 骨架
            ├── Task10  CSS 样式
            ├── Task16  精灵加载器
            └── Task13  状态机
    └── Task06  Rust 配置模块
        └── Task07  IPC 命令
            ├── Task08  系统托盘 ←── Task05
            └── Task12  前端配置桥接 ←── Task09

Task13  状态机
├── Task14  倒计时引擎 ←── Task12
├── Task15  护眼提醒 ←── Task10
├── Task23  驴动画 ←── Task16, Task17, Task18, Task19
└── Task24  粒子系统 ←── Task16, Task21

Task14  倒计时引擎
└── Task22  磨盘动画 ←── Task16, Task20

Task22 + Task23 + Task24 + Task14
└── Task25  Canvas 渲染主循环

Task12 + Task13 + Task14 + Task15 + Task25
└── Task26  前端入口整合

Task07 + Task08
└── Task27  Rust 整合注册

Task26 + Task27
└── Task28  端到端联调
```

---

## 静音约束检查点

| 检查位置 | 确认项 |
|---|---|
| `tauri.conf.json` | 无 audio 相关 capability |
| `src/assets/` | 无 sounds/ 目录、无任何 .wav/.mp3 文件 |
| `reminder.js` | 提醒仅操作 DOM 可见性，无 AudioContext / new Audio() |
| `index.html` | 无 `<audio>` 标签 |

---

> 以上 28 个任务为本项目完整开发序列，后续编码逐 Task 推进，不跨模块修改。
