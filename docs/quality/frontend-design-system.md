# CampusQuest Frontend Design System

> Purpose: give frontend and design agents one stable visual language across Student, Teacher, and Admin surfaces.
>
> Product direction: **academic productivity with restrained gamification**, themed as
> **「羊皮卷与火漆」Parchment & Wax Seal（酒馆派遣 × 学院风）** per §0 (owner-approved 2026-09-30).

## 0. Theme amendment: Parchment & Wax Seal（owner 批准 2026-09-30）

本节是 owner 已批准的方向修订。凡本节与下文旧规则冲突，**以本节为准**；AGENTS.md 中
「禁装饰渐变 / 克制游戏化」等视觉条款在 §0 圈定的表面上同样被本节取代（AGENTS.md 文件本体的
相应措辞由 owner 择机更新）。产品语义、API、状态机、RBAC、隐私规则不受本节影响。

### 0.1 生效范围（分层契约）

- **主题化表面**：Landing、Student 全部页面（含委托板、详情、赏金铺、荣誉堂、信使）。
- **维持冷静工作台**：Teacher、Admin、认证/2FA、审核与审计界面——只同步 §0.2 色板与
  排版，不引入纹理粒子、hero wash、装饰动效（沿用既有 §9/§11 的严肃约束）。
- 积分/兑换/审核等业务语义不变；「委托/令牌/接取/赏金铺/荣誉殿堂/信使」仅为文案层命名。

### 0.2 Token 层（映射到 globals.css 既有变量名）

| 变量 | 值（浅暖主题） | 说明 |
| --- | --- | --- |
| `--background` | `#f2e9d5` 羊皮纸暖白 | 页面底色 |
| `--surface-1` / `--surface-2` | `#faf3e3` 米纸 / `#ece0c4` 旧纸 | 面板两级 |
| `--foreground` / `--muted-foreground` / `--subtle-foreground` | `#33291c` 墨褐 / `#6f6350` / `#8c7f69` | 正文/辅助/最弱 |
| `--border` / `--border-strong` | `#d9c9a6` / `#b9a377` 皮革褐 | 静边框 / 强边框 |
| `--primary` / `--primary-strong` | `#9c2b23` 火漆红 / `#7e211b` | 主操作与当前导航 |
| `--danger` / `--warning` / `--success` / `--info` | `#a3271e` 朱砂 / `#a8791c` / `#4a7c3f` / `#2f5d8a` | 语义不变 |
| `--rarity-normal/rare/epic/legendary` | `#5b5648` / `#2f5d8a` / `#6d4a9e` / `#a8791c` | 难度火漆色，见 §0.3 |
| `--font-display` | Georgia + 中宋/宋体 serif 栈 | 仅标题/徽记/数字大字 |
| 形状 | radius 8 / 14 / 22px；`--shadow-1` 静物影、`--shadow-2` 弹层影 | 卡片仍靠边框+表面分层 |

纹理：羊皮纸底纹与卷轴纸纹用**内联 SVG `feTurbulence` data-URI**（≤2KB、非阻塞），
禁止外链位图纹理；表格数字沿用 `tabular-nums`。

### 0.3 难度等级视觉体系（炉石式：徽记 + 边框双通道）

难度=任务稀有度（NORMAL/RARE/EPIC/LEGENDARY），**文字标签永远同行渲染**，颜色永不单独表义。
徽记与边框是两条独立通道，都必须呈现：

| 难度 | 徽记（SVG，45° 内区分形状） | 卡片边缘效果 |
| --- | --- | --- |
| 普通 | 墨线**方戳**（双线框，纸底墨字） | 1.5px 墨褐单线框，哑光纸面 |
| 稀有 | **圆蜡印**（蓝蜡渐变 + 蜡泪细节） | 2px 蓝蜡外框 + 内嵌 3px 纸底细线（双框），悬停冷光泽 |
| 史诗 | **八角紫蜡印**（内环 + 花角） | 紫色花角饰件（四角 L 形）+ 内侧发丝线，轻晕影 |
| 传说 | **金箔盾徽**（月桂弧 + 缎带 + 星） | 金箔渐变厚框（border-box 渐变）+ 四角饰件 + 顶嵌宝石，悬停一次性光泽扫过 |

- 徽记为内联 SVG（`data-seal` 注入），禁用位图；`<text>` 保留汉字字符（普/稀/史/传）。
- 传说**无待机循环动画**；光泽扫过仅 hover/focus-within 触发一次，`prefers-reduced-motion`
  下退化为瞬时状态。
- 徽记不大于任务标题的字重视觉权重；卡片标题仍是卡片的第一元素。

### 0.4 卷轴弹窗（scroll dialog）规范

- 结构：`上卷轴杆 + 羊皮纸身 + 下卷轴杆`。杆 = 木纹渐变圆杆 + 两端轴头；纸身 = 米纸 +
  纸纹 + 上下内阴影；杆略宽于纸（纸从中抽出）。
- 打开动画 = **展开**：纸身 `clip-path: inset(50% 0 50% 0 → 0)`（GSAP，150–450ms），
  关闭快速收合；禁止整框 fade。
- 弹窗语义不变：native `<dialog>`（焦点圈、Escape、背板关闭）；表单/告警/按钮沿用 §9 组件规则。
- 纸面上所有文字对比度按 AA 校验；纹理不降低可读性。

### 0.5 动效清单（Landing + Student 表面，GSAP 3）

允许并已入库的动效（实现见 `docs/design/campusquest-demo-a.html`，生产化为 `motion/` 模块）：

1. hero 编排：徽记 SVG 描边 → 标题/文案/CTA 依次入柱 → 委托卡发牌 stagger；
2. 烛尘粒子（12–16 枚，纯装饰，`aria-hidden`，reduced-motion 直接隐藏）；
3. 统计数字滚动计数（进视口一次）；
4. 旅程/进度路径 scrub 描绘（ScrollTrigger）；
5. 按钮：火漆按压（scale .92 + back.out 回弹 + 印环）、次按钮涟漪、CTA 磁吸（quickTo）；
6. 委托卡拖拽（Draggable + bounds + 弹性落座）；
7. 火漆盖印签名交互（拖印章 → hitTest → 印痕落下 + 金屑四溅）；
8. 卷轴弹窗展开/收合；吐司滑入。

硬约束：全部动效包在 `gsap.matchMedia("(prefers-reduced-motion: no-preference)")`，
revert 后页面必须完整可见（无元素卡在隐藏态）；动效仅 transform/opacity/clip-path/stroke-dashoffset；
不阻塞交互（pointer-events none 的装饰层）；员工端表面不引入以上任何一项。

### 0.6 引用实现

- 交互/视觉基线 demo：`docs/design/campusquest-demo-a.html`（A 方案产品化 demo，owner 审查用）。
- 三方案对比的历史提案：`docs/design/quest-style-proposals.html`（B/C 方案留档，不再实施）。
- 基线（旧克制风格）demo：`docs/design/ui-preview-demo.html`。

## 1. Design intent

CampusQuest should feel credible enough for teachers and administrators, motivating enough for students, and dense enough for real operational work.

The interface should communicate:

- **trust** — academic and administrative actions feel deliberate and auditable;
- **clarity** — task status, deadlines, points, and next actions are obvious;
- **momentum** — progress, ranking, honors, and rarity add energy without turning the product into a game skin;
- **calm density** — dashboards and review queues can show substantial information without visual noise.

The preferred reference family is modern productivity software and well-composed shadcn/Radix dashboards. Do not imitate any reference brand one-for-one.

## 2. Visual personality

Use these adjectives when deciding between two visual treatments:

**calm, precise, compact, optimistic, human, slightly playful** — on themed Student/Landing
surfaces (§0) extend with: **storybook, tactile, candlelit, adventurous**.

Avoid:

**flashy, casino-like, neon, glassy, futuristic-AI, overly corporate, cartoon-heavy**

(§0 amendment: themed textures/parchment gradients are allowed on Student surfaces within the
§0.2 palette; neon/glass/looping celebration stay forbidden everywhere.)

Rarity labels and honors may be playful. Authentication, submission, review, account security, redemption approval, and Admin operations should remain visually serious.

## 3. Token-first implementation

All recurring visual values MUST originate from shared tokens, preferably CSS variables consumed by Tailwind and shadcn primitives.

Do not scatter raw hex values or one-off radius and shadow values through feature components.

Recommended token groups:

~~~text
surface:
  background          (warm-neutral near-white, Plan 11)
  surface-1
  surface-2
  surface-brand       (the ONE soft brand wash: hero/progress emphasis
                       only — never a page background)
  overlay / overlay-scrim

text:
  foreground
  muted-foreground
  subtle-foreground
  inverse-foreground

border:
  border              (soft; background contrast carries hierarchy first)
  border-strong
  focus-ring

semantic:
  primary
  success
  warning
  danger
  info

rarity:
  rarity-normal
  rarity-rare
  rarity-epic
  rarity-legendary

shape:
  radius-sm
  radius-md
  radius-lg

shadow:
  shadow-xs
  shadow-sm
  shadow-raised        (the one lifted level: dialog/popover/hero objects)

motion:
  duration-fast / duration-slow
  ease-standard
  lift-hover           (the 1–2px hover lift interactive cards may take)
~~~

Prefer OKLCH-compatible theme variables when the selected Tailwind and shadcn setup supports them.

### Token usage rules

- Primary accent is for the main action and current navigation state, not every icon.
- Semantic colors communicate actual state, not decoration.
- Rarity colors are accents on badge, icon, or keyline. They MUST NOT become full-page backgrounds.
- Border and surface contrast should carry most hierarchy; shadow is secondary.
- One page should not invent a new visual vocabulary.

## 4. Color strategy

Default experience should be light-first, with tokens structured so a complete dark theme remains possible.

Recommended behavior:

- large page backgrounds: neutral;
- cards and panels: one subtle surface step above background;
- primary action: one clear accent;
- destructive action: semantic danger only;
- success: completed or approved states;
- warning: deadline risk, revision needed, provider failure;
- info: neutral system information.

Rarity mapping should remain recognizable but restrained:

| Rarity | Intended accent |
| --- | --- |
| Normal | neutral or graphite |
| Rare | cool blue |
| Epic | violet |
| Legendary | warm amber or gold |

Do not rely on rarity color alone. Always render the text label.

## 5. Typography

The product is information-heavy. Body typography must prioritize readability over personality.

Use:

- a highly legible sans-serif body and UI font;
- optional restrained display treatment for product or marketing headings only;
- tabular numbers for points, ranks, dates, counts, and table metrics where alignment matters.

Hierarchy guideline:

- page title: strong but not oversized;
- section title: distinct from card titles;
- body: comfortable line height;
- metadata: muted, never so faint that it harms readability;
- dense table text: compact, but not uncomfortably small.

Avoid giant dashboard headings and oversized KPI numbers that push useful information below the fold.

## 6. Spacing and density

Use a consistent spacing scale. Prefer fewer spacing values rather than arbitrary per-component tuning.

Desktop:

- main content gutters clearly separate navigation and work area;
- table and review pages use a denser vertical rhythm than auth pages;
- related controls stay visually grouped.

Mobile:

- keep a comfortable page gutter;
- do not compress tappable actions into tiny icon-only clusters;
- stack primary controls before secondary metadata.

A page with operational data should prioritize the work, not empty whitespace.

## 7. Shape and elevation

Recommended character:

- controls: small-to-medium radius;
- cards: medium radius;
- dialogs and drawers: slightly stronger radius;
- pills: only for badges, statuses, and filter chips.

Use elevation sparingly:

- most cards use border plus surface difference;
- dialogs and popovers may use a shadow;
- avoid making every card look like it is floating.

## 8. Page shells

### Navigation shell (Plan 11: three bands)

All authenticated workspaces share one navigation geometry, three
bands by viewport:

- **Wide (≥64rem)**: a visually QUIET sidebar owns the primary
  navigation (surface step down, muted links, brand-tinted active
  state with an inset keyline; exactly one `aria-current="page"` per
  nav landmark); the top bar degrades to a context/actions strip
  (bell, user — no nav, no brand).
- **Medium (40–64rem)**: the horizontal top nav keeps every
  destination.
- **Narrow (<40rem)**: Student gets a fixed 5-slot bottom nav with
  safe-area inset and reserved content bottom padding (the fixed bar
  never covers a primary action); Teacher/Admin get a hamburger menu
  sheet carrying the FULL staff list (native dialog semantics: focus
  trap, Escape, backdrop close).

Routes never change with the band; reachability does not either.

### Student

Primary navigation should make these easy to reach:

- Home
- Tasks
- My Claims (anchored on the dashboard in V1 — no /claims index route)
- Rankings
- Rewards
- Notifications
- Profile

Dashboard priorities:

1. work requiring action;
2. approaching deadlines and revision requests;
3. points and reward progress;
4. ranking and growth;
5. task discovery.

Do not make the Student home a wall of equal-sized metric cards.
Plan 11 realizes this as ONE next-action hero (the single dominant
element, and the only page-level `--surface-brand` consumer): a
teacher-returned revision outranks everything, else the
earliest-deadline open claim, else a discovery CTA. Discovery cards
dedupe against the student's open claims.

### Teacher

Prioritize:

- review queue;
- task management;
- task statistics;
- assignment import;
- community and reports.

Teacher pages may be denser than Student pages.

### Admin

Prioritize operational scanability:

- users and whitelist;
- rewards and redemptions;
- system and notifications;
- audit;
- repair operations.

Admin pages should favor tables, filters, drawers, and explicit confirmations over decorative dashboard cards.

## 9. Component rules

### Buttons

Use a clear hierarchy:

- Primary: one main action in a local context.
- Secondary: normal alternative.
- Ghost: lightweight toolbar action.
- Destructive: destructive intent only.

A button is a button whatever the element: link-styled buttons
(`<Link className="btn">`) never render the anchor underline; inline
prose links (`.link`) keep theirs.

Avoid two adjacent primary buttons competing for attention.

Icon-only buttons require accessible labels and, where helpful, tooltips.

### Cards

Cards are grouping containers, not the default layout primitive.

Good uses:

- active Claim summary;
- reward item;
- compact growth summary;
- task card in discovery.

Poor uses:

- wrapping every section inside another card;
- turning every desktop table row into a large card;
- one card per label/value pair.

Plan 11 task cards: the title owns the card; rarity is a compact
LEFT KEYLINE (`data-rarity` border color — quiet for NORMAL) with
its TEXT label in the metadata row; color never carries rarity
alone. Interactive cards may take the 1px hover lift
(`--lift-hover`, reduced-motion safe).

### Identity vs status (Plan 11 core rule)

Assignment identity (platform / keyword / claim time) is NEVER drawn
in a semantic tone: `.claim-panel` renders on the neutral
`--surface-brand` surface with a neutral border in every state.
Workflow status lives only on the status badge, the five-step
progress strip (领取 → 提交 → 校验 → 审核 → 完成; current strong,
done quiet, future subtle; non-linear terminals render no strip),
and the alerts. When a revision is required, the revision banner
precedes the assignment facts and owns the visual priority.

### Tables

Use tables for Teacher and Admin dense data.

Requirements:

- sticky header when useful;
- sensible loading, empty, and error states;
- server-backed pagination and filters where data can grow;
- consistent row actions;
- destructive actions never hidden behind ambiguous unlabeled icons;
- numeric columns align predictably.

On mobile, use a deliberate compact/list alternative if a table becomes unreadable.

### Status badges

Status always includes text. Color is supplemental.

Use product wording such as:

- 待提交
- 校验中
- 待审核
- 需修改
- 已完成
- 已过期

Do not expose raw enum names such as UNDER_REVIEW.

### Task rarity（§0.3 修订：难度等级双通道体系）

Rarity treatment（难度=稀有度，普通→传说）：

- **徽记 + 边框双通道**：每档难度有形状不同的 SVG 徽记（墨戳/圆蜡/八角蜡/金盾）与不同的
  卡片边缘效果（单线/双框/花角/金箔框+宝石），见 §0.3 对照表；
- 文字标签（普通/稀有/史诗/传说）永远与徽记同行渲染；颜色永不单独表义；
- 徽记与边框不得大于任务标题的视觉权重；
- 传说无待机循环动画；hover 光泽仅触发一次且 reduced-motion 退化；
- NORMAL 保持安静——普通委托不因装饰而显得比高难委托更醒目。

### Forms

- label remains visible after input;
- help text explains consequences, not the obvious field name;
- errors appear close to the field;
- sensitive forms state what happens next;
- use appropriate autocomplete and input mode;
- disabled states remain legible.

### File upload

Upload UI must show:

- allowed formats;
- size policy;
- current file;
- progress;
- upload and finalize failures;
- validation state;
- structured validation result;
- retry or new-version action.

Drag-and-drop is optional enhancement; keyboard-accessible file selection is required.

### Leaderboard

Leaderboard should feel competitive but not humiliating.

Always provide:

- top results;
- around-me section;
- current-user highlight;
- period switch;
- honor display.

Do not use danger styling for low rank.

### Comments

Anonymous mode must be explicit before posting.

Comment UI should visually separate:

- author or anonymous label;
- timestamp and edited marker;
- content;
- vote and reaction controls;
- moderation or deleted state.

Deleted parent comments remain as tombstones so child context survives.

## 10. Loading, empty, error, and permission states

Every feature page MUST define these states before implementation is considered complete.

### Loading

- use skeletons where shape is stable;
- use inline spinner for isolated actions;
- do not block the whole page for a minor mutation.

### Empty

Explain:

1. what is empty;
2. why it might be empty;
3. what the user can do.

### Error

Show:

- human-readable message;
- retry when safe;
- request ID on unexpected server failure.

Do not expose stack traces.

### Permission denied

Do not render a broken page full of disabled controls. Explain that the user lacks access and provide navigation back.

## 11. Motion

Motion is subordinate to comprehension.

**§0.5 amendment（owner 批准 2026-09-30）**：Landing 与 Student 表面允许 §0.5 动效清单
（GSAP 3：hero 编排、烛尘粒子、计数、路径 scrub、火漆按压/涟漪/磁吸、拖拽、盖印、卷轴弹窗、吐司）。
其余旧规则继续约束 **Teacher/Admin/认证表面**：

Use motion for (staff surfaces):

- drawer and dialog transitions;
- small list insertion or removal;
- optimistic vote and reaction feedback;
- subtle progress and state changes.

Avoid (all surfaces):

- page-load choreography on operational pages（Teacher/Admin 队列、表格）;
- parallax;
- decorative looping animation（烛尘粒子除外——Student 表面白名单项）;
- flashing rarity effects（传说光泽仅 hover 一次）.

硬规则（对 §0.5 动效同样生效）：`prefers-reduced-motion` 下回退为完整静态页，无元素卡在
隐藏态；动效仅 transform/opacity/clip-path/stroke-dashoffset；装饰层 pointer-events:none。

## 12. Accessibility baseline

Target WCAG 2.2 AA behavior for V1.

Requirements:

- semantic HTML first;
- visible keyboard focus;
- keyboard-operable controls;
- dialogs and popovers manage focus correctly;
- icons have accessible names where needed;
- form controls have labels;
- errors are programmatically associated;
- color is never the only state indicator;
- reduced-motion preference is respected;
- pointer and touch targets are comfortably usable, with primary mobile actions around 44px minimum target dimension when feasible;
- contrast is checked in both normal and muted states.

Prefer accessible primitives, for example Radix-backed shadcn components, instead of reimplementing dialogs, menus, popovers, tabs, and focus traps.

## 13. Responsive behavior

Design from workload classes rather than device names.

### Narrow

Student phone use. Prioritize current action and summary. Collapse secondary metadata.

### Medium

Tablet or small laptop. Sidebar may collapse; tables simplify.

### Wide

Teacher and Admin workstation. Use denser tables, split panes, filters, and persistent navigation.

Do not make desktop simply the mobile layout with more whitespace.

## 14. Copy and tone

CampusQuest copy should be concise, calm, and action-oriented.

Prefer:

- 当前可获得 160 积分
- 提交未通过校验，请修正 3 项问题
- 老师已退回修改，奖励档位已保留

Avoid:

- punitive framing such as 你被扣除 40 分;
- game slang in serious review or security flows;
- vague errors such as 操作失败;
- fake urgency.

## 15. Figma workflow

Figma is an optional design workspace, not the runtime source of truth.

Recommended reference frames:

1. Student Dashboard
2. Task Detail and Claim
3. Submission and Validation Report
4. Rankings and Around Me
5. Teacher Review Queue and Review Detail
6. Admin Dense Table and Confirmation Dialog

Figma components and code components should share names where practical.

Design tokens ultimately live in code. If Figma and repository tokens disagree, resolve the intended source deliberately rather than allowing silent drift.

## 16. Visual review checklist

Before frontend work is accepted, inspect at minimum:

- desktop and narrow viewport;
- normal, loading, empty, and error;
- long Chinese nickname and task title;
- long keyword;
- zero and very large point values;
- deadline under 4h;
- revision state;
- keyboard focus order;
- no sensitive identity leakage.

See docs/quality/quality-gates.md for the full gate.
