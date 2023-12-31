# 聊天页面

聊天组件提供了丰富的功能，支持文本、表情、图片、语音、文件等多种类型消息的输入，支持显示消息列表、自定义头像、自定义消息状态、自定义消息气泡，并可以修改消息状态。

![img](chat_detail_overview.png)

## 集成聊天页面

在项目中集成聊天页面组件 `ChatFragment`，并传入相应的参数即可使用。

| 参数           | 类型 | 是否必需   | 描述      |
| :------------- | :-----| :----- | :-------- |
| `chatId` | String | 是 | 会话 ID。  |
| `chatType` | String  |  是 | <br/> - `0`：单聊；<br/> - `1`：群聊； <br/> - `2`：聊天室。 |
| `propsRef`   |  | 否 | 设置聊天组件控制器。   |
| `screenParams` | | 否  | 设置聊天组件的参数。   |
| `messageBubbleList` | | 否  | 设置自定义消息气泡组件。   |
| `onUpdateReadCount` | | 否 | 未读消息计数更新时发生。   |
| `onClickMessageBubble` | | 否 | 单击消息气泡通知时发生。   |
| `onLongPressMessageBubble` | | 否 | 按住消息气泡时发生。   |
| `onClickInputMoreButton` | | 否 | 按下**更多**按钮时发生。   |
| `onPressInInputVoiceButton` | | 否 | 按下语音按钮时发生。   |
| `onPressOutInputVoiceButton` | | 否  | 释放语音按钮时发生。   |
| `onSendMessage`  | | 否   | 消息开始发送时发生。   |
| `onSendMessageEnd` |  | 否  | 消息发送完成时发生。   |
| `onVoiceRecordEnd` |  | 否 | 语音消息录制完成时发生。   |

```typescript
import * as React from "react";
import { ChatFragment, ScreenContainer } from "react-native-chat-uikit";
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";// 会话 ID。
  const chatType = 0; // 会话类型。
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment screenParams={{ chatId, chatType }} />
    </ScreenContainer>
  );
}
```

聊天页面相关的方法如下表所示：

| 方法   | 描述   |
| :------- | :----- | 
| `sendTextMessage`  | 发送文本消息。   | 
| `sendImageMessage`   | 发送图片消息。   |
| `sendVoiceMessage`  | 发送语音消息。    |
| `sendCustomMessage`  | 发送自定义消息。   |
| `sendFileMessage`  | 发送文件消息。   |
| `sendVideoMessage`  | 发送视频消息。   |
| `sendLocationMessage`  | 发送位置消息。   |
| `loadHistoryMessage`  | 加载历史消息。   |
| `deleteLocalMessage`  | 删除本地消息。   |
| `resendMessage` | 重发发送失败的消息。   |
| `downloadAttachment`  | 下载附件。   |

## 发送语音消息 

例如，你可以自行录制语音，然后调用 `sendVoiceMessage` 方法发送。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        onVoiceRecordEnd={(params) => {
          chatRef.current.sendVoiceMessage(params);
        }}
      />
    </ScreenContainer>
  );
}
```
![img](chat_detail_send_voice_msg.png)

## 自定义聊天气泡         

当默认的聊天气泡无法满足需求时，你可以自行设计聊天气泡的样式。

`MessageBubbleList` 为自定义聊天气泡列表组件，可以修改任何气泡内容，例如：头像、文字、图片、语音样式、消息状态等。该组件提供子组件实现文本、图片、音视频消息的样式：

- `TextMessageItem`：自定义文本消息样式。
- `LocationMessageItem`：自定义定位消息样式。
- `CustomMessageItem`：自定义自定义消息样式。
- `ImageMessageItem`：自定义图片消息样式。
- `VideoMessageItem`：自定义短视频消息样式。
- `VoiceMessageItem`：自定义语音消息样式。
- `FileMessageItem`：自定义文件消息样式。


```typescript
import type { MessageBubbleListProps } from "../fragments/MessageBubbleList";
import MessageBubbleList from "../fragments/MessageBubbleList";
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        messageBubbleList={{
          MessageBubbleListP: MessageBubbleListFragment,
          MessageBubbleListPropsP: {
            TextMessageItem: MyTextMessageBubble,
            VideoMessageItem: MyVideoMessageBubble,
            FileMessageItem: MyFileMessageBubble,
          } as MessageBubbleListProps,
          MessageBubbleListRefP: messageBubbleListRefP as any,
        }}
      />
    </ScreenContainer>
  );
}
```

![img](chat_detail_msg_list_item_custom_1.png)
![img](chat_detail_msg_list_item_custom_2.png)
![img](chat_detail_msg_list_item_custom_3.png)

## 自定义点击消息气泡回调

你可以设置 `ChatFragment` 中的 `onClickMessageBubble` 自定义点击消息气泡回调，例如：播放语音、预览图片等。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        onClickMessageBubble={(data: MessageItemType) => {
          // TODO：对于语音消息，进行播放；对于图片消息，进行预览。
        }}
      />
    </ScreenContainer>
  );
}
```

## 自定义长按消息气泡通知 

你可以设置 `ChatFragment` 中的 `onLongPressMessageBubble` 自定义长按消息气泡通知，例如：可以显示不同右键菜单。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        onLongPressMessageBubble={() => {
          // TODO：显示右键菜单。例如，消息转发、消息删除和消息重发等。
        }}
      />
    </ScreenContainer>
  );
}
```

![img](chat_detail_msg_list_item_long_press.png)

## 自定义发送消息前通知

你可以设置 `ChatFragment` 中的 `onSendMessage` 自定义发送消息前通知。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        onSendMessage={(message: ChatMessage) => {
          // TODO: 更新消息。
        }}
      />
    </ScreenContainer>
  );
}
```

## 自定义发送消息完成回调

你可以设置 `ChatFragment` 中的 `onSendMessageEnd` 自定义发送消息完成，例如，更新消息发送状态。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        onSendMessageEnd={(message: ChatMessage) => {
          // TODO：更新消息发送状态，即发送成功或失败。
        }}
      />
    </ScreenContainer>
  );
}
```

## 自定义背景色  

你可以设置 `MessageBubbleListPropsP` 中的 `backgroundColor` 自定义背景色。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        messageBubbleList={{
          MessageBubbleListP: MessageBubbleListFragment,
          MessageBubbleListPropsP: {
            style: { backgroundColor: "yellow" },
          } as MessageBubbleListProps,
          MessageBubbleListRefP: messageBubbleListRefP as any,
        }}
      />
    </ScreenContainer>
  );
}
```

![img](chat_detail_bg.png)

## 自定义时间标签显示和隐藏

你可以设置 `messageBubbleList` 中的 `showTimeLabel` 自定义时间标签显示和隐藏。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ChatFragment
        screenParams={{ chatId, chatType }}
        messageBubbleList={{
          MessageBubbleListP: MessageBubbleListFragment,
          MessageBubbleListPropsP: {
            showTimeLabel: false,
          } as MessageBubbleListProps,
          MessageBubbleListRefP: messageBubbleListRefP as any,
        }}
      />
    </ScreenContainer>
  );
}
```

![img](chat_detail_time_label.png)



