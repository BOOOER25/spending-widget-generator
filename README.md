# 小组件效果生成

无需登录，按「填我的 → 填对方 → 保存图片」依次操作，配置双方头像、ID、消费与商品图片，分别生成 680×680 PNG。参考已确认的淡粉 / 奶黄视觉稿，支持图片裁切、原图模式、透明贴纸、白色描边和柔和阴影。演示照片为 AI 生成的示例素材；用户上传的照片不经过图片生成服务。

## 免费抠图

采用 [IMG.LY background-removal-js](https://github.com/imgly/background-removal-js) 1.7.0，使用 `isnet_quint8` 模型，在浏览器 Worker 中运行。首次使用从 IMG.LY 官方 CDN 下载约 40 MB 模型及运行资源。CPU 性能、WebView 能力与 CDN 网络影响实际速度；失败时可重试或上传已有透明 PNG。没有付费 API 调用，不上传商品图片。此测试方案不承诺第三方 CDN 的长期可用性。

本项目在 AGPL-3.0-only 下发布，完整许可见 LICENSE.md。浏览器抠图库版权归 IMG.LY；ONNX Runtime 归微软及贡献者，依赖许可随构建保留。静态发布包包含完整源代码 `source.zip`，网页底部提供源码仓库入口。

## 开发

Node.js 20+。执行 `npm ci`、`npm test`、`npm run build`、`npm run serve`。本地地址 http://127.0.0.1:4173 。

## 替换阿里云

在 `config.js` 设置 `provider: 'api'` 与 `apiUrl`。`background.js` 的适配器支持 multipart 的 `file` 字段，服务端直接返回图片二进制，或 `{code:0,data:{imageUrl:'https://…'}}`。后端保管凭据、调用 SegmentCommonImage，并处理 CORS 和临时图片读取。GitHub Pages 不运行服务端代码，不保管访问密钥。

## 数据与限制

仅姓名 / ID、分类、金额、时间文本保存在当前浏览器 localStorage，刷新后头像与商品恢复示例；上传的图片在内存里处理，不写入仓库。浏览器内输入最多 20 MB，解码后缩放至最长边 1600。金额 0–999999.99，分类与 ID 最多 12 字。导出图片为 680×680 PNG。

保存优先调用系统文件分享，不支持时请求下载并提供图片预览。App 内直接存相册需后续接入实际原生桥接协议；未将普通浏览器下载视为相册保存。
