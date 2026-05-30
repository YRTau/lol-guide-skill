---
name: lol-guide
description: "查英雄联盟攻略：对线打法、出装符文、英雄克制。当用户提到英雄名字+对线/出装/符文/怎么打/克制时自动触发。"
argument-hint: "英雄A vs 英雄B 上路"
---

# 英雄联盟对线攻略

## 数据来源

通过 BrowserAct 浏览器访问 OP.GG，必须使用代理：
```
--custom-proxy "http://127.0.0.1:7890"
```

浏览器 ID：98424110926465315（stealth，带代理）
浏览器 ID：direct_local_98874150816317472（chrome-direct，抖音等需要登录的站）

## 查询流程

用户输入格式：`英雄A vs 英雄B 位置`，例如 `武器 vs 铁男 上路`

### 第1步：查对线数据

用 stealth 浏览器打开 OP.GG 对线页面：
```
browser-act --session lol browser open 98424110926465315 "https://op.gg/lol/champions/<英雄A>/counters/top?target_champion=<英雄B>"
```

**必须提取的数据（缺一不可）：**
1. **对线击杀率（Lane Kill Rate）** —— 最重要
2. 游戏胜率（Win Rate）
3. KDA
4. 参团率（Kill Participation）
5. 一塔时间（First Tower）

### 第2步：查出装符文

导航到 build 页面：
```
browser-act --session lol navigate "https://op.gg/lol/champions/<英雄A>/build/top"
```

**必须提取的数据：**
1. 符文（主系 + 副系，附使用率和胜率）
2. 召唤师技能
3. 技能加点顺序
4. 出门装
5. 核心三件套（附使用率和胜率）
6. 第四件选择

### 第3步：关闭 session

```
browser-act session close lol
```

### 第4步：输出数据后，自动询问是否要观看对线视频

**在输出完所有攻略数据后，必须追加以下内容：**

```markdown
---
要看对线视频吗？搜 B站、YouTube、抖音（需要占用你的 Chrome）。
```

然后使用 AskUserQuestion 工具询问用户：

```json
{
  "questions": [{
    "question": "要搜对线视频吗？",
    "header": "搜视频",
    "options": [
      {"label": "B站 + YouTube", "description": "不需要登录，直接搜"},
      {"label": "全部平台（含抖音）", "description": "会占用你的 Chrome，搜完自动释放"},
      {"label": "不用了", "description": "跳过视频搜索"}
    ],
    "multiSelect": false
  }]
}
```

### 第5步：根据用户选择搜索视频

#### 选项1：B站 + YouTube（stealth 浏览器）

同时搜索两个平台：
```
# B站
browser-act stealth-extract "https://search.bilibili.com/all?keyword=<英雄A>+vs+<英雄B>+ob&order=pubdate" --content-type markdown --custom-proxy "http://127.0.0.1:7890"

# YouTube
browser-act stealth-extract "https://www.youtube.com/results?search_query=<英雄A>+vs+<英雄B>+ob+韩服" --content-type markdown --custom-proxy "http://127.0.0.1:7890"
```

B站搜索词格式：`英雄A vs 英雄B ob`、`英雄A 对线 英雄B 韩服ob`、`选手名 英雄A vs 英雄B`
YouTube 搜索词格式：`英雄中文名 vs 英雄中文名 ob 韩服`、`英雄英文名 vs 英雄英文名 ob high elo`

输出时附上视频链接。

#### 选项2：全部平台（含抖音，使用 chrome-direct）

先搜 B站 + YouTube（同上），然后搜抖音：
```
# 抖音（需要先确认 chrome-direct session 是否已存在）
browser-act --session douyin browser open direct_local_98874150816317472 "https://www.douyin.com/search/<英雄A>%20vs%20<英雄B>%20ob?type=video"
browser-act --session douyin get markdown
browser-act session close douyin
```

搜完抖音后关闭 session，释放用户的 Chrome。

#### 选项3：不用了

跳过，不做任何操作。

## 输出格式

### 攻略部分

```markdown
## 英雄A vs 英雄B 对线数据

| 数据 | 英雄A | 英雄B |
|------|------|------|
| 对线击杀率 | XX% | XX% |
| 游戏胜率 | XX% | XX% |
| KDA | X.XX:1 | X.XX:1 |
| 一塔时间 | XX'XX" | XX'XX" |

## 出装符文

### 符文
（列出具体符文树）

### 召唤师技能
（使用率 + 胜率）

### 出装
出门装 → 核心三件 → 第四件

### 对线打法
（基于实际数据的打法建议）
```

### 视频部分（如果用户要搜）

```markdown
## 对线视频

### B站
| 视频 | UP 主 | 时间 | 链接 |
|------|------|------|------|

### YouTube
| 视频 | 时间 | 链接 |
|------|------|------|

### 抖音（如适用）
| 视频 | UP 主 | 时间 | 链接 |
|------|------|------|------|
```

**推荐格式：** 按时间排序，优先推荐近半年内的；标注哪些是职业选手视角；每个平台选 2-3 个最相关的。

## 严格禁止

1. **不要编造技能交互** —— 不确定某个技能能不能挡另一个技能，就说"不确定"，不要瞎编
2. **不要把游戏总胜率当成对线击杀率** —— 这是两个完全不同的数据
3. **不要跳过对线击杀率** —— 用户最关注这个数据
4. **数据引用要标注来源** —— 是 OP.GG 的翡翠+段位数据
5. **搜完抖音必须关闭 session** —— 会占用用户的 Chrome
6. **不要跳过第4步的询问** —— 每次查完攻略都要问用户要不要看视频
