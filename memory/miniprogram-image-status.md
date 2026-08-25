---
name: miniprogram-image-status
description: 岩涺石小程序图片素材清单：已有CDN直链、缺失图片分类（可AI生成/必须实拍）
metadata:
  type: project
---

## GitHub 仓库

`YnKZ717/yanjushi-miniprogram`（私有库），图片通过 jsDelivr CDN 分发：
`https://cdn.jsdelivr.net/gh/YnKZ717/yanjushi-miniprogram@master/pics/...`

## 已有 CDN 直链（9张）✅

### 怪兽形象图（4张）
- 观火者 — `pics/monsters/观火者.jpg`
- 泥巴怪 — `pics/monsters/泥巴怪.jpg`
- 大地建造者 — `pics/monsters/大地建造者.jpg`
- 野生建筑师 — `pics/monsters/野生建筑师.jpg`

### 怪兽收养计划活动图（5张）
- adopt_tree.png — 女孩抱白怪兽+茶树"小树的茶茶"，民宿管家递龙井茶
- adopt_plant.png — 两人种茶苗+橙色怪兽+"安吉白茶"木牌+茶苗挂"茶"字牌
- adopt_wall.png — 一家三口看满墙彩色怪兽陶塑展示架
- adopt_certificate.png — 阳台收养证书+茶包"ANJI MONSTER TEA BAG"
- adopt_select.png — 女孩选怪兽+"小怪家族欢迎你"墙

## 缺失图片（17张）

###  可 AI 生成（10张）
1. `activities/kiln.jpg` — 窑火与茶，深夜围炉茶会，柴烧茶器
2. `activities/stone.jpg` — 石头的口信，石头上刻字画画后烧制
3. `activities/chai.jpg` — 柴烧集训营，拉坯/揉泥/装窑全流程
4. `activities/improvise.jpg` — 即兴创作营，山野大地为画布
5. `experiences/hand.jpg` — 陶艺手捏体验，双手捏制陶土特写
6. `experiences/wheel.jpg` — 拉坯体验，手在拉坯机上塑造陶器
7. `experiences/tea.jpg` — 罐罐茶下午茶，西北罐罐茶+安吉白茶+点心
8. `rooms/shiqing.jpg` — 石青客房，冷蓝灰色调，落地窗+山景+私汤露台
9. `rooms/chunchen.jpg` — 春辰客房，绿色调，观星天窗+竹林小院
10. `rooms/zheshi.jpg` — 石客房，暖橙色独栋木屋，壁炉+私汤庭院

### 🔴 必须提供实物图片（4张）
11. `rooms/dailan.jpg` — 黛蓝客房，无边泳池景观（真实建筑设施）
12. `rooms/ouhe.jpg` — 藕荷客房，闺蜜房实景（真实房间布局）
13. `mall/mini.jpg` — 迷你怪兽陶塑 128 元（商城实物商品）
14. `mall/big.jpg` — 大只怪兽陶塑 298 元（商城实物商品）

###  建议实拍（3张）
15. `mall/tea-s.jpg` — 白茶茶器礼盒（小），柴烧茶器+安吉白茶 2 罐
16. `mall/tea-l.jpg` — 白茶茶器礼盒（大），完整茶器+安吉白茶 4 罐+怪兽杯垫
17. `mall/postcard.jpg` — 手绘明信片一套 8 张（可用 AI 生成怪兽插画效果图）

## 提示词已就绪
- 怪兽形象 4 套提示词已完成：`{{default_output_dir}}/commercial\result\MBTI怪兽提示词_元素怪兽版.txt`
- 剩余 10 张 AI 可生成图片的提示词尚未编写

## 相关文件
- 数据文件：`{{default_output_dir}}\yanjushi\program\utils\data.js`（所有图片路径定义）
- CDN 直链清单：`{{user_pictures_dir}}\miniprogram\cdn\cdn直链.txt`
- GitHub Issue #3："微信活动配图CDN直链"
- GitHub Issue #2："图片素材 CDN 外链"（保留的旧版）
