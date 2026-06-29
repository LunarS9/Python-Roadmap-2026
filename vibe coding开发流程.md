# 软件开发流程

## 流程
1. 写全局 AGENTS.md（`~/.codex/AGENTS.md`）
2. 写项目 AGENTS.md（项目根目录 `/AGENTS.md`）
3. 产出 PROJECT.md（项目蓝图）
4. 拆分 TODO.md（任务清单）
5. 逐模块开发（套用 Prompt 模板）
6. 完成一个模块 → 更新 AGENTS.md 状态 + TODO.md 打勾
7. 全部完成 → 更新 README.md → 提交 GitHub

~/.codex/AGENTS.md（全局，复制一次即可，两种项目都适用）
markdown# 全局工作约定

## 代码风格
- 做微信小程序：优先用原生 Component，不引入额外框架（Taro/uni-app除外）
- 做电脑桌面应用（Windows/Mac）：优先用 Electron（用网页技术做桌面软件，跨平台最简单，不会另外说明就默认用这个）
- JS用ES6+，函数命名camelCase，文件命名kebab-case
- 注释用中文

## 协作方式
- 只改我指定的文件，不要顺手重构
- 改已有文件只给diff，不要整文件重新粘贴
- 多文件改动前先列出将修改的文件清单，等我确认再开始
- 不确定的地方直接问我，不要自己猜

## 测试与提交
- 改完给出验证步骤
- 不自动 git commit

/AGENTS.md（项目级，每个项目放一份，先确定项目类型）
markdown# 项目约定：[项目名]

## 项目类型（二选一）
- [ ] 微信小程序
- [ ] 电脑桌面应用（Windows/Mac）

## 技术栈
- 微信小程序：[原生 / Taro / uni-app]
- 桌面应用：[Electron / Tauri]

## 目录结构 - 微信小程序
miniprogram/
├── pages/
├── components/
├── utils/
├── services/
└── app.js / app.json / app.wxss

## 目录结构 - 桌面应用（Electron）
src/
├── main/        # 控制窗口、系统功能
├── renderer/    # 界面（你实际看到的页面）
├── utils/
└── package.json

## 命名规则
- 小程序页面：pages/模块名/模块名.js/.wxml/.wxss/.json
- 桌面应用页面：renderer/模块名/模块名.html + .js
- 接口：services/xxx.js

## 模块状态
- [ ] 首页
- [ ] 详情页
- [ ] 我的/设置页

## 禁止事项
- 不引入未声明的第三方库
- 不改主入口文件结构，除非明确要求

PROJECT.md 生成 Prompt
我要开发一个[微信小程序 / 电脑桌面应用（Windows和Mac都要用 / 只要Windows / 只要Mac）]：[一句话描述功能]

产出 PROJECT.md，包含：
1. 功能模块拆分
2. 页面清单（路径+用途+交互）
3. 数据模型（字段定义）
4. 接口清单（函数名+输入+输出）
5. 开发优先级
6. 如果是桌面应用，说明用什么工具做（Electron还是其他），为什么

先不写代码。

TODO.md 模板
markdown# 任务清单

## P0
- [ ] 首页：核心功能汇总
- [ ] 详情页：查看+编辑+删除
- [ ] 新增页：添加数据

## P1
- [ ] 我的/设置页

## P2
- [ ] 导出功能

单模块开发 Prompt 模板
任务：[一句话，范围小]
涉及文件：[具体路径]
背景：
- 已有接口：[函数名] —— 位于 [路径]
约束：
- 只改上述文件
- 最小 diff，已有代码不重新粘贴
验收标准：
- [具体效果]
追问模板（不重复背景）
刚才的[功能]再加[变化点]，其他不变。
收尾 Prompt
[模块名]已验收，请把 AGENTS.md 里该模块状态改成已完成，
并补充新约定：[如有]
