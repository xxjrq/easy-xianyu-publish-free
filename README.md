# 免费闲鱼商品上架

把已有的商品资料和图片填到闲鱼发布页，生成发布前草稿。免费使用，不需要第三方 API Key。

## 使用前先准备

你需要安装并启动 [Easy WebBridge](https://github.com/xxjrq/easy-webbridge)，备用下载地址是 [Gitee](https://gitee.com/xxjrq/easy-webbridge)。在 EasyBR 中先登录闲鱼，再把 `browserId` 和账号别名交给智能体。没有 EasyBR 时，智能体只能帮你整理资料，不能替你打开闲鱼。

## 怎么说

“使用 `$easy-xianyu-publish-free`，用 `easybr-xianyu-a` 的闲鱼主店，把 `./product.json` 填成发布草稿，发布前停住。”

默认会检查字段并停在发布按钮前。登录、验证码、实名和风控需要你自己处理；不保存账号信息，也不保证成交或流量。

