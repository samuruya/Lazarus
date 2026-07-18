# Charon — Design Snapshot

## Overview
Retro pixel-art gaming/community platform with a dark CRT-inspired palette, sharp edges, and punchy arcade-style motion.

## Colors
- Background: `#0d0d0f`
- Panel / surface: `#1c1c20`, `#26262b`
- Border: `#3a3a40`, `#4d4d54`
- Text: `#f3efe9`
- Accent: `#e23b3b`
- Accent bright: `#ff5a4d`
- Accent deep: `#8f1f1f`
- Accent soft: `#ff8a6b`
- Muted: `#b7b2ad`
- Ghost: `#6e6e76`

## Border Radius
- All elements: `0` (sharp, pixel-perfect edges)

## Buttons
- Font: `Press Start 2P`, 10px
- Primary: background `var(--accent)`, color `#fff`, border `2px solid #000`, padding `12px 16px`, box-shadow `3px 3px 0 rgba(0,0,0,.55)`
- Hover: background `var(--text)`, translate `(-1px,-1px)`, box-shadow `4px 4px 0 rgba(0,0,0,.6)`
- Active: translate `(3px,3px)`, box-shadow `0 0 0 #000`
- Secondary: background `var(--panel-2)`, color `var(--text)`, border-color `var(--border)`
- Small variant: padding `10px 12px`, font-size `9px`

## Cards
- Background: `var(--panel)`, padding `18px`, border `2px solid var(--border)`, box-shadow `4px 4px 0 rgba(0,0,0,.6)`
- Background image: dither pattern (`repeating-linear-gradient(45deg, rgba(255,255,255,.025) 0 2px, transparent 2px 4px)`)
- Corner accents: `::before`/`::after` pseudo-elements — 12px squares with `3px solid var(--accent)` border
- Large variant: `grid-column: span 2`, min-height `200px`
- Header: `font-size 11px`, border-bottom `2px dashed var(--border-light)`

## Inputs, Pills, Tabs
- Input: background `#15172a`, border `2px solid var(--border)`, box-shadow `inset 2px 2px 0 rgba(0,0,0,.45)`, padding `12px`, font-size `10px`
- Focus: border-color `var(--accent)`, box-shadow `inset ... , 0 0 0 2px rgba(255,90,77,.35)`
- Tab: background `var(--panel-2)`, border `2px solid var(--border)`, padding `10px 18px 12px`, box-shadow `3px 3px 0 rgba(0,0,0,.55)`
- Tab active: background `var(--accent)`, color `#fff`, border `#000`, inset highlight
- Pill: background `var(--panel-2)`, border `2px solid var(--border)`, padding `8px 12px`, font-size `9px`

## Typography & Spacing
- Font: `'Press Start 2P'`, `'Courier New'`, monospace
- Scale: body `10px`, html `14px`, headings `14px`–`24px`
- Spacing: grid gaps `14px`–`16px`, card padding `18px`, panel padding `16px 18px 22px`
- Line-height: `1.9`
- Text rendering: `pixelated`, no font smoothing

## Motion
- Transitions: mostly `0.15s ease` for hover/active states
- Keyframes: `bootIn`, `slideIn`, `fadeIn`, `flameFlicker`, `spriteTrain`, `xpFloat`, `xpGain`, `xpMote`, `groundScroll`, `platformPulse`, `auraPulse`, `levelFlash`
- Easing: `ease`, `ease-in-out`, `cubic-bezier(.2,.8,.3,1)`
- Everything is crisp and punchy — no long, smooth animations

## Notes
This is a cohesive retro-arcade identity: dark CRT background, flame-red accent, sharp corners, dither overlays, and pixel-perfect motion.

Back to [[main.md]]
