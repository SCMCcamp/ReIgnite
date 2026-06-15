# RE:Ignite 营会模拟游戏系统

RE:Ignite 是一个以「人生资源」为核心的沉浸式营会模拟游戏。玩家在工作、超市、娱乐、团康、生命抉择与修复者等区域之间移动，用 R 币代表自己的时间、精力与健康，在赚钱、消费、冒险、关系建立与修复中体验选择带来的后果。

本仓库提供一套基于 HTML、CSS、JavaScript 与 Supabase Realtime 的现场操作系统，可用于中控管理、玩家查看个人状态，以及分区 NPC 记录交易和事件。

项目说明白手册：`Re_Ignite_说明白手册.pdf`

## 核心概念

- R 币：代表玩家的人生资源，可通过工作、游戏、股票、任务或事件获得，也可能因为消费、税务、赌博、贷款或危机被扣除。
- 生命状态：以颜色呈现玩家状态，通常从绿色开始，并可能因过劳、诱惑、赌博、危机或修复事件改变。
- 分站体验：玩家在不同区域中面对不同诱因，例如工作压力、消费选择、娱乐下注、团队关系与生命修复。
- 实时同步：所有玩家状态、广播、经济动态、股价与日志通过 Supabase 同步，方便现场多端操作。
- 回顾标签：系统与 NPC 操作可整理出玩家历程标签，例如重生者、幸存者、负债者、工作狂、消费者、冒险家、帮助者、投资者、现金者、暴发户等。

## 文件结构

```text
.
├── admin.html                  # 中控端：玩家、广播、经济、股市、税务与回执管理
├── player.html                 # 玩家端：个人状态、广播、股价、生命状态与回执显示
├── supermarket.html            # 超市区 NPC 操作端：R 币、股票与消费记录
├── game.html                   # 娱乐区 NPC 操作端：R 币、贷款与娱乐行为记录
├── Re_Ignite_说明白手册.pdf     # 活动玩法、分站规则、岗位与时间线说明
└── README.md                   # 项目说明
```

## 页面入口

部署到 GitHub Pages 后，可按以下路径访问：

| 页面 | 路径 | 用途 |
| --- | --- | --- |
| 中控端 | `/admin.html` | 总控人员管理全局状态 |
| 玩家端 | `/player.html` | 玩家查看自己的 R 币、生命状态、股票与消息 |
| 超市区 | `/supermarket.html` | 超市 NPC 记录消费、股票与工作相关操作 |
| 娱乐区 | `/game.html` | 娱乐区 NPC 记录下注、贷款与 R 币变化 |

玩家端支持用 URL 参数快速进入指定玩家：

```text
player.html?name=张三&group=A组
```

## 系统功能

### 中控端

- 管理玩家资料、组别、R 币、生命颜色、贷款、股票与站点进度。
- 发布快报、经济动态与现场事件。
- 管理 RE:Set 股价，可手动、随机或按经济事件调整。
- 执行税务事件，例如固定扣除、富人税或危机处罚。
- 查看玩家操作日志，并支持撤销部分 R 币、贷款与股票操作。
- 生成并发布「人生回执单」，用于活动结束后的反思与回顾。

### 玩家端

- 查看个人生命颜色、R 币、贷款、股票持有量与市值。
- 实时接收快报、经济动态与中控发布的回执单。
- 显示 RE:Set 股价趋势。
- 根据生命颜色呈现不同强度的视觉压力效果。
- 当生命状态进入黑色时触发 GAME OVER 遮罩。
- 支持深色与浅色主题，并适配手机端使用。

### 超市区

- 作为 R 币消费区与股票交易服务点。
- NPC 可记录玩家购买商品、股票买卖与 R 币增减。
- 支持组别筛选、倒计时与连接状态提示。
- 操作会写入玩家日志，方便中控追踪。

### 娱乐区

- 作为 R 币消费与风险区，支持骰子、21 点、游戏币兑换等玩法。
- NPC 可记录玩家输赢、R 币变化与贷款。
- 玩家可在娱乐区任职，也可能因连续娱乐或非法贷款触发惩罚。
- 操作会写入玩家日志，方便中控追踪。

## PDF 玩法摘要

### 分站设置

| 区域 | 核心体验 | 主要规则 |
| --- | --- | --- |
| 工作区 | 被任务淹没，学习优先级 | 每 15 分钟一轮，最多 20 人；完成任务获得 R 币；连续完成过多任务可能降低生命状态 |
| 生命抉择 | 人生路上不一定总能做正确选择 | 玩家面对交换、拾取遗留 R 币、股票消息等诱惑；接受诱惑可能获得 R 币但降低生命状态 |
| 超市区 | R 币兑换与股票交易 | 玩家购买商品、消费 R 币、购买 RE:Set 股；消费本身不改变生命颜色 |
| 娱乐区 | 消费、下注与冒险 | 玩家可下注、玩牌、兑换游戏币或任职；连续娱乐过度可能降低生命状态 |
| 团康区 | 关系建立 | 玩家需要跨组组队挑战；胜利可获得 R 币，成为志愿者可提升生命状态 |
| 修复者 | 放下一切并恢复生命 | 玩家可选择放弃所有 R 币恢复为绿色，或通过修复者事件提升生命状态 |

### 工作任务示例

- 到超市区任职：收银员、排货员。
- 到娱乐区任职：荷官、保安、工作人员。
- 到工作区任职：管理、安排任务、疏通人群、支持 R 币兑换。
- 到团康区任职：协调玩家与游戏小帮手。
- 礼品工厂：包装礼物。
- 千纸鹤工厂：折叠千纸鹤、手写卡片或抄写经文。
- 创意部：协助拍照与录制宣传短视频。

### 团康游戏示例

- 同心不落地：用吸管传递橡皮筋，考验团队配合。
- 腋下风暴：夹气球穿越障碍，将成功带到终点的气球计分。
- 速度之王：限时抢水瓶，按带回数量计分。
- 旋转吧！食物：蒙眼喂食挑战，按成功次数计分。

### 经济与事件时间线

活动约 2.5 小时，PDF 中安排了多段市场与危机事件：

- 娱乐区开放：玩家可任职获得报酬。
- 超市区开张：商品初始促销，开放消费与股票交易。
- 税务局突袭：强制征收 R 币；不足者可能被带去工作区劳动。
- 娱乐区二阶：奖池翻倍，开放更高风险玩法。
- 金融危机：工作机会锐减。
- 超市二阶：报酬与商品价格调整。
- 娱乐区三阶：开放贷款与更高倍率赌注。
- 超市三阶：薪资与股票收益提高。
- 扫赌行动：取缔娱乐区，处罚在场玩家并关闭非法贷款。
- 能源危机：工作站关闭。
- 经济放宽：税务、富人税、商品价格与市场状态再次调整。
- 结束与反思：回顾玩家历程与生命选择。

## Supabase 数据表

建议在 Supabase SQL Editor 执行以下初始化脚本，并为相关表开启 Realtime。

```sql
CREATE TABLE players (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  group_name TEXT NOT NULL,
  color TEXT DEFAULT 'green',
  rbc INTEGER DEFAULT 5,
  loan INTEGER DEFAULT 0,
  stock INTEGER DEFAULT 0,
  is_workaholic BOOLEAN DEFAULT false,
  is_consumer BOOLEAN DEFAULT false,
  is_adventurer BOOLEAN DEFAULT false,
  is_helper BOOLEAN DEFAULT false,
  s1 BOOLEAN DEFAULT false,
  s2 BOOLEAN DEFAULT false,
  s3 BOOLEAN DEFAULT false,
  s4 BOOLEAN DEFAULT false,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE news (
  id SERIAL PRIMARY KEY,
  content TEXT NOT NULL,
  time TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE economy_status (
  id SERIAL PRIMARY KEY,
  content TEXT NOT NULL,
  time TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE stock_prices (
  id SERIAL PRIMARY KEY,
  price INTEGER NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE player_logs (
  id BIGSERIAL PRIMARY KEY,
  player_id INTEGER NOT NULL,
  action_type TEXT NOT NULL,
  old_value TEXT,
  new_value TEXT,
  detail TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE player_receipts (
  player_id INTEGER PRIMARY KEY,
  max_rbc INTEGER,
  min_rbc INTEGER,
  most_dangerous_time TEXT,
  behavior_tags TEXT[],
  metaphor TEXT,
  summary_text TEXT,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

ALTER TABLE players REPLICA IDENTITY FULL;
ALTER TABLE player_logs REPLICA IDENTITY FULL;
ALTER TABLE news REPLICA IDENTITY FULL;
ALTER TABLE economy_status REPLICA IDENTITY FULL;
ALTER TABLE stock_prices REPLICA IDENTITY FULL;
ALTER TABLE player_receipts REPLICA IDENTITY FULL;

ALTER PUBLICATION supabase_realtime ADD TABLE players;
ALTER PUBLICATION supabase_realtime ADD TABLE player_logs;
ALTER PUBLICATION supabase_realtime ADD TABLE news;
ALTER PUBLICATION supabase_realtime ADD TABLE economy_status;
ALTER PUBLICATION supabase_realtime ADD TABLE stock_prices;
ALTER PUBLICATION supabase_realtime ADD TABLE player_receipts;
```

## 配置方式

1. 创建 Supabase 项目。
2. 在 Supabase SQL Editor 执行上方 SQL。
3. 在 `admin.html`、`player.html`、`supermarket.html` 与 `game.html` 中填写 Supabase 项目凭证。

```javascript
const SUPABASE_URL = "https://你的项目.supabase.co";
const SUPABASE_ANON_KEY = "你的 anon key";
```

4. 确认 Supabase Realtime 已为所有相关表启用。
5. 本地直接打开 HTML 文件测试，或部署到 GitHub Pages。

## 部署到 GitHub Pages

1. 将仓库推送到 GitHub。
2. 进入仓库 `Settings` -> `Pages`。
3. Source 选择 `Deploy from a branch`。
4. Branch 选择 `main`，目录选择 `/root`。
5. 保存后等待 GitHub Pages 构建完成。

部署完成后，访问地址通常为：

```text
https://你的用户名.github.io/ReIgnite/admin.html
https://你的用户名.github.io/ReIgnite/player.html
https://你的用户名.github.io/ReIgnite/supermarket.html
https://你的用户名.github.io/ReIgnite/game.html
```

## 技术栈

- HTML5
- CSS3
- JavaScript
- Chart.js
- Supabase PostgreSQL
- Supabase Realtime
- Google Fonts: Rajdhani、Noto Sans SC

## 活动运营提醒

- 现场每个区域需要 NPC 熟悉规则，并及时在操作端记录玩家变化。
- R 币变化、贷款、股票与生命颜色调整应尽量实时录入。
- 工作区、娱乐区和团康区有容量与轮次限制，建议安排人员维持秩序。
- 税务局、扫赌行动、能源危机等事件会强烈影响玩家体验，建议由中控统一广播。
- 活动结束前可通过人生回执单与标签回顾，引导玩家反思自己的选择、压力、关系与修复。

## License

MIT License
