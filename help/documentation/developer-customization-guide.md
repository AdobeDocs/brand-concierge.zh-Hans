---
title: 开发人员和自定义指南
description: 了解如何安装Brand Concierge Web SDK和Web客户端、自定义外观和内容、处理客户端事件以及导出对话数据。
role: Developer,Admin
level: Experienced
toc: true
source-git-commit: 13db0491c987a08492820ac216e20feb87f30e44
workflow-type: tm+mt
source-wordcount: '1168'
ht-degree: 4%

---


# 开发人员和自定义指南 {#developer-customization-guide}

本指南适用于实施或自定义Brand Concierge部署的开发人员和技术团队。 它包括安装Web SDK和Web客户端、自定义外观和内容、通过回调函数侦听客户端事件，以及导出对话数据以进行报告。

## Web SDK和Web客户端安装 {#installation}

### 先决条件 {#prerequisites}

* 组织是Adobe Experience Platform (AEP)客户。
* 该页面通过Adobe Experience Platform Web SDK进行管理。
* 已为Brand Concierge启用该页面上使用的数据流ID。

### 步骤1：插入Web SDK {#inject-web-sdk}

将以下内容添加到页面的`<head>`部分：

```html
<script>
  !(function (n, o) {
    o.forEach(function (o) {
      n[o] ||
        ((n.__alloyNS = n.__alloyNS || []).push(o),
        (n[o] = function () {
          var u = arguments;
          return new Promise(function (i, l) {
            n[o].q.push([i, l, u]);
          });
        }),
        (n[o].q = []));
    });
  })(window, ["alloy"]);
</script>
<script src="https://cdn1.adoberesources.net/alloy/2.31.1/alloy.min.js"></script>
```

### 步骤2：插入Web客户端 {#inject-web-client}

在Web SDK脚本后添加以下内容，这些脚本仍位于`<head>`部分中：

```html
<script src="https://experience.adobe.net/solutions/experience-platform-brand-concierge-web-agent/static-assets/main.js"></script>
```

### 步骤3：配置Web SDK {#configure-web-sdk}

用您组织自己的值来调用`alloy("configure", ...)`，以代替下面的占位符：

```javascript
alloy("configure", {
  defaultConsent: "in",
  edgeDomain: "edge.adobedc.net",
  edgeBasePath: "ee",
  datastreamId: "YOUR_DATASTREAM_ID",
  orgId: "YOUR_IMS_ORG_ID",
  debugEnabled: true,
  idMigrationEnabled: false,
  thirdPartyCookiesEnabled: false,
  prehidingStyle: ".personalization-container { opacity: 0 !important }",
  onBeforeEventSend: (options) => {
    const x = options.xdm;
    const params = new URLSearchParams(window.location.search);
    const titleParam = params.get("title");
    if (titleParam) {
      x.web.webPageDetails.name = titleParam;
    } else {
      x.web.webPageDetails.name = "default-page";
    }
    return true;
  }
});
alloy("sendEvent", {});
```

| 字段 | 描述 |
|---|---|
| `datastreamId` | 为此页面配置的数据流ID，为Brand Concierge启用。 |
| `orgId` | 在下配置礼宾的IMS组织ID。 |
| `debugEnabled` | 验证集成后，在生产中设置为`false`。 |
| `prehidingStyle` | 在加载个性化内容之前应用CSS，以避免无样式内容闪烁。 |
| `onBeforeEventSend` | 可选挂接，用于在发送XDM有效负载之前对其进行修改 — 通常用于设置页面名称或上下文。 |

### 步骤4：初始化Web客户端 {#initialize-web-client}

在Web SDK配置调用后，通过调用引导API初始化Web客户端：

```javascript
window.adobe.concierge.bootstrap({
  instanceName: "alloy",
  stylingConfigurations: window.styleConfigurations,
  selector: "#brand-concierge-mount"
});
```

| 参数 | 类型 | 必需 | 描述 |
|---|---|---|---|
| `instanceName` | 字符串 | 是 | Web SDK实例名称。 |
| `stylingConfigurations` | JSON对象 | 是 | Web客户端样式配置（请参阅[可视化和内容自定义](#customization)）。 |
| `selector` | 字符串 | 是 | Web客户端装载到的HTML元素的CSS选择器。 |
| `onEvent` | 函数 | 否 | 客户端事件的回调（请参阅[客户端事件和回调函数](#events)）。 |

## 可视化和内容自定义 {#customization}

传递到`bootstrap()`的`stylingConfigurations`对象控制整个Web客户端的外观、行为和文本。 它分为多个区域。

### 元数据 {#metadata}

```javascript
"metadata": {
  "brandName": "Your Brand",
  "version": "1.0.0",
  "language": "en-US",
  "namespace": "brand-concierge"
}
```

### 行为 {#behavior}

控制各个聊天功能的功能行为。

```javascript
"behavior": {
  "input": {
    "enableVoiceInput": true
  },
  "chat": {
    "messageAlignment": "left",
    "messageWidth": "80%"
  },
  "privacyNotice": {
    "title": "Privacy Notice",
    "text": "By using this automated chatbot, you consent that any personal information you provide in the chat may be collected, used, analyzed, disclosed, and retained by Adobe and its service providers, in accordance with the Adobe Privacy Policy. Please do not enter any sensitive personal information (e.g., financial or health data)."
  },
  "disclaimer": {
    "attachWithInput": true
  },
  "chatTranscript": {
    "enabled": true,
    "maxSessions": 1,
    "maxMessagesPerSession": 20,
    "cleanupInterval": 24
  },
  "meetingForm": {
    "fieldsPerRow": 2,
    "title": { "text": "Schedule meeting", "alignment": "left" },
    "subtitle": { "text": "I'd be happy to help you schedule a meeting! Please fill out the form below, and we'll follow up with a calendar to confirm your day and time.", "alignment": "left" },
    "buttons": {
      "submit": { "text": "Schedule meeting", "alignment": "left" },
      "cancel": { "text": "Cancel", "alignment": "left" }
    }
  },
  "calendarWidget": {
    "title": { "text": "Book a meeting", "alignment": "left" },
    "subtitle": { "text": "Thanks! Here's a calendar where you can choose a time that works best for your schedule:", "alignment": "left" },
    "postTitle": { "text": "Once confirmed, you'll receive a calendar invite with all the details.", "alignment": "left" },
    "buttons": {
      "confirm": { "text": "Schedule a meeting", "alignment": "left" },
      "cancel": { "text": "Cancel", "alignment": "left" }
    }
  }
}
```

### 免责声明 {#disclaimer}

```javascript
"disclaimer": {
  "text": "AI responses may be inaccurate or misleading. Be sure to double check answers and sources."
}
```

### 文本字符串 {#text-strings}

所有面向用户的副本可通过`text`对象覆盖。 公用键：

| 键 | 用途 |
|---|---|
| `welcome.heading` / `welcome.subheading` | 欢迎屏幕标题和副文 |
| `input.placeholder` | 输入字段占位符文本 |
| `input.messageInput.aria` / `input.send.aria` / `input.mic.aria` | 输入控件的辅助功能标签 |
| `error.network` / `error.general` | 向访客显示的错误消息 |
| `loading.message` | 生成响应时显示的文本 |
| `feedback.dialog.title.positive` / `.negative` | 反馈对话框标题 |
| `feedback.dialog.question.positive` / `.negative` | 反馈对话框提示文本 |
| `feedback.toast.success` | 提交反馈后的确认toast |
| `feedback.thumbsUp.aria` / `feedback.thumbsDown.aria` | 反馈按钮的辅助功能标签 |

### 数组 {#arrays}

可配置的内容列表：

```javascript
"arrays": {
  "welcome.examples": [
    {
      "text": "I want to edit and enhance my photos",
      "image": "https://example.com/idea-1.png",
      "backgroundColor": "#66BFE7"
    }
  ],
  "feedback.positive.options": [
    "Helpful and relevant recommendations",
    "Clear and easy to understand",
    "Friendly and conversational tone",
    "Visually appealing presentation",
    "Other"
  ],
  "feedback.negative.options": [
    "Not helpful or relevant",
    "Confusing or unclear",
    "Too formal or robotic",
    "Poor visual presentation",
    "Other"
  ]
}
```

### 资产 {#assets}

```javascript
"assets": {
  "icons": {
    "company": "<svg>...</svg>"
  }
}
```

### 主题 {#theme}

CSS自定义属性控制颜色、字体和布局：

```css
"theme": {
  "--color-primary": "#1473e6",
  "--color-primary-hover": "#0056b3",
  "--color-button-primary": "#3B63FB",
  "--color-accent": "#9085ED",
  "--color-button-submit": "#4759e6",
  "--color-button-submit-hover": "#3a4bce",
  "--color-message-user": "#1473e6",
  "--font-family": "'Adobe Clean', adobe-clean, 'Trebuchet MS', sans-serif",
  "--main-container-background": "linear-gradient(135deg, #66ccff, #cc99ff, #ffcc99, #ccff99)",
  "--submit-button-fill-color": "white",
  "--card-text-background": "var(--color-background)",
  "--card-text-border-radius": "var(--border-radius-card)",
  "--message-concierge-link-decoration": "underline",
  "--message-max-width": "100%"
}
```

## 客户端事件和回调函数 {#events}

通过事件回调系统，页面可实时观察Web客户端生命周期事件、用户交互、响应、反馈和错误，这对于将参与数据发送到Adobe Analytics、Google Analytics或其他第三方系统非常有用。

### 关键特性 {#key-characteristics}

* **单个回调** — 一个`onEvent`函数接收由`event.eventType`区分的所有事件类型。
* **只读** — 事件数据是克隆的快照，不能用于修改客户端的行为。
* **错误隔离** — 捕获并记录回调中引发的异常；它们不会破坏Web客户端。
* **通过`bootstrap()`**&#x200B;注册 — 传递的方式与`onBeforeEventSend`相同。

### 快速入门 {#quick-start}

```javascript
window.adobe.concierge.bootstrap({
  instanceName: "my-instance",
  selector: "#brand-concierge-mount",
  stylingConfigurations: { /* ... */ },
  onEvent: (event) => {
    console.log(event.eventType, event.timestamp, event.data);
  }
});
```

### 按事件类型过滤 {#filtering}

```javascript
onEvent: (event) => {
  switch (event.eventType) {
    case "query:submitted":
      console.log("User query:", event.data.query);
      break;
    case "response:completed":
      console.log("Response received:", event.data.conversationId);
      break;
    case "card:clicked":
      console.log("Card clicked:", event.data.element.entity_info.productName);
      break;
    case "error:occurred":
      console.log("Error:", event.data.errorMessage);
      break;
  }
}
```

### 事件类型 {#event-types}

| 事件类型 | 值 | 类别 | 当它触发时 |
|---|---|---|---|
| `WEBCLIENT_INITIALIZED` | `webclient:initialized` | 生命周期 | 客户端完成初始化（已装入DOM，已加载内容） |
| `QUERY_SUBMITTED` | `query:submitted` | 用户交互 | 用户提交消息（键入或来自建议） |
| `PROMPT_SUGGESTION_CLICKED` | `promptSuggestion:clicked` | 用户交互 | 用户点击提示提示药丸 |
| `CARD_CLICKED` | `card:clicked` | 用户交互 | 用户单击信息卡 |
| `HISTORY_CLEARED` | `history:cleared` | 用户交互 | 用户清除聊天历史记录 |
| `RESPONSE_STARTED` | `response:started` | 响应 | 第一个流区块来自API |
| `RESPONSE_COMPLETED` | `response:completed` | 响应 | 接收并呈现完整响应 |
| `CARDS_RENDERED` | `cards:rendered` | 响应 | 信息卡（单个图像或轮播）完成渲染 |
| `FEEDBACK_SUBMITTED` | `feedback:submitted` | 反馈 | 用户提交反馈表单（拇指朝上/向下并带有详细信息） |
| `ERROR_OCCURRED` | `error:occurred` | 错误 | 出现错误（网络、API或运行时） |

### 生命周期事件 {#lifecycle-events}

在客户端完全初始化后触发`webclient:initialized`：已加载内容、插入CSS、在DOM中呈现聊天UI。

```json
{
  "eventType": "webclient:initialized",
  "timestamp": 1741638123789,
  "data": {
    "instanceName": "my-instance"
  }
}
```

### 用户交互事件 {#user-interaction-events}

当用户根据提示建议或小组件选项提交消息（无论是否键入）时，`query:submitted`都会触发。

```json
{
  "eventType": "query:submitted",
  "timestamp": 1741638124000,
  "data": {
    "query": "What photo editing tools do you offer?"
  }
}
```

`promptSuggestion:clicked`在用户单击提示性建议药丸时触发。 它在&#x200B;*之前*&#x200B;触发后续`query:submitted`事件。

```json
{
  "eventType": "promptSuggestion:clicked",
  "timestamp": 1741638124100,
  "data": {
    "suggestion": "Tell me more about Photoshop"
  }
}
```

当用户单击信息卡时，`card:clicked`触发。

```json
{
  "eventType": "card:clicked",
  "timestamp": 1741638124200,
  "data": {
    "element": {
      "entity_info": {
        "productName": "Adobe Photoshop",
        "productDescription": "Photo editing software",
        "productPageURL": "https://www.adobe.com/products/photoshop.html",
        "productImageURL": "https://example.com/photoshop.png"
      }
    }
  }
}
```

`history:cleared`在用户单击clear-chat-history按钮时触发。

```json
{
  "eventType": "history:cleared",
  "timestamp": 1741638124400,
  "data": {}
}
```

### 响应事件 {#response-events}

当第一个流区块从API到达时，`response:started`触发。

```json
{
  "eventType": "response:started",
  "timestamp": 1741638125000,
  "data": {
    "conversationId": "conv-abc-123",
    "interactionId": "int-xyz-456"
  }
}
```

收到完整响应时，`response:completed`将触发。

```json
{
  "eventType": "response:completed",
  "timestamp": 1741638126000,
  "data": {
    "conversationId": "conv-abc-123",
    "interactionId": "int-xyz-456"
  }
}
```

在DOM中呈现卡片后，`cards:rendered`触发。 它与`response:completed`分开触发，并指示使用的显示模式。

```json
{
  "eventType": "cards:rendered",
  "timestamp": 1741638126100,
  "data": {
    "element": [
      { "entity_info": { "productName": "Adobe Photoshop" } },
      { "entity_info": { "productName": "Adobe Illustrator" } }
    ],
    "displayMode": "carousel"
  }
}
```

### 反馈事件 {#feedback-events}

当用户完成并提交反馈表单（在缩略图打开/关闭后）时，`feedback:submitted`触发。

```json
{
  "eventType": "feedback:submitted",
  "timestamp": 1741638127000,
  "data": {
    "conversationId": "conv-abc-123",
    "interactionId": "int-xyz-456",
    "feedbackType": "negative",
    "selectedOptions": ["Incorrect information", "Not relevant"],
    "notes": "The response did not address my question about pricing."
  }
}
```

### 错误事件 {#error-events}

当客户端遇到网络、API或运行时错误时，`error:occurred`触发。

```json
{
  "eventType": "error:occurred",
  "timestamp": 1741638128000,
  "data": {
    "errorMessage": "Something went wrong. Please try again."
  }
}
```

### 事件对象结构 {#event-object-structure}

每个事件都具有相同的顶级形状：

```typescript
interface BrandConciergeEvent {
  eventType: string;  // e.g. "query:submitted"
  timestamp: number;  // Unix epoch, milliseconds
  data: object;       // Event-specific payload
}
```

### 数据类型引用：元素（产品卡） {#element-reference}

```typescript
interface Element {
  id?: string;
  type?: string;
  entity_info: {
    productName: string;
    productDescription: string;
    description: string;
    productPageURL: string;
    details: string;
    backgroundColor: string;
    learningResource: string;
    productImageURL: string;
    logo: string;
    variants?: Record<string, ElementVariant>;
    primary: ElementAction;
    secondary: ElementAction;
  };
}

interface ElementAction {
  label: string;
  url: string;
}
```

### 最佳实践 {#best-practices}

* **用于分析和监视。** 跟踪参与情况、查询模式和产品兴趣；将`error:occurred`转发到错误跟踪服务；跟踪卡片点击以进行转化分析。
* **保持回调快速。** 它在主线程上同步运行，因此请避免阻止网络调用：

```javascript
// Good — fire and forget
onEvent: (event) => {
  navigator.sendBeacon("/analytics", JSON.stringify(event));
}

// Avoid — blocking network call
onEvent: async (event) => {
  await fetch("/analytics", { body: JSON.stringify(event) });
}
```

* **对于状态机，不要依赖严格的事件顺序**。 事件在逻辑序列中触发，但使用`conversationId`和`interactionId`关联相关事件而不是假定顺序。
* **处理您自己的回调中的错误。** 客户端将隔离并记录回调错误，但回调中未经处理的错误仍可能会丢失分析数据：

```javascript
onEvent: (event) => {
  try {
    myAnalytics.track(event);
  } catch (e) {
    console.warn("Analytics tracking failed", e);
  }
}
```

## 使用AEP查询服务导出对话 {#export-conversations}

Brand Concierge将对话数据（提示、响应和反馈）写入Adobe Experience Platform (AEP)数据集。 您可以使用查询服务(SQL)直接查询这些内容，以构建自定义报表。

### 查找数据集和表名称 {#find-dataset}

1. 打开Adobe Experience Platform。

1. 转到&#x200B;**[!UICONTROL 数据集]**。

1. 搜索`cja_brand_concierge`以列出与Brand Concierge相关的数据集。

1. 打开您需要的数据集（例如，如果存在多个数据流，则打开响应而不是打开其他数据流）。

1. 在数据集详细信息视图中，查找查询服务使用的&#x200B;**[!UICONTROL 表名称]**，并检查示例或预览数据以确认列（提示、响应、反馈、时间戳等）。

>[!NOTE]
>
>表名称绑定到每个数据集，并且因环境和沙盒而异。 如果您有多个沙盒或部署，请在正确的沙盒中重复这些步骤，以便表名与写入数据的位置匹配。

### 示例查询 {#example-query}

```sql
SELECT *
FROM cja_brand_concierge_responses_dataset_5f5105bd_1c38_4ebc_8505_bd
WHERE timestamp >= TIMESTAMP '2026-03-16 00:00:00'
  AND timestamp <= NOW()
ORDER BY timestamp ASC;
```

>[!IMPORTANT]
>
>上面的表名称只是一个插图 — 不要对其进行硬编码。 首先在AEP中确认数据集的实际表名称（请参阅[查找数据集和表名称](#find-dataset)），然后调整时间过滤器、排序顺序或其他子句以满足您的报表需求。 使用与数据集相同的沙盒，从贵组织的查询服务工作流（UI、API或连接的客户端）运行查询。

### 在查询服务UI中运行查询 {#run-query-ui}

如果您需要手动拉取数据来进行报告，则查询服务UI提供了一种直接运行和下载结果的方法：

1. 在Adobe Experience Platform中，转到&#x200B;**[!UICONTROL 查询]**。

1. 在编辑器中输入查询并单击&#x200B;**[!UICONTROL 运行查询]**。

1. 查询完成后，结果会显示在编辑器下方的&#x200B;**[!UICONTROL 结果]**&#x200B;选项卡中。 从那里，您可以下载结果。

### 进一步阅读 {#further-reading}

* [查询服务API文档](https://experienceleague.adobe.com/zh-hans/docs/experience-platform/query/home){target="_blank"} — Adobe对查询服务行为、限制、身份验证和API路径的官方引用，这些引用将随着时间的推移而发生更改，与本指南无关。
