![Uploading preview.png…]()
[README.md](https://github.com/user-attachments/files/34638205/README.md)
# HELIOS · 天穹轨道站

一座完全使用 WebGL / Three.js 程序化生成的电影级轨道空间站。无需外部 3D 模型，所有结构、材质、光照、星空、行星与界面均在浏览器中实时构建。

![Uploading preview.png…]()

## 启动

双击 **`start.bat`**，浏览器会自动打开：

```
http://127.0.0.1:8123
```

> 由于使用 ES Module + 本地贴图，必须通过 HTTP 服务打开，不能直接双击 `index.html`。
> 没有 Node.js 时，也可在本目录运行 `python -m http.server 8123`，再访问上面的地址。

## 操作

| 操作 | 说明 |
| --- | --- |
| 鼠标拖拽 | 环绕观察 / 改变视角 |
| 滚轮 | 拉近 / 拉远 |
| 顶部导航 / 右侧 VIEWPORT | 空间站、地球、太阳系、结构拆解、自由驾驶 |
| 进入驾驶舱 | 第三人称驾驶天穹站本体飞行；鼠标环绕观察，滚轮拉近拉远，行星位置与距离常驻 |
| 点击空间站节点 | 查看模块档案并飞近 |
| 自动巡航 | 30 秒环游太阳系，依次经过地球、太阳与全部行星 |
| 巡航速度滑杆 | 0.2×–3× 调整巡游速度，1× 为 30 秒一圈 |
| W / S | 前进 / 倒退 |
| A / D | 左右平移 |
| Q / E | 下降 / 上升 |
| 方向键 | 转向 / 俯仰 |
| Shift | 加速 |
| F | 跃迁冲刺（5 秒冷却） |
| 结构拆解 | 608 个部件沿结构法线自动分离，可随时重组 |
| 时间流速滑杆 | 加速 / 减慢行星、地球、空间站环体的运动 |
| 暂停按钮 | 冻结时间 |
| 左下“系统档案” | 打开世界观设定 |
| 右上声音图标 | 开启 / 关闭程序化环境音 |
| 右上全屏图标 | 进入全屏 |
| `Enter` / `Esc` | 进入空间站 / 关闭面板 |
| `1` `2` `3` `4` `5` | 空间站 / 地球 / 太阳系 / 拆解 / 驾驶 |
| `Space` / `F` / `M` | 暂停 / 全屏 / 环境音 |

顶部导航提供 5 个模式：

1. **空间站** — 天穹站主视角，跃迁门、重力居住环、太阳能翼与地球同框。
2. **地球** — 近距离观察昼夜线、云层、海洋高光与城市灯光。
3. **太阳系** — 拉远到黄道面，观察八大行星、轨道、土星环与小行星带。
4. **结构拆解 / EXPLODED ASSEMBLY** — 608 个部件分阶段自动分离，可随时重组。
5. **自由驾驶 / FREE FLIGHT** — 第三人称驾驶天穹站本体：鼠标锁定环绕，滚轮调整距离，WASD 平移、Q/E 升降、Shift 加速、F 跃迁；太阳与八大行星的位置和距离实时标记在 HUD 上。

空间站面板里的 **自动巡航** 仍可启动 30 秒连续行星航程。

## 更多画面

**地球 / EARTH**

![earth](./screenshots/earth.png)

**太阳系 / SOL SYSTEM**

![system](./screenshots/system.png)

**自由驾驶 / FREE FLIGHT**

![game](./screenshots/game.png)

**自动巡航 / GRAND TOUR**

![tour](./screenshots/tour.png)

**结构拆解 / EXPLODED ASSEMBLY**

![explode](./screenshots/explode.png)

## 技术构成

- **Three.js r170**：WebGL 渲染、PBR 金属材质、自定义 GLSL 着色器。
- **EffectComposer + UnrealBloom**：电影级泛光与 ACES 色调映射。
- **地球**：实时昼夜混合、城市灯光、云层阴影、海洋高光、大气 Fresnel 边缘。
- **太阳系**：程序化太阳表面、日冕、行星轨道、土星环 shader、小行星带粒子。
- **空间站**：608 个程序化部件——中央龙骨、桁架、双环居住舱、太阳能翼、散热器、对接港、天线、跃迁门、引擎、导航灯、无人机。
- **巡航 / 拆解**：30 秒行星航程、曲速线、FOV 冲击、模块级 + 零件级爆炸视图、重组动画。
- **界面**：原生 HTML / CSS / JS，无框架；3D 锚点投影到 HUD，标签自动避让。
- **音效**：Web Audio 实时合成的低频环境声，无音频文件。

## 文件结构

```
space-station/
├─ index.html
├─ start.bat
├─ server.mjs
├─ css/style.css
├─ js/
│  ├─ main.js        # 场景、镜头、HUD、交互
│  ├─ earth.js       # 地球着色器与月球
│  ├─ solar.js       # 太阳系、行星、小行星带
│  ├─ station.js     # 空间站程序化建模
│  └─ assets.js      # 贴图加载
├─ assets/textures/  # 地球、月球、行星贴图
├─ vendor/three/     # 本地 Three.js 与 addons
└─ favicon.ico
```

## 调整入口

- 空间站位置：`js/main.js` 中的 `STATION_OFFSET`
- 视角机位：`js/main.js` 中的 `getView()`
- 地球外观：`js/earth.js` 的 `earthFragment`
- 行星轨道 / 尺寸：`js/solar.js` 顶部的 `PLANETS`
- 品牌文案 / HUD：`index.html`

## 兼容性

建议使用 Chrome / Edge / Firefox 最新版，开启硬件加速。场景针对独立显卡优化；集成显卡如出现掉帧，可把 `js/main.js` 中 `renderer.setPixelRatio(...)` 的上限从 `1.8` 调到 `1.25`，并降低 `UnrealBloomPass` 分辨率。

---

**HELIOS PROGRAM / PHASE 07**  
*人类第一座恒星际跃迁枢纽*
