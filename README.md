# lol-guide

Claude Code 自定义 Skill，查询英雄联盟对线攻略并自动搜索对线视频。

## 功能

- 查询 OP.GG 对线数据（击杀率、胜率、KDA、出装、符文）
- 自动询问是否搜索对线视频
- 支持 B站、YouTube、抖音三个平台

## 安装

将 `.claude/skills/lol-guide/SKILL.md` 放到你的项目目录即可：

```
your-project/
  .claude/
    skills/
      lol-guide/
        SKILL.md
```

## 使用

在 Claude Code 中输入：

```
/lol-guide 剑魔 vs 杰斯 上路
/lol-guide 蒙多打剑姬
/lol-guide 青钢影辅助出装
```

## 输出示例

```
## 剑魔 vs 杰斯 对线数据

| 数据 | 剑魔 | 杰斯 |
|------|------|------|
| 对线击杀率 | 55.90% | 44.10% |
| 游戏胜率 | 46.23% | 53.77% |
| KDA | 1.73 : 1 | 1.73 : 1 |
| 一塔时间 | 17'1" | 16'11" |

（出装符文、打法建议...）

---
要看对线视频吗？
```

## 数据来源

- [OP.GG](https://www.op.gg) — 翡翠+段位数据
- 视频平台：B站、YouTube、抖音

## 前置条件

- [BrowserAct CLI](https://www.browseract.com) — 用于抓取网页数据
- Python 3.12+、uv 包管理器
- 代理（可选，用于访问 OP.GG）

## License

MIT
