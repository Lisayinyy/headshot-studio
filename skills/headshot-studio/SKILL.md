---
name: headshot-studio
description: "海马体风格 AI 职业照（MiniMax H3 Image）。用户想把自拍、生活照做成职业照、商务照、简历照、求职照、证件照（白底/蓝底/红底）、LinkedIn 头像、个人形象照，或提到海马体、影棚照、精修大头照时使用；以用户照片为参考图调用 H3 Image 生成保留本人五官的影棚成片，并逐张质检后交付。"
---

# 海马体职业照

把用户的一张自拍或生活照，拍成影棚级职业照。生成模型是 MiniMax H3 Image，用户照片作为参考图传入，调用方式见第 4 步。

核心原则：**像本人，比本人状态好**。五官、脸型、年龄、肤色、发际线、痣和眼镜都保留；只做影棚会做的事——布光、换装、换背景、整理发丝、自然磨皮提亮。

## 1. 收照片

1. 用户附图或给出路径。照片不在当前工作区内（如下载目录、对话附件的临时目录）时，先复制到工作区 `headshots/source/`，文件名用 ASCII，之后都引用复制后的相对路径。
2. 用户只把图贴在对话里、没有可读的本地文件时，请用户把图片文件拖进来或给出路径；不要用文字描述代替参考图。
3. 自己看一遍照片（你能直接看图，不要调用 `images_understand`）：
   - 多人合照：问清楚拍哪一位，并在提示词里写清其位置和衣着特征。
   - 脸被遮挡、严重侧脸、极暗、强美颜滤镜、分辨率很低：直说会影响像不像，建议换一张正脸、自然光、不开美颜的照片；用户坚持就继续。
   - 用户给了多张本人照片（不同角度）：最多选 3 张最清楚的一起传入，像本人的程度会更高。
4. 只处理用户本人或已获本人同意的照片；不生成公众人物、不把 A 的脸换到 B 身上。

## 2. 问套系（一次问完）

用户已经说清楚的项不再问；用户说"你看着办 / 直接拍"时全部用推荐项，并把采用的选择列给用户。否则调用一次 `ask_user`（`async: false`），最多 4 题，每题最多 4 个选项，用 `recommended` 标推荐项，不要在 label 里写"推荐"：

| 题 | 选项 |
|---|---|
| 拍哪种套系？ | 商务职业照（推荐）· 简历求职照 · 证件照 · 形象照 / LinkedIn 头像 |
| 穿什么？ | 按套系搭配（推荐）· 深色西装 + 白衬衫 · 浅色衬衫或针织，不穿外套 · 保留照片里的衣服，只整理平整 |
| 背景？ | 按套系推荐（推荐）· 高级灰渐变 · 纯白 · 证件蓝 |
| 拍几张？ | 1 张 · 2 张不同姿态（推荐）· 4 张（表情 + 角度组合） |

"拍几张"的题干里写明：活动期内 MiniMax Code 每个账号共有 10 次免费图片生成，每张图计 1 次，用完后按 H3 Image 正常计费。证件照用户要红底等其他颜色时，按用户回答的颜色走。

各套系的着装、背景、光线、表情和姿态参数见 `references/styles.md`，选定后读取对应小节。

## 3. 写提示词

每张图一条 request。提示词用英文写（模型对英文摄影术语更敏感），按下面四段顺序拼接，内容取自 `references/styles.md`：

1. **Identity lock**（每条必带，原文照抄 styles.md 里的段落）。
2. **Shot**：套系的画幅、机位、着装、背景、布光。
3. **Variant**：这一张的姿态和表情（多张时每张取不同变体）。
4. **Retouch & negatives**：海马体式自然精修与禁止项（原文照抄）。

用户补充的具体要求（比如"头发扎起来""戴眼镜""不要露齿笑"）写进 Shot 或 Variant，并且优先于套系默认值。

## 4. 生成

生图走 `mcode-tools` 的 Connector 工具 `connector__matrix__generate_image`（MiniMax H3 Image）。不要用 `tool_search` 找生图工具，按下面步骤直接做：

1. 加载 `mcode-tools-master` Skill（运行任何 `mcode-tools` 命令前必须先加载）。
2. 上传参考图拿到临时 URL（1 小时有效，同一轮对话内复用，重拍超过 1 小时要重新上传）：

   ```bash
   mcode-tools upload_temp_url headshots/source/selfie.jpg
   ```

   取返回 JSON 的 `temp_url`。
3. 把全部张数写进一个参数文件 `headshots/<YYYYMMDD-HHMM>/args.json`，一次调用提交：

   ```json
   {
     "requests": [
       {
         "prompt": "<四段提示词>",
         "input_urls": ["<temp_url>"],
         "output_file": "business-1",
         "aspect_ratio": "3:4",
         "resolution": "2K"
       }
     ]
   }
   ```

   ```bash
   mcode-tools connector call connector__matrix__generate_image --args-file headshots/<YYYYMMDD-HHMM>/args.json
   ```

   - 字段只有 `prompt`、`input_urls`、`output_file`、`aspect_ratio`、`resolution`；不要传 `input_files`、`output_path`。
   - `output_file` 用 `<套系英文名>-<序号>`，套系英文名为 `business` / `resume` / `id` / `profile`；时间取当前本地时间。
   - `aspect_ratio` 默认 `3:4`（半身），形象照选"头像"用途时用 `1:1`。`resolution` 固定 `2K`。
   - 生成 2–4 张通常需要 1–3 分钟，命令超时至少给 600 秒。
4. 结果里 `success_items[]` 给出 `node_id` 和 `file_name`（扩展名以它为准，通常是 `.jpg`）。逐个下载到成片目录：

   ```bash
   url=$(mcode-tools get_asset_url <node_id> | python3 -c 'import sys,json;print(json.load(sys.stdin)["download_url"])')
   curl -sSL "$url" -o "headshots/<YYYYMMDD-HHMM>/<file_name>"
   ```

   不要自己拼 OSS/CDN 地址。
5. `failed_items[]` 里写着"request failed; please retry"这类临时错误时，只把失败的那几条 request 重新提交一次，成功的不重复生成。内容审核、额度用尽、参考图无法读取等错误如实告诉用户，不要换用其他生图工具或网上图片充数。

当前会话的工具列表里直接有 `image_synthesize` 时，也可以改用它：参数同样放在 `requests` 数组里，参考图用工作区相对路径 `input_file_paths`，成片位置用 `output_file_path`（如 `headshots/<YYYYMMDD-HHMM>/business-1.png`），其余字段和上面一致，工具会直接把图片存到该路径。

## 5. 逐张质检

生成后用 `read` 打开每张成片，和原照片对比检查：

- **像不像**：脸型、眼型、鼻子、嘴型、眉形、年龄感、肤色、痣；不能变成"网红脸"或明显变瘦变年轻。
- **硬伤**：多余或扭曲的手指、领口/领带/纽扣左右不对称、耳朵或眼镜变形、牙齿粘连、头发与背景交界发糊、背景出现文字或水印。
- **套系符合度**：证件照必须正脸、双耳可见（头发不遮挡时）、无阴影纯色底、表情平和。

有硬伤或不像本人的那张，告诉用户问题在哪，询问是否重拍这一张（重拍同样计 1 次），重拍时在提示词里针对问题加约束；不要未经用户同意自动重拍。

## 6. 交付

用一张表列出成片：文件路径、姿态/表情、质检结论。然后补充：

- 成片为 2K、3:4，可直接用于简历、招聘平台、企业通讯录和社交头像。
- 证件照成片仅适合简历、工牌、内部系统等场景；护照、身份证、签证、考试报名等官方证件对照片真实性和规格有强制要求，需到有资质的照相馆拍摄或按官方要求核验，不要把 AI 生成照片当作官方证件照提交。
- 想换套系、换背景或再要几张，直接说即可（继续计次）。
