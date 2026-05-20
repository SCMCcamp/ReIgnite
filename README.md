# 🔥 RE:Ignite · 营会模拟游戏系统

> **在忙碌与压力中重新点燃生命，兴起职场青年活出信仰见证**

RE:Ignite 是一个**沉浸式「迷你人生模拟」营会游戏系统**。玩家在「R城」中通过真实的选择改变生命颜色（🟢绿 / 🟡黄 / 🟠橙 / 🔴红 / ⚫黑），赚取/花费 R币（人生资源），经历忙碌、压力、诱惑、修复等职场人生场景。

系统由**中控端**、**玩家端**及**分区 NPC 操作端**组成，所有数据通过 **Supabase** 实时同步，适合 30–50 人的营会活动。

👉 项目地址：[https://github.com/SCMCcamp/ReIgnite](https://github.com/SCMCcamp/ReIgnite)

---

## ✨ 核心功能

### 👑 中控端 (`admin.html`)
- **全局玩家管理**：R币增减（±1/±2/±5）、生命颜色切换、站台进度（站1~站4）、贷款/股票管理
- **快报广播**：发送系统公告，自动记录历史，支持删除
- **经济动态**：发布通胀/通缩/正常事件，联动股价波动
- **股票模块**：RE:Set 股 K线图（手动/随机/经济联动），每30分钟自动波动
- **税收工具**：阶梯式富人税（基于平均值，可自定义税率区间）或固定扣除每人 3 R币
- **人生回执单**：自动分析玩家日志生成个性化标签（重生者、投资者、股市达人等），支持预览与批量发布
- **玩家详情侧边栏**：操作记录日志，支持**撤销**（R币/贷款/股票回滚）
- **小组筛选、颜色状态筛选**
- **实时数据订阅**（Supabase Realtime）

### 👤 玩家端 (`player.html`)
- 个人状态卡片（生命颜色、R币、持股数量）
- 实时接收快报 & 经济动态（带脉冲通知动画）
- RE:Set 股行情图及个人持股市值
- 生命颜色特效（绿→黄→橙→红→黑逐级增强视觉压迫感）
- 当生命颜色变为黑色时，自动触发**全屏 GAME OVER 遮罩**
- 角色选择器（首访或 URL 参数 `?name=张三&group=A组` 自动登录）
- **人生回执单弹窗**：收到中控端发布的回执单后实时弹出（通过广播频道），支持触摸下滑关闭
- **深色/浅色主题切换**
- 手机端适配

### 🏪 分区 NPC 操作端（可选，基于示例扩展）
- 超市区 (`station2-supermarket.html`) & 娱乐区 (`station3-entertainment.html`)
- 展示玩家 ID、姓名、组别、R币（娱乐区额外有贷款复选框）
- R币快捷操作（+1/+2/+5、-1/-2/-5）
- 工作倒计时（可重置）
- 连接状态指示灯
- 小组筛选
- 所有操作自动记录到 `player_logs` 表

---

## 🗄️ 数据库结构 (Supabase)

| 表名 | 用途 |
|------|------|
| `players` | 玩家信息（姓名、组别、颜色、R币、贷款、股票、站台进度、手动标签） |
| `news` | 快报历史 |
| `economy_status` | 经济动态历史 |
| `stock_prices` | 股价历史 |
| `player_logs` | 玩家操作日志（R币/贷款/股票/颜色/站台/行为标签） |
| `player_receipts` | 人生回执单（最高/最低R币、最险时刻、标签、隐喻文案） |

> ⚠️ 所有表需开启 **Realtime**（`ALTER TABLE ... REPLICA IDENTITY FULL;`）以实现实时同步。

---

## 🚀 部署到 GitHub Pages

### 1. 准备工作
- 注册 [GitHub](https://github.com) 账号
- 创建 Supabase 项目，获取 `SUPABASE_URL` 和 `SUPABASE_ANON_KEY`
- 在 Supabase SQL Editor 中执行初始化脚本（见下方）

### 2. 初始化 Supabase 表

```sql
-- 一键复制到 SQL Editor 执行
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

-- 开启 Realtime（必需）
ALTER TABLE players REPLICA IDENTITY FULL;
ALTER TABLE player_logs REPLICA IDENTITY FULL;
ALTER TABLE news REPLICA IDENTITY FULL;
ALTER TABLE economy_status REPLICA IDENTITY FULL;
ALTER TABLE stock_prices REPLICA IDENTITY FULL;
ALTER TABLE player_receipts REPLICA IDENTITY FULL;

-- 将表添加到实时发布
ALTER PUBLICATION supabase_realtime ADD TABLE players;
ALTER PUBLICATION supabase_realtime ADD TABLE player_logs;
ALTER PUBLICATION supabase_realtime ADD TABLE news;
ALTER PUBLICATION supabase_realtime ADD TABLE economy_status;
ALTER PUBLICATION supabase_realtime ADD TABLE stock_prices;
ALTER PUBLICATION supabase_realtime ADD TABLE player_receipts;
3. 配置前端文件
下载本仓库的所有 .html 文件，并修改其中的 Supabase 凭证：

javascript
const SUPABASE_URL = "https://你的项目.supabase.co";
const SUPABASE_ANON_KEY = "你的 anon key";
需要修改的文件：admin.html、player.html（如有分区操作端也需修改）

4. 上传到 GitHub
创建一个新仓库（例如 ReIgnite）

将以下文件上传到仓库根目录：

admin.html

player.html

README.md（本文件）

（可选）station2-supermarket.html、station3-entertainment.html

进入仓库 Settings → Pages，将 Branch 设为 main，保存

5. 访问地址
页面	地址
中控端	https://你的用户名.github.io/ReIgnite/admin
玩家端	https://你的用户名.github.io/ReIgnite/player
超市区	https://你的用户名.github.io/ReIgnite/supermarket（如已添加）
娱乐区	https://你的用户名.github.io/ReIgnite/game（如已添加）
💡 玩家端支持 URL 参数直接进入：player.html?name=张三&group=A组


📁 文件结构
text
.
├── admin.html                    # 中控端（总控台）
├── player.html                   # 玩家端（个人状态 + 实时消息）
├── station2-supermarket.html     # 超市区（可选，需自行添加）
├── station3-entertainment.html   # 娱乐区（可选，需自行添加）
├── README.md                     # 项目说明
└── (可选) assets/                # 图片、图标等资源
🧰 技术栈
前端：HTML5、CSS3、JavaScript (ES6+)

图表：Chart.js 4.4.0

后端服务：Supabase（PostgreSQL + Realtime + 广播）

字体：Google Fonts (Rajdhani + Noto Sans SC)

🤝 贡献与定制
欢迎根据营会实际需求调整：

修改 group_name 的组别名称（需同步修改 Supabase 数据和所有前端筛选按钮）

调整 rbc 初始值、倒计时时长等

自定义快报/经济动态的预设文案

修改税收阶梯或股票评分规则（在 admin.html 中调整相应函数）

📄 许可证
MIT License

🙏 致谢
本游戏设计灵感源于 Life Game 及职场青年营会需求，所有代码为原创，可自由使用于非商业营会活动。

在忙碌与压力中，愿每一个人都能重新点燃生命，成为职场中的光。 🔥
