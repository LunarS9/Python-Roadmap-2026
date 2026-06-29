# 小程序开发流程
## 第1步 - 开始前,跟Codex说一次（建立全局习惯，只需要做一次）：
请帮我在 ~/.codex/AGENTS.md 里写入以下内容：
微信小程序优先用原生 Component，不引入额外框架。
JS用ES6+，函数命名camelCase，文件命名kebab-case。
改已有文件时只给diff，不要重新粘贴整个文件。
多文件改动前先列出将修改的文件清单，等我确认再开始。
不确定的地方直接问我，不要自己猜。
## 第2步 - 进入项目文件夹后，第一句话：
我要做一个微信小程序，功能是个人记账（记录每日支出、按月统计、查看历史记录）。
请帮我生成一份 PROJECT.md，包含：功能模块拆分、页面清单、数据字段、接口规划、开发优先级。先不要写代码。
## 第3步 - Codex给你PROJECT.md之后，下一句：
根据 PROJECT.md，帮我生成一份 TODO.md 任务清单，按优先级排好。
## 第4步 - 正式开始写第一个页面，照这个格式说：
现在只做首页 index 这一个页面：
- 功能：显示本月支出总额 + 最近5条记录列表
- 文件：pages/index/index.js、index.wxml、index.wxss
- 不要动其他文件
## 第5步 - Codex写完之后，你测试有问题/要加功能，直接说变化点，不用重复背景：
首页加一个下拉刷新功能，其他不变。
## 第6步 - 这个页面做完后，确认状态：
index首页已经测试通过，请在AGENTS.md里把它标记为已完成。
## 第7步 - 重复第4-6步，换成下一个页面（比如详情页）：
现在做详情页 detail：
- 功能：显示一条记录的详情，可以编辑/删除
- 文件：pages/detail/detail.js、detail.wxml、detail.wxss
- 复用已有的 services/record.js 里的接口
- 不要动其他文件
全部做完后：
所有页面都已完成，帮我更新README.md，写清楚这个项目是做什么的、怎么运行。

## 流程
1. 写全局 AGENTS.md（`~/.codex/AGENTS.md`）
2. 写项目 AGENTS.md（项目根目录 `/AGENTS.md`）
3. 产出 PROJECT.md（项目蓝图）
4. 拆分 TODO.md（任务清单）
5. 逐模块开发（套用 Prompt 模板）
6. 完成一个模块 → 更新 AGENTS.md 状态 + TODO.md 打勾
7. 全部完成 → 更新 README.md → 提交 GitHub

---

## `~/.codex/AGENTS.md`（全局，复制一次即可）

```markdown
# 全局工作约定

## 代码风格
- 微信小程序优先用原生 Component，不引入额外框架
- JS ES6+，函数 camelCase，文件 kebab-case
- 注释用中文

## 协作方式
- 只改我指定的文件，不要顺手重构
- 改已有文件只给 diff，不要整文件重新粘贴
- 多文件改动前先列清单确认
- 不确定就问，不要猜

## 测试与提交
- 改完给出验证步骤
- 不自动 git commit
```

---

## `/AGENTS.md`（项目级，每个小程序项目放一份）

```markdown
# 项目约定：[项目名]

## 技术栈
- [原生小程序 / Taro / uni-app]
- 状态管理：[globalData / store]
- 网络请求统一走 services/

## 目录结构
miniprogram/
├── pages/
├── components/
├── utils/
├── services/
└── app.js / app.json / app.wxss

## 命名规则
- 页面：pages/模块名/模块名.js/.wxml/.wxss/.json
- 接口：services/xxx.js

## 模块状态
- [ ] index 首页
- [ ] detail 详情页
- [ ] mine 我的页面

## 禁止事项
- 不改 app.json 页面顺序
- 不引入未声明的第三方库
```

---

## PROJECT.md 生成 Prompt

```
我要开发一个微信小程序：[一句话描述]

产出 PROJECT.md，包含：
1. 功能模块拆分
2. 页面清单（路径+用途+交互）
3. 数据模型（字段定义）
4. 接口清单（函数名+输入+输出）
5. 开发优先级

先不写代码。
```

---

## TODO.md 模板

```markdown
# 任务清单

## P0
- [ ] index：首页汇总
- [ ] detail：详情+编辑+删除
- [ ] add：新增记录

## P1
- [ ] mine：我的页面

## P2
- [ ] 导出报表
```

---

## 单模块开发 Prompt 模板

```
任务：[一句话，范围小]
涉及文件：[具体路径]
背景：
- 已有接口：[函数名] —— 位于 [路径]
约束：
- 只改上述文件
- 最小 diff，已有代码不重新粘贴
验收标准：
- [具体效果]
```

## 追问模板（不重复背景）

```
刚才的[功能]再加[变化点]，其他不变。
```

## 收尾 Prompt

```
[模块名]已验收，请把 AGENTS.md 里该模块状态改成已完成，
并补充新约定：[如有]
```
