# 依赖与离线说明

本站在 `file://` 下可直接打开，不依赖任何 CDN 或外部请求。

| 依赖 | 位置 | 说明 |
|---|---|---|
| KaTeX 0.16.9 | `vendor/katex/` | 公式渲染（JS/CSS/字体全部本地内置） |
| 内联 SVG | 数据驱动生成 | 所有图由 `tools/figs.py` 生成，直接写进 HTML，无外部图片 |
| 领域数据 | `data/domains/<id>/*.json` + `bundle.js` | 每个领域独立打包；`data/manifest.js` 是领域清单 |
| 站点入口 | `index.html` | 单页应用；进入领域时才加载该领域的 `bundle.js` |

## 为什么不是单个 `data/bundle.js`

七个领域合计 3000 万字正文。若打成单文件，首屏要同步解析近百 MB，弱机不可用。
因此按领域拆分：首屏只读 `manifest.js`（几 KB），点进某个领域才加载
`data/domains/<id>/bundle.js`（约 10–20 MB）。`tools/bundle_data.py` 负责打包。

## 验收

`tools/verify_site.py` 仍按单领域验收：`tools/verify_domain.sh <id>` 会把该领域数据
镜像到临时目录再跑全部闸门（字数、引用、图/表/公式、bundle 一致性）。
