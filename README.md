# 学习乐园 Learning Hub

给三年级孩子做的互动学习游戏导航站。数学 / 英语 / 语文，一个网站全部入口。

## 线上地址

```
https://cnuocnuo0026-d6gmy0w5ja740e8e5-1462119982.tcloudbaseapp.com/game/
```

部署到腾讯云 CloudBase 静态托管，**GitHub Actions 自动部署**：推送到 main 分支后约 2 分钟自动上线，无需手动操作。

## 目录结构

```
games-hub/
├── index.html               # 导航首页（Tab 切换学科，游戏卡片入口）
├── math/                    # 数学学科游戏
│   └── fraction-adventure.html    # 分数大冒险（三年级分数初步认识）
├── english/                 # 英语学科游戏
│   └── word-match.html      # 单词消消乐（三年级词汇连连看）
├── chinese/                 # 语文学科（预留，待开发）
└── .github/workflows/deploy.yml   # 自动部署配置（push 即上线）
```

## 如何添加一个新游戏（重要）

1. 游戏 HTML 文件放入对应学科文件夹（如 `math/`、`english/`、`chinese/`）
2. 文件名必须用英文（GitHub Pages / 云托管中文文件名会出问题）
3. 游戏必须是**单 HTML 文件、离线可用**，数据内嵌在 `<script>` 中，不依赖外部资源
4. 在 `index.html` 对应学科的 `<div class="grid">` 里添加一张卡片（参考现有卡片结构）
5. 推送 main 分支，自动部署上线

## 设计硬规则（必须遵守）

- 所有控件（按钮/下拉/输入框）**白底 `#fff`、黑字 `#333`**，不能半透明，不用毛玻璃
- 标题、副标题黑色；EN 文字 `#2c3e9b`（蓝），ZH 文字 `#c44569`（粉），角标 `#4a5fcf`
- 容器纯白无毛玻璃；棋盘/内容区 `justify-content: center` 居中
- 高对比度适配课堂大屏；适配手机 / 平板 / Windows / Mac 多平台
- 计分规则参考：答对 +10，答错 −5，限时 +2/秒；倒计时 60 秒，最后 10 秒红色闪烁

## UI 统一规范（2026-08 沉淀，新游戏照此实现）

所有色值写进 `:root` 变量，禁止散落硬编码。**糖果天空版参考实现：`math/fraction-adventure.html`**；`index.html` 导航首页仍为旧奶油底，后续改版时照此升级。

```css
:root{
  --ink:#333333;      /* 正文黑字 */
  --title:#111111;    /* 标题黑 */
  --en:#2c3e9b;       /* EN 蓝 / 数学 */
  --zh:#c44569;       /* ZH 粉 / 语文 */
  --badge:#4a5fcf;    /* 角标蓝紫 */
  --gold:#f5b301;     /* 星星金 */
  --ok:#2e9e5b;       /* 答对绿 / 英语 */
  --bad:#d64550;      /* 答错红 */
  --card:#ffffff;     /* 卡片白 */
  --page:#fdfbf7;     /* 备用页底（非控件） */

  /* 糖果卡通色板 */
  --pink:#ff8fb2;   --pink-soft:#ffe3ec;
  --orange:#ff9f5a; --orange-soft:#ffe7d1;
  --green:#45c486;  --green-soft:#d9f5e7;
  --purple:#8f7bf0; --purple-soft:#e6e0fd;
  --blue:#4f9dff;   --blue-soft:#dcebff;
  --lemon:#ffd54f;  --lemon-soft:#fff3c9;
  --aqua:#43d4e6;
  --btn-shadow:#ffc6d8;     /* 按钮糖果粉投影 */
  --btn-shadow-blue:#b9d0ff;
  --card-shadow:#e6dfd2;    /* 通用卡片投影 */
}
```

科目色：数学 = 蓝 `--en`（浅底 `#eef1fb`）、英语 = 绿 `--ok`（浅底 `#e7f6ec`）、语文 = 粉 `--zh`（浅底 `#fdecea`）。需要多色彩时优先取糖果色板，禁止凭空发明新色相。

### 组件配方（糖果卡通主题）

- 按钮/选项：白底、`3px` 黑描边、`18px` 圆角、糖果粉硬投影 `0 5px 0 var(--btn-shadow)`；hover 上浮 `translateY(-2px)` + 投影加深为 `0 7px 0`，按下 `translateY(4px)` + 投影缩为 `0 1px 0`；主按钮 `border-color:var(--en)` + 蓝色投影 `var(--btn-shadow-blue)`
- 卡片/形状盒：白底、`3px` 描边、`22px` 圆角、投影 `0 5px 0 var(--card-shadow)`；**关卡/主题卡片用彩色描边 + 彩色投影**（粉/橙/绿/紫四色，类名 `lv1`–`lv4`），hover 抬升 `translateY(-4px)` + 投影加深
- Tab / 主题按钮：药丸形 `border-radius:999px`
- emoji 贴纸格：约 86px 方块、24px 圆角、科目色描边 + 浅色底，hover 轻微旋转放大
- 角标 chip：胶囊形 `border-radius:999px`、实心底白字（EN 用 `--en` 蓝、ZH 用 `--zh` 粉）
- 分数图形切片：糖果彩虹色轮换填充（`#ff8fb2 → #ffd54f → #5fd7a3 → #7fb2ff → #c78aff → #ff9f5a → #4dd0e1`）、`stroke:#fff` 白边分隔、`fill-opacity` 渐显涂色

### 布局规则

- 游戏区对称居中：按钮行、图形区、选项、提示文字、答题反馈全部居中；选项用 `flex` + `max-width:760px`，桌面 3 列居中、手机自动换行
- 大字重（700–900）+ `clamp()` 流式字号，适配手机到课堂投影仪
- 间距节奏：gap 14–18px、卡片 padding 22–28px、区块间隔 22–26px

### 背景配方（糖果天空）

- 页底：白色圆点纹理 + 蓝天到暖色渐变：
  `radial-gradient(#ffffff 1.6px, transparent 1.6px) 0 0 / 26px 26px, linear-gradient(180deg,#7ecdf6 0%,#b4e6fb 38%,#e9f8ff 62%,#fff3d6 100%)`
- 卡通装饰层：`<div class="bg-deco" aria-hidden="true">`（fixed、`inset:0`、`pointer-events:none`、`overflow:hidden`），放 emoji 太阳 ☀️、白云 ☁️、彩虹 🌈、气球 🎈、星星 ⭐/✨/🌟、纸杯蛋糕 🧁，以及两个绿色小山丘（`border-radius:50% 50% 0 0` 渐变块）
- 装饰动效：太阳 `sunPulse`、云朵左右摇摆 `sway`、气球/彩虹/蛋糕上下浮动 `bob`、星星闪烁 `twinkle`，全部只用 `transform/opacity`（性能友好）
- 手机端（`max-width:640px`）：隐藏彩虹、气球、星星、蛋糕、山丘，太阳和云朵缩小，避免遮挡内容
- 内容容器 `.wrap{ position:relative; z-index:1 }` 盖在装饰层上方；装饰不与硬规则冲突：控件始终纯白不透明

### 状态与交互

- 主题切换：`html[data-theme]` + CSS 变量换肤；head 内提前读 `localStorage` 防刷新闪白；默认卡通
- 教学提示组件：`.tip-wrap`（居中按钮 + 卡片）、按钮带 `aria-expanded`、卡片默认 `display:none`、展开时 pop 动效
- 无障碍三件套：`:focus-visible` 描边（`3px solid var(--blue)`）、`prefers-reduced-motion` 关闭动画、Tab 用 `aria-selected`
- 结果页庆祝：大分数用彩虹渐变字（`background-clip:text`，用 `@supports` 兜底纯黑）、`#confetti` 彩带 emoji 随机落下（`confFall`）
- 卡片入场：`cardIn` 上浮渐显，`nth-child` 依次延迟，让页面有节奏感

### 受保护契约（改版时不动）

- 页面链接、文案、Tab 标签、关卡/计分逻辑
- 单 HTML 离线可用，不引 CDN / 外部字体，用系统字体栈
- 推送 main 分支由用户单独指令执行

## 开发约定

- 所有游戏为独立静态 HTML，互不依赖
- 新学科 = 新建文件夹 + 导航页加 Tab + 加卡片
- 修改后推送 GitHub 即自动部署，线上地址不变

## 常用 git 命令（在项目目录下执行）

```bash
git status          # 查看改动
git add -A          # 暂存所有改动
git commit -m "说明" # 提交
git push            # 推送到 GitHub（触发自动部署）
```
