# 生成工作流参考

> 本文件是 SKILL.md STEP 1-3 的执行细节。SKILL.md 保留步骤概述和指针，具体表格和示例在此查阅。
> **加载方式**：触发 pixel-bloom 时必读全部。

---

## § STEP 0 — 视觉调研

```bash
opencli browser research open "https://www.pinterest.com/search/pins/?q=<英文搜索词>"
opencli browser research state
```

观察形状特征、色彩、光影层次，直接转化为 STEP 2 的拆解决策。

> 若浏览器工具不可用，从用户描述推断形态特征，直接进 STEP 1。

---

## § STEP 1 — 画幅选项与场景模式

1. 让用户选画幅（3:4 / 9:16 / 1:1 / 4:3 / 16:9），用 CSS `aspect-ratio` 锁定
2. 询问是否包含音效（yes / no），含音效时后续需读 `references/audio-engine.md`
3. 确定场景模式（决定后续所有技术决策）：

| 场景模式 | 画布 | 特征 |
|---------|------|------|
| 封闭容器（穹顶/球体/玻璃瓶） | 固定 320×480 | Z-index 三明治 + `transform:scale` 适配屏幕 |
| 开放场景（全视口天空/水下） | 全视口 | 无玻璃壳，CSS 极光光斑漂浮 |
| Ganzfeld（已触发） | 全视口 | 高明度 Aero 光场 + Skyspace 透镜容器 |

---

## § STEP 2 — 像素拆解

将用户描述逐层分解为像素基础元素 + 程序化模型：

```
用户描述
  → 场景模式
  → 主物体：有自主运动（鱼/宠物/史莱姆）→ FSM 三态（Wander/Chase/Flee）+ Perlin noise 驱动
             无自主运动（植物/摆件/珊瑚）→ React-only（交互触发单次反馈动画）
  → 副物体：至少选 2 种 A-D 程序化模型（如 A水草 + C珊瑚礁）
  → 背景：全视口 Aero 渐变 + 极光光斑
```

- 气泡必须用 `drawAeroBubble()`（偏移高光正圆），禁用 `rect()` 圆角替代
- 调色板从 `code-templates.md` 预设数组（WARM / COOL / NEON）取，禁止对单个物体随机 RGB

---

## § STEP 3 — 技术决策

5 项决策，逐一明确后才写代码：

| 决策项 | 选项 |
|--------|------|
| 画布类型 | 固定 320×480 / 全视口 |
| 运动模式 | FSM Wander/Chase/Flee / React-only |
| 玻璃类型 | 穹顶 / 球体 / Skyspace 透镜（Ganzfeld）/ 无 |
| 粒子 | Perlin 漩涡 / 下落 / 拖影残像（Ganzfeld：`fill(10,8,20,12)` 替代 `clear()`）/ 无 |
| 音效 | 交互 chime（默认）/ 174Hz 三角波底噪 / 432+216Hz 颂钵（Ganzfeld） |

> 含音效时：读 `references/audio-engine.md`。AudioContext 在首次用户交互后 `resume()`，masterGain 起始值 0 淡入防爆音。
