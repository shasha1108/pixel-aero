# Pixel Aero 通用方法论

> 不穷举场景。掌握这些原则，任意新场景都能自主做对决策。

## 一、架构

### 固定画布
像素艺术用固定像素尺寸（320×480 或类似），`transform:scale()` 适配屏幕。不传 `windowWidth/windowHeight`。

```javascript
const CW=320,CH=480;
createCanvas(CW,CH);noSmooth();
```

`noSmooth()` 在固定画布上单独生效（不需要 `pixelDensity(1)`，高 DPI 下后者反而可能发虚）。

### CSS Z-index 优于 JS
p5.js 会覆盖 JS 设置的 canvas style。固定画布场景用 CSS `!important`。

```css
canvas{position:absolute!important;z-index:7!important;pointer-events:none}
```

### 玻璃容器三层分离
```
背景层(fixed,全视口) → 毛玻璃底板(z:6,仅此层有blur) → Canvas(z:7,锐利) → 玻璃壳(z:8,无blur)
```
**铁律：blur 只在底板，永远不在 canvas 上层。**

容器尺寸固定时，穹顶/球体的 border-radius 用精确数学值：
- 穹顶 `border-radius: <玻璃宽度的一半>px <玻璃宽度的一半>px 0 0`
- 球体 `border-radius: 50%`（正方形元素）

### 无玻璃的开放场景
全视口天空渐变 + `position:fixed` 极光光斑 + canvas 填满视口。无玻璃壳层。

---

## 二、形状

### 地形用数学公式
椭圆、正弦、抛物线——精确可控。

```javascript
// 半椭圆岛屿
for(let dy=0;dy>-h;dy-=PX){
  let lim=round(sqrt(1-pow(dy/h,2))*r);
  for(let dx=-lim;dx<=lim;dx+=PX){/*...*/}
}
```

### 程序化模型（按需选用，至少一种）
| 模型 | 适用于 |
|------|--------|
| A·堆叠摇摆 | 树干、茎、水草、柳条 |
| B·网格剔除 | 灌木、云朵、海绵、草丛 |
| C·圆域筛选 | 花蕊、珊瑚、蘑菇伞、多肉 |
| D·扇形展开 | 棕榈叶、海扇、花瓣 |

**像素拆解法：** 任意目标物体 → 分解为基础几何（方块/圆/线/三角）+ 选配 A-D 模型 + 调色板数组。

---

## 三、运动

### 像素呼吸：方向符号推，禁止 scale()
```javascript
// 边缘块沿自身方向外推 1PX
if(breathe>.5){ox=(b.dx>0?1:-1)*PX;oy=(b.dy>0?1:-1)*PX}
```
蘑菇/荧光体等不适合推位置 → 颜色明暗交替：`alpha=map(breathe,-1,1,150,255)`

### FSM 状态机
- 有自主运动（宠物/鱼）→ Wander/Chase/Flee 三态 + Perlin noise 驱动
- 无自主运动（植物/摆件）→ React-only（交互触发单次反馈）

### Perlin 粒子
```javascript
let ang=noise(s.x*.01,s.y*.01,frameCount*.02)*TWO_PI*2;
s.vx+=cos(ang)*.1;s.vy+=sin(ang)*.1;
s.vx*=.95;s.vy*=.95;  // 阻尼
s.vy-=.02;             // 微重力
```

### 双圈发光渲染
```javascript
blendMode(SCREEN);
fill(100,200,255,a*.3);circle(x,y,sz*4);   // 外光晕
fill(255,255,255,a);   circle(x,y,sz*1.5); // 内亮点
blendMode(BLEND);
```

---

## 四、材质

### 玻璃光泽
- 边框不等宽（上 3px 厚 / 下 1px 薄）
- `::after` 对角白色渐变高光
- `inset` 内发光（厚度感）
- 水珠挂载到玻璃 DOM 元素内：`dome.appendChild(drop)`

### 水珠透镜
```css
.water-drop{backdrop-filter:brightness(1.2) contrast(1.3) blur(2px);
  box-shadow:inset 2px 2px 4px rgba(255,255,255,.8),0 5px 5px rgba(0,0,0,.1)}
```

### 底座质感
3 段渐变（亮面→固有色→暗面）+ 强外投影 + 顶部高光线 = 物体重量感。

---

## 五、交互

### 单击/双击分离模板
```javascript
let lt=0,to;layer.addEventListener('pointerdown',e=>{
  let n=Date.now();if(n-lt<300){clearTimeout(to);onDouble();lt=0}else{lt=n;to=setTimeout(onSingle,300)}
});
```
不用 p5 内置双击事件。

### 反馈要求
交互至少产生一种视觉反馈（涟漪/粒子/颜色变化/缩放）和一种听觉反馈（Web Audio 合成音效）。

---

## 六、色彩

- 预设调色板数组，不为每个物体独立 random RGB
- Frutiger Aero 底色：`linear-gradient(160deg,#a8e6f8,#cdf0fa,#e4f8f0,#f4fcf9)`
- 场景内 2-3 套调色板统一色调

---

## 七、音效

所有声音 Web Audio API 纯代码合成，零音频文件。
- AudioContext 在首次用户交互时初始化并 resume
- masterGain 起始值 0，linearRampToValueAtTime 淡入防爆音
- 174Hz 三角波 drone 作为治愈底噪（可选）
- 交互反馈音效：sine wave pop/chime
