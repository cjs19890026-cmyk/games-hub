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
