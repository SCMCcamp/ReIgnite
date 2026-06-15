# RE:Ignite HTML User Guide

This guide explains how to use the four HTML pages in this repository during a RE:Ignite event.

## Pages

| File | Main user | Purpose |
| --- | --- | --- |
| `admin.html` | Game director / control team | Manage all players, broadcasts, economy, stock price, tax, logs, and end-game receipts |
| `player.html` | Players | View personal status, R coins, life color, messages, stock value, and end-game receipt |
| `supermarket.html` | Supermarket NPC | Record purchases, R coin changes, and RE:Set stock transactions |
| `game.html` | Entertainment NPC | Record gambling, entertainment R coin changes, and loans |

## Before The Event

1. Open the Supabase project used by the HTML files.
2. Confirm these tables exist and have Realtime enabled:
   - `players`
   - `news`
   - `economy_status`
   - `stock_prices`
   - `player_logs`
   - `player_receipts`
3. Add all players to the `players` table before the event starts.
4. Confirm each player has:
   - `name`
   - `group_name`
   - starting `color`, usually `green`
   - starting `rbc`
   - starting `loan`, usually `0`
   - starting `stock`, usually `0`
5. Test all four pages on the devices that will be used on-site.
6. Keep `admin.html` open on the control team device throughout the game.

## General Operating Rules

- Use one official device for each NPC station to avoid duplicate entries.
- Record every R coin, stock, loan, and life-color change immediately.
- If a mistake is made, use the admin player log to undo where possible.
- Keep the control team informed before major events such as tax raids, gambling shutdowns, and economy changes.
- Do not refresh repeatedly during live play unless a page stops syncing.

## Admin Page

Open:

```text
admin.html
```

The admin page is the control center for the whole event.

### Broadcast News

Use the `快报广播` panel to send announcements to all player devices.

1. Type the announcement in the input box.
2. Click `发送快报`.
3. The latest message appears immediately on `player.html`.
4. Click `展开历史` to view or delete recent announcements.

Use this for:

- Station openings and closures.
- Tax raids.
- Gambling shutdowns.
- Crisis events.
- End-game instructions.

### Publish Economy Status

Use the `经济状况` panel to update the game economy.

1. Click a preset:
   - `正常`
   - `通胀`
   - `通缩`
2. Or type a custom economy message.
3. Click `发布经济动态`.

The newest economy update appears on player devices and can affect stock movement when economy-linked adjustment is used.

### Manage Stock Price

Open `RE:Set股 · 实时价格走势`.

Available actions:

- `随机波动`: creates a random stock price movement.
- `经济联动调整`: adjusts stock price based on the latest economy status.
- `手动设置股价`: enter a number, then click `设置`.

Use this when the game timeline calls for:

- Stock boost.
- Stock crash.
- Market warmth.
- Financial crisis.

### Manage Tax

Open `税收管理`.

Available actions:

- `富人税（阶梯）`: deducts tax based on each player’s R coin amount compared with the configured brackets.
- `扣除每人 3 R币`: deducts 3 R coins from every player.
- `添加税率区间`: adds another progressive tax bracket.

Before clicking tax buttons:

1. Announce the event through `快报广播`.
2. Check the tax preview.
3. Confirm the control team is ready.
4. Apply the tax once.

### Player Table

The player table is used to update individual player state.

Common controls:

- Life color dots: set a player to green, yellow, orange, red, or black.
- R coin buttons: adjust R coins by `-5`, `-2`, `-1`, `+1`, `+2`, or `+5`.
- Loan buttons: adjust loan by `-20` or `+20`.
- Stock buttons: adjust stock count by `-1` or `+1`.
- Behavior checkboxes: mark behavior tags such as workaholic, consumer, adventurer, and helper.
- Reborn button: mark the player as restored or reborn.
- Detail button: open that player’s operation log.

### Filters

Use the group chips to show players from a specific group.

Use the status cards to filter by:

- All players.
- Green.
- Yellow.
- Orange.
- Red.
- Black.
- Warning status.

Click `清除筛选` to return to the full list.

### Undo Mistakes

1. Find the player in the table.
2. Click `详情`.
3. Review recent operation logs.
4. Click `撤销` beside the mistaken action if available.

Use undo carefully. It should be used for operator mistakes, not for normal game consequences.

### Generate End-Game Receipts

Open `人生回执单 · 个性化总结与发布`.

1. Click `生成所有玩家回执单预览`.
2. Review generated tags and summaries.
3. Click `批量发布到数据库`.
4. Player pages will receive and show the receipt.

Use this near the end of the event, after final player values and tags are correct.

### Reset All

The `重置所有` button resets global player state. Only use it before a test run or before a fresh event.

Do not use it during live gameplay unless the whole event is being restarted.

## Player Page

Open:

```text
player.html
```

Players use this page to view their own live game status.

### Choose A Player

If no player is selected, the page shows a role selector.

1. Choose the correct name and group.
2. Click `确认身份`.
3. The page loads that player’s personal dashboard.

You can also open a player directly with URL parameters:

```text
player.html?name=张三&group=A组
```

### Player Dashboard

Players can see:

- Name and group.
- Current life color.
- R coin amount.
- Loan amount.
- Stock holdings.
- Station progress.
- Latest RE:Ignite news.
- Latest economy status.
- RE:Set stock price and chart.
- Current stock value.

### News And Economy History

If history is available:

1. Tap `历史记录` under news or economy.
2. Review recent messages.
3. Tap again to collapse the list.

### Theme Toggle

Tap the theme button in the top-right corner to switch between dark and light mode.

### Life Color Effects

The page becomes visually heavier as the player’s life color worsens.

Typical order:

```text
green -> yellow -> orange -> red -> black
```

When a player reaches black, the page shows a full-screen `GAME OVER` overlay. The player can dismiss the overlay, but their life color remains black until the admin changes it.

### End-Game Receipt

When the admin publishes receipts, the player page shows an `人生回执单`.

The receipt can include:

- Highest R coin count.
- Lowest R coin count.
- Stock holdings.
- Most dangerous moment.
- Behavior tags.
- Reflection sentence.

Players can close it by tapping the confirmation button or the backdrop.

## Supermarket Page

Open:

```text
supermarket.html
```

Use this page at the supermarket station.

### Timer

The page includes a 15-minute work timer.

- The timer starts automatically.
- Click `⟳` to reset it to `15:00`.
- A warning appears during the final 5 minutes.

### Group Filter

Use group chips at the top to show only one group or all players.

### Player Row

Each player row shows:

- Player ID.
- Name.
- Group.
- Current R coins.
- Current stock count.
- Controls for R coins.
- Controls for stock.

### Record R Coin Changes

Use the R coin buttons for purchases, work pay, refunds, and other supermarket actions.

Available values:

```text
-5, -2, -1, +1, +2, +5
```

Examples:

- Player buys a 5R item: click `-5`.
- Player earns 2R from a supermarket role: click `+2`.
- Operator corrects an accidental overcharge: click the matching positive amount.

### Record Stock Changes

Use the stock buttons:

- `+1`: player buys one RE:Set stock.
- `-1`: player sells or loses one RE:Set stock.

The operation is logged to `player_logs`.

## Entertainment Page

Open:

```text
game.html
```

Use this page at the entertainment station.

### Timer

The page includes a 15-minute work timer.

- The timer starts automatically.
- Click `⟳` to reset it to `15:00`.
- A warning appears during the final 5 minutes.

### Group Filter

Use group chips at the top to show only one group or all players.

### Player Row

Each player row shows:

- Player ID.
- Name.
- Group.
- Current R coins.
- Current loan amount.
- Controls for R coins.
- Controls for loans.

### Record R Coin Changes

Use the R coin buttons to record gambling wins, gambling losses, entertainment spending, staff pay, or penalties.

Available values:

```text
-5, -2, -1, +1, +2, +5
```

Examples:

- Player loses 2R: click `-2`.
- Player wins 5R: click `+5`.
- Player works as staff and earns 2R: click `+2`.

### Record Loans

Use the loan buttons:

- `+20`: player takes a loan.
- `-20`: player repays or has a loan removed.

Coordinate loan rules with the control team before using this during the event.

## Recommended Event-Day Flow

### 1. Setup

1. Open `admin.html` on the control team device.
2. Open `supermarket.html` on the supermarket device.
3. Open `game.html` on the entertainment device.
4. Ask players to open `player.html`.
5. Confirm every device shows `实时在线` or connected status.
6. Send a test news broadcast.
7. Confirm player devices receive it.

### 2. Start

1. Confirm all players have the correct starting R coins and life color.
2. Announce the opening message from `admin.html`.
3. Open the first stations according to the event timeline.

### 3. During Play

1. NPCs record station actions immediately.
2. Admin publishes economy and crisis events.
3. Admin adjusts life colors when NPCs report consequences.
4. Admin checks player logs if disputes occur.
5. Admin uses stock and tax tools according to the event timeline.

### 4. End Game

1. Stop station activities.
2. Confirm final R coins, loans, stocks, and life colors.
3. Mark final behavior tags.
4. Generate receipt previews.
5. Publish receipts.
6. Ask players to read their receipt on `player.html`.
7. Begin debrief or reflection.

## Troubleshooting

### A Player Cannot See Their Name

- Confirm the player exists in the `players` table.
- Confirm the player has a `group_name`.
- Refresh `player.html`.

### Page Does Not Update

- Check internet connection.
- Refresh the page once.
- Confirm Supabase Realtime is enabled.
- Check whether the device is using the correct HTML file.

### Wrong R Coin Value

- Use `admin.html`.
- Open the player’s `详情`.
- Undo the mistaken operation if available.
- If undo is not available, manually adjust using the R coin buttons.

### Wrong Player Selected On Player Page

- Open `player.html` again without URL parameters.
- Choose the correct player.
- Or use the direct URL format:

```text
player.html?name=正确名字&group=正确组别
```

### GAME OVER Appears

This means the player’s life color is black.

- If correct, continue the game rules as planned.
- If it was a mistake, change the player’s life color from `admin.html`.

## Operator Checklist

- Admin device is open and connected.
- Supermarket device is open and connected.
- Entertainment device is open and connected.
- Player list is correct.
- Starting R coins are correct.
- Starting life colors are correct.
- News broadcast test works.
- Economy update test works.
- NPCs understand which buttons to press.
- End-game receipt process is tested before the live event.
