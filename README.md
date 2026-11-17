# 引力回响 · Gravity Echo

一款深空风格的桌面引力弹射小游戏：牵引光点，瞄准同色弧环，在生命耗尽前争取最高分。

游戏本体是单个 `index.html`。所有画面由 Canvas 实时绘制，不依赖图片、外部字体或第三方库。

![引力回响游戏画面](screenshots/gameplay.png)

## 玩法简介

**目标：** 用引力牵引光点并弹向同色弧环，在生命耗尽前刷新高分。

**规则：** 吸附的光点越多，得分倍率越高，但核心收缩更快、过载风险也越大。

- 光点会持续向核心漂移，未被弹射而进入核心会导致核心过载，生命 -1。
- 弹射命中同色弧环得分；命中异色弧环扣 1 点生命。
- 连续 3 次命中同色弧环触发「共振」，全场光点减速 2 秒。
- 分数越高，光点生成越快、颜色种类越多，目标环旋转也会逐渐加快。

## 操作

| 输入 | 作用 |
| --- | --- |
| 按住鼠标左键 | 展开引力场，吸附附近光点 |
| 按住时移动鼠标 | 调整吸附光点的角度与弹射方向 |
| 松开鼠标左键 / 空格 | 将吸附的光点反向弹射 |
| `R` | 立即重新开始 |
| 点击画面 | 跳过开场 / 结算后重新开始 |

游戏面向桌面浏览器，不支持手机或触屏操作。推荐在 1280×720 及以上分辨率下游玩。

## 特色

- 深空蓝黑渐变、星点视差、呼吸核心与柔和辉光
- Canvas 绘制的自定义准星与吸附轨迹
- 实时弹射预判线，提前判断是否会命中同色弧环
- 多光点蓄力倍率，与核心过载风险形成取舍
- 同色三连触发「共振」全场减速
- 随机目标环初始角度、随分数递增的难度与颜色种类
- 粒子拖尾、得分飘字、结算面板与本地最高分
- 单个 HTML 文件，完全离线可玩

## 本地运行

直接双击打开 `index.html` 即可，无需安装或构建。

也可以启动一个本地静态服务器：

```bash
python -m http.server 8080
```

然后访问 `http://localhost:8080/`。

## 部署到 GitHub Pages

本目录已经初始化为 Git 仓库，并包含首次提交。

1. 在 GitHub 新建一个空仓库，不要勾选初始化 README。
2. 在本目录添加远程仓库并推送：

```bash
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git push -u origin main
```

如果拿到的是不含 `.git` 的纯文件副本，先初始化再推送：

```bash
git init -b main
git add .
git commit -m "feat: add Gravity Echo"
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git push -u origin main
```

3. 打开仓库的 **Settings → Pages**。
4. 在 **Build and deployment** 中选择 **Deploy from a branch**。
5. 选择 `main` 分支和 `/ (root)` 目录，保存。
6. 稍等片刻，访问 `https://<你的用户名>.github.io/<仓库名>/`。

因为游戏入口就是根目录下的 `index.html`，不需要额外的构建步骤，也不需要 GitHub Actions。

## 项目结构

```text
.
├── index.html              # 完整游戏，内联 CSS / JS
├── README.md
├── .gitignore
├── .gitattributes
└── screenshots/
    ├── intro.png           # README 展示图
    └── gameplay.png        # README 展示图
```

`screenshots/` 只用于项目展示，游戏运行不依赖其中任何文件。

## 技术实现

- 原生 HTML / CSS / JavaScript，无构建流程
- Canvas 2D + `requestAnimationFrame` 主循环
- 状态机：`start / playing / over`
- 响应式 Canvas，高 DPI 渲染上限为 2 倍
- 光点上限 16、粒子上限 240、涟漪上限 18，避免长时间游玩时性能下降
- 最高分保存在 `localStorage`，键名：`gravity-echo-best-v1`
- 所有游戏图形均由 Canvas 矢量路径和渐变绘制，无贴图
