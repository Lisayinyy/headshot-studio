# 海马体职业照（headshot-studio）

MiniMax Code 插件：上传一张自拍或生活照，用 MiniMax H3 Image（`mcode-tools` 的 `connector__matrix__generate_image`）生成影棚级职业照。

## 套系

| 套系 | 适用 | 默认着装 | 默认背景 |
|---|---|---|---|
| 商务职业照 | 企业官网、名片、通讯录 | 深色西装 + 白衬衫 | 高级灰渐变 |
| 简历求职照 | 简历、招聘平台 | 深色西装 + 白衬衫 | 浅灰近白 |
| 证件照 | 简历、工牌、内部系统 | 深色西装 + 白衬衫 | 白 / 蓝 / 红纯色 |
| 形象照 / LinkedIn | 社交头像、个人主页 | 商务休闲 | 暖米色虚化 |

着装、背景、张数（1 / 2 / 4）可在开拍前选择；成片为 2K、3:4（头像可选 1:1），每张生成后逐张质检。

## 计费

活动期内（上线至 11 月 20 日）MiniMax Code 每个账号共 10 次免费图片生成，每张图计 1 次；之后按 H3 Image 正常计费。

## 目录

```text
.minimax-plugin/plugin.json
icon.png
skills/headshot-studio/SKILL.md              工作流：收照片 → 问套系 → 写提示词 → 生成 → 质检 → 交付
skills/headshot-studio/references/styles.md  四个套系的提示词参数、姿态变体与选项映射
```

## 本地安装

```bash
cp -R headshot-studio ~/.minimax/plugins/headshot-studio
mcode plugin list -m local   # 应显示 headshot-studio@local
```

## 说明

AI 生成的证件照不适用于护照、身份证、签证、考试报名等官方证件。
