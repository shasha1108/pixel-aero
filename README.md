# Pixel Bloom · 像素生命的绽放

> **Claude Code skill for pixel art + Frutiger Aero luminous interactive H5 pages.**
> Cyber pets, cyber plants, digital aquariums, open sky gardens, underwater scenes — wherever pixel life grows and glows.
> **赛博养宠、赛博养花、电子水族箱、天空花园、水下世界——像素生命绽放光芒的地方。**

## What this does

Generates complete standalone HTML pages combining:
- **p5.js pixel rendering** — `pixelDensity(1)` + `noSmooth()`, sharp grid-aligned sprites
- **Frutiger Aero luminous world** — Aero gradients, glassmorphism (optional container), ambient light orbs, Ganzfeld light fields
- **Procedural flora** — 4 algorithmic models (stack-sway, grid-cull, radial-domain, fan-spread) that grow and sway
- **AI FSM creatures** — Wander / Chase / Flee state machine with Perlin noise motion — life that moves and reacts
- **Touch rituals** — tap feed, double-tap interact, drag soothe; custom pointerdown debounce
- **Web Audio synthesis** — healing bowls, ambient drones, chord pads, binaural beats — all code-synthesized, zero audio files

Three scene modes: closed container (glass terrarium), open viewport (sky/underwater), and Ganzfeld (immersive light field).

## Quick start

```
/pixel-bloom 帮我做一个像素多肉盆栽，阳光透过玻璃洒下来
```

## Project structure

| File | Purpose |
|------|---------|
| `SKILL.md` | Core workflow — role, boundary rules, architecture constraints, 6 generation steps |
| `references/design-principles.md` | Decision principles — motion laws, materials, anti-patterns, Ganzfeld mode |
| `references/code-templates.md` | Code library — defensive skeleton, procedural models A-D, FSM, interaction templates, color palettes, 15-item quality checklist |
| `references/audio-engine.md` | Web Audio synthesis recipes — zero audio files |

## 🔗 More from @shasha1108

| Repo | What |
|------|------|
| [healing-visual-lab](https://github.com/shasha1108/healing-visual-lab) | 交互式数字疗愈作品集——15 件 Three.js/WebGL 交互实验 |
| [healing-space](https://github.com/shasha1108/healing-space) | 触觉驱动的交互式疗愈 H5 生成器——GPU 流体、WebGL 着色器 |
| [inner-voice](https://github.com/shasha1108/inner-voice) | 小红书情绪内容创作——隐喻挖掘、场景写作、视觉叙事 |
| [h5-publish-skill](https://github.com/shasha1108/h5-publish-skill) | 一键发布 H5 到 GitHub Pages——拖入文件夹即上线 |

<p align="center"><sub>Source code under <a href="LICENSE">MIT License</a></sub></p>
