---
name: easy-xianyu-publish-free
description: 免费、免第三方 API Key 的闲鱼商品上架助手。用户想把商品资料和图片整理后填写到闲鱼发布页、检查必填项或生成待发布草稿时使用。它通过用户已登录的 Easy WebBridge 浏览器工作，必须指定 browserId 和账号别名；默认停在最终发布确认前，不承诺成交、流量或规避风控。
---

# 免费闲鱼商品上架

把已有的商品资料填进闲鱼发布页，减少重复复制粘贴。这个 Skill 不提供闲鱼账号，也不绕过登录、验证码或风控。

## 适用场景

- 商品资料已经在本地 Markdown、TXT、JSON 或表格中，想整理成闲鱼发布草稿。
- 需要在指定的闲鱼账号环境中填写标题、描述、价格、分类、数量和图片。
- 需要在发布前检查字段是否完整，而不是让智能体直接替你发布。

## 前置条件

1. 安装并启动 [Easy WebBridge](https://github.com/xxjrq/easy-webbridge)，网络不稳定时可用 [Gitee 镜像](https://gitee.com/xxjrq/easy-webbridge)。
2. 确认本机连接地址 `127.0.0.1:17777` 可用，并在 EasyBR 中提前登录闲鱼。
3. 提供一个明确的在线 `browserId`、账号别名和商品资料路径。没有 EasyBR 时只能生成填写清单，不能操作网页。

## 输入

```json
{
  "browserId": "easybr-xianyu-a",
  "accountAlias": "闲鱼主店",
  "source": "./product.json",
  "mode": "draft-only",
  "images": ["./images/01.jpg", "./images/02.jpg"]
}
```

`source` 至少包含 `title`、`description`、`price`、`category`；缺少字段时先返回清单，不猜价格或分类。`mode` 只能是 `draft-only` 或 `confirm-before-publish`。

## 操作步骤

1. 校验 `browserId`、账号别名、资料路径和图片是否存在；同一 `browserId` 已有任务时排队，不并发操作。
2. 通过 Easy WebBridge 的已公开连接能力选择浏览器，不在 Skill 内虚构接口或保存 Cookie、密码、Token。
3. 打开闲鱼发布入口，确认页面显示的是目标账号。未登录、验证码、实名、风控或权限提示立即返回 `needs_user_action`。
4. 按页面实际字段依次填写标题、描述、分类、价格、数量、成色和图片；每个关键字段填完后重新读取页面确认。
5. 输出字段检查结果和发布页截图或链接（若浏览器能力提供），状态为 `draft_ready` 时停在发布按钮前。
6. 只有用户在当前任务中明确确认，才能进入下一步；点击最终发布仍应返回 `needs_user_action`，由用户在页面完成最后确认。

## 输出

生成 JSON 或 Markdown 报告，至少包含 `status`、`browserId`、`accountAlias`、`filledFields`、`missingFields`、`imageCount`、`sourceUrl`、`nextAction` 和 `errors`。状态只使用 `draft_ready`、`needs_user_action`、`partial`、`failed`。

## 失败处理

- Easy WebBridge 未启动或找不到 `browserId`：返回 `needs_user_action`，说明启动地址和需要选择的浏览器。
- 闲鱼要求登录、验证码、实名或风控：停止当前账号，不重试绕过；其他账号任务不受影响。
- 图片格式、大小或数量不符合页面要求：列出具体文件和修复建议，不伪造上传成功。
- 页面字段改版或读取不到值：保留已经完成的字段，返回 `partial`，不要点击发布。

## 安全边界

默认不发布、不改价、不下架、不删除、不私信、不付款。任何这些动作都要用户在当次操作前明确确认；同一浏览器的任务必须串行，多账号结果必须带账号别名和 `browserId`。

## 自测

```bash
node scripts/self-test.mjs
```

