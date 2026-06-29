# 小程序开发流程

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
