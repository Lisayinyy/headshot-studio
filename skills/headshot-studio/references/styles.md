# 套系参数

提示词按 SKILL.md 的四段顺序拼接：Identity lock → Shot → Variant → Retouch & negatives。

## Identity lock（每条必带，原样使用）

```text
Use the person in the reference photo(s) as the only subject. This must be unmistakably the same real person: preserve their exact face shape and jawline, facial proportions, eye shape and size, eyelids, eyebrow shape, nose, lips, ears, skin tone, apparent age, hairline, hair color, and any moles, freckles or distinctive marks. If they wear glasses in the reference, keep the same glasses. Do not slim the face, enlarge the eyes, raise the nose bridge, change ethnicity, or make them look younger or like a different person.
```

## Retouch & negatives（每条必带，原样使用）

```text
Retouching: premium Chinese portrait studio finish — even, luminous skin with natural texture and visible pores kept, temporary blemishes and under-eye shadows softened, flyaway hairs tidied, clothes wrinkle-free, crisp eyes with clear catchlights, slightly bright high-key exposure, clean neutral color grading. Photorealistic photograph, sharp focus on the eyes, 2K detail.
Avoid: plastic or airbrushed skin, heavy makeup, beauty-filter look, distorted hands or extra fingers, asymmetric collar or tie, warped glasses, text, logo, watermark, frame, props, other people, busy or textured background.
```

## business — 商务职业照

Shot：

```text
Professional corporate headshot in a high-end portrait studio. Chest-up framing, subject centered with comfortable headroom, shot on an 85mm lens at f/4 from eye level. Wardrobe: {ATTIRE}. Background: {BACKGROUND}. Lighting: large soft key light from front-left at 45 degrees with a beauty dish feel, soft fill from the right, subtle hair light separating the head from the background, no harsh shadows.
```

- 默认着装 `{ATTIRE}`：`a tailored dark navy or charcoal suit jacket over a crisp white shirt, neat and well-fitted`（男士可加 `with a solid dark tie`；用户未提及领带时不加）。
- 默认背景 `{BACKGROUND}`：`seamless studio backdrop in elegant medium grey with a soft gradient vignette, slightly lighter behind the head`。

Variant（按张数依次取）：

1. `Body turned about 20 degrees to the side, face turned back to the camera, shoulders relaxed, a confident gentle closed-mouth smile.`
2. `Facing the camera squarely, upright posture, a warm natural smile showing a little teeth, friendly and approachable.`
3. `Waist-up framing with arms lightly crossed, body angled slightly, a calm assured expression with a subtle smile.`
4. `Facing the camera, chin slightly down, serious and composed professional expression, direct eye contact.`

## resume — 简历求职照

Shot：

```text
Clean professional resume photo in a portrait studio. Head-and-shoulders framing, subject centered and facing the camera, shot on an 85mm lens at f/5.6 from eye level. Wardrobe: {ATTIRE}. Background: {BACKGROUND}. Lighting: soft, even, shadowless frontal studio light with gentle fill, bright and fresh.
```

- 默认着装：`a well-fitted dark suit jacket over a plain white shirt, formal and tidy`。
- 默认背景：`plain seamless very light grey, almost white, perfectly even`。

Variant：

1. `Facing the camera squarely, natural closed-mouth smile, bright attentive eyes.`
2. `Shoulders angled very slightly, face to camera, a slightly wider friendly smile.`
3. `Facing the camera, neutral pleasant expression with relaxed lips, confident.`
4. `Head tilted very slightly, soft genuine smile, approachable.`

## id — 证件照

Shot：

```text
Formal ID-style portrait photo. Strictly frontal: head and shoulders square to the camera, head level and centered, both shoulders visible, face occupying about 70 percent of the frame height, both ears visible unless covered by hair, eyes open looking straight into the lens. Wardrobe: {ATTIRE}. Background: {BACKGROUND}, perfectly flat with no gradient, no texture and no shadow. Lighting: completely even frontal studio lighting, no shadows on the face or background, no reflections on glasses.
```

- 默认着装：`a dark suit jacket over a white shirt with a neat collar`。
- 背景颜色：白底 `pure white (#FFFFFF) solid color`；蓝底 `standard ID photo blue (#438EDB) solid color`；红底 `standard ID photo red (#D9001B) solid color`。用户没指定时用白底。
- 证件照 Variant 只变表情，姿态一律正面：
  1. `Neutral calm expression, mouth closed, relaxed.`
  2. `Very slight natural closed-mouth smile.`
  3. `Neutral expression, chin very slightly lowered, mouth closed.`
  4. `Gentle pleasant expression, mouth closed, eyes relaxed.`

## profile — 形象照 / LinkedIn 头像

Shot：

```text
Modern personal branding portrait for LinkedIn and personal websites, editorial portrait studio style. Chest-up framing (or tight head-and-shoulders for a 1:1 avatar), subject slightly off-center, shot on an 85mm lens at f/2.8 from eye level. Wardrobe: {ATTIRE}. Background: {BACKGROUND}. Lighting: soft window-like key light from the side with gentle falloff, warm subtle rim light, a lively, polished yet approachable mood.
```

- 默认着装：`smart business-casual — a soft blazer over a light knit top or an open-collar shirt`。
- 默认背景：`softly blurred warm beige studio backdrop with a gentle gradient, creamy bokeh`。

Variant：

1. `Leaning very slightly toward the camera, a bright genuine smile showing teeth, energetic.`
2. `Body turned to the side, face to camera, relaxed confident half smile.`
3. `Hand lightly touching the jacket lapel, calm smile, thoughtful look.`
4. `Facing the camera, warm closed-mouth smile, eyes smiling.`

## 用户选项映射

着装：

- 深色西装 + 白衬衫：`a tailored dark navy or charcoal suit jacket over a crisp white shirt`
- 浅色衬衫或针织，不穿外套：`a well-ironed light-colored shirt or fine knit top, no jacket`
- 保留照片里的衣服：`the same clothing as in the reference photo, cleaned up, wrinkle-free and neatly arranged`

背景（非证件照）：

- 高级灰渐变：`seamless studio backdrop in elegant medium grey with a soft gradient vignette`
- 纯白：`seamless pure white studio backdrop, clean and bright`
- 证件蓝：`seamless studio backdrop in soft professional blue, evenly lit`

## 组合冲突处理

同一条提示词里不能出现互相矛盾的描述。拼接前按下面的规则改写：

- **画幅**：business 的 Variant 3 是 `Waist-up framing`。用它时把 Shot 里的 `Chest-up framing` 一并改成 `Waist-up framing`，不要两种画幅同时出现。
- **证件照背景只能是纯色**：背景题选了"高级灰渐变"时，证件照按白底处理并在交付时说明；选"证件蓝"时用证件照小节的 `standard ID photo blue (#438EDB) solid color`，不要用非证件照的 `soft professional blue`。
- **不穿外套**：着装是"浅色衬衫或针织，不穿外套"，或保留的原衣服没有外套时，profile 的 Variant 3（`Hand lightly touching the jacket lapel`）改为 `Arms relaxed at the sides, calm smile, thoughtful look.`；business 默认着装里关于领带的补充也不要加。
- **1:1 头像**：profile 选头像用途时 Shot 用括号里的 `tight head-and-shoulders` 画幅，画面里放不下手，Variant 3 同样改用上一条的无手势写法。
