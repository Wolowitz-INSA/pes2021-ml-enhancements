# Community r4 — 发布说明 / Release notes

## 中文

请先阅读[中文使用说明](社区使用说明-中文.md)。包内包含安装与卸载脚本、六个运行模组、年度补强与战术离线工具、中英文说明及第三方许可。队友 AI 甲、乙只能选择一个，因此默认安装五个模组。

- 修复队友 AI 页面文字错乱和切换时强退：两版均长期保存已注册的回调，避免垃圾回收后调用失效函数。主菜单页面显示与反复切换已经实测通过；这不代表全部比赛行为已完成验证。
- 修复攻守等级与队友 AI 使用 Sider 未开放的 Lua 函数而无法加载的问题。更新后必须完整退出并重启游戏和 Sider。
- 队友 AI 甲用于固定球员，乙用于整队切人。默认局部等级为 6，可改为 5；覆盖部分持球、传球和盘带决策，不等于全面替换整套 AI，部分判断对 5、6 的处理相同。
- 攻守等级自动识别玩家球队；社区默认使用中性实力，配置与实力表使用方法见指南。
- 即时赛果默认弱化球队精神与队长加成、启用可调概率曲线，并使用双方实际首发的当前注册位置总评均值。它不是按临场所踢位置重新计算的总评。
- 对手阶段阵型修正与原生按能力选人默认开启。前者不冻结正常跑动，后者仍依赖游戏调用原生自动选人路径。
- 年度补强提供 Public 动态前 16 强版，以及需要核对球队内部引用号的固定豪门模板。默认保护当前玩家球队与其外租球员，输出独立候选存档。

离线工具首次运行前须在所选 JSON 配置中填写自己的数据库路径。Public 动态选择补强对象，但不自动兼容所有存档格式；目前离线工具要求所支持的 575 俱乐部布局。运行模组也保留原生代码特征和身份检查。遇到不匹配须停止使用对应功能，不要绕过检查。

功能可用于符合条件的现有存档，无需为安装本包重开档。完整比赛中的效果、整队切人时序、候选档进入游戏后推进日期及多赛季平衡仍需试玩验证。包内没有个人存档、运行采样、游戏程序或名单数据库。

## English

Start with the [English guide](Community-Guide-English.md). This package includes installation and removal scripts, six runtime modules, offline recruitment and tactics tools, bilingual instructions and third-party licenses. The two teammate AI editions are alternatives, so the default installation loads five modules.

- Fixed corrupted teammate AI overlay text and crashes when switching pages. Both editions now retain their registered callbacks to prevent calls to garbage-collected functions. Main-menu display and repeated switching have passed a live test; this does not validate every gameplay effect.
- Fixed mentality and teammate AI startup failures caused by Lua functions unavailable in Sider. Fully exit and restart both the game and Sider after updating.
- AI edition A is for fixed-player control; edition B is for full-team switching. The default local tier is 6, configurable to 5. It covers selected ball, pass and dribble decisions, not the entire AI. Some decisions treat tiers 5 and 6 identically.
- Mentality control identifies the player's club automatically. Community defaults use neutral strength; see the guide for configuration and strength tables.
- Instant results reduce spirit and captain contributions by default, use an editable probability curve, and use the actual starting XI's current registered-position OVR average for each side. This is not a recalculation for the position played in the match.
- Opponent phase-shape correction and native By Ability selection start enabled. The former does not freeze ordinary movement; the latter still depends on the game's native automatic selection calls.
- Annual recruitment offers Public selection of the strongest 16 non-player clubs, plus fixed elite-club templates whose internal references must be checked against the user's roster. It protects the current player's club and outgoing loans and writes a separate candidate save.

Before first use, configure your database paths in the selected offline JSON profile. Public selects recruitment targets dynamically; it does not make every save format compatible. Offline tools currently require the supported 575-club layout. Runtime modules retain native code-signature and identity checks. Stop using a feature when those checks fail; do not bypass them.

Eligible existing careers are supported; installing this package does not require a new career. Full-match effects, controller-switch timing, in-game candidate-save date advancement and multi-season balance still need playtesting. No personal saves, runtime captures, game executables or roster databases are included.
