# 会话页面

会话列表组件支持更新、添加、删除会话记录、样式修改、状态改变等。

## 它提供的方法包括：

- 更新：更新对话列表项。
- 创建：创建对话列表项。
- 删除：删除对话列表项。
- updateRead：将对话设置为已读。
- updateExtension：设置对话自定义字段。

## 它提供的属性和回调通知包括：

- propsRef：设置对话列表控制器。
- onLongPress：按住对话列表项时发生。
- onPress：单击对话列表项时发生。
- onUpdateReadCount：更新对话列表项时发生。
- sortPolicy：设置对话列表项的排序规则。
- RenderItem：自定义对话列表项的样式。

## 最简单的集成示例如下：

```typescript
import * as React from "react";
import {
  ConversationListFragment,
  ScreenContainer,
} from "react-native-chat-uikit";
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ConversationListFragment />
    </ScreenContainer>
  );
}
```

## 自定义：点击对话列表项，进入聊天详情页面。 如果需要自定义，可以关注这个回调通知。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ConversationListFragment
        onPress={(data?: ItemDataType) => {
          // todo: enter to chat detail screen.
        }}
      />
    </ScreenContainer>
  );
}
```

## 自定义：您可以长按会话列表项以显示该项目的上下文菜单。 如果需要自定义，可以关注这个回调通知。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ConversationListFragment
        onLongPress={(data?: ItemDataType) => {
          // todo: show context menu.
        }}
      />
    </ScreenContainer>
  );
}
```

## 自定义：很多组件需要关注未读通知来改变消息提醒的状态。 如果需要，请遵循回调通知。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ConversationListFragment
        onUpdateReadCount={(unreadCount: number) => {
          // todo: show unread message count.
        }}
      />
    </ScreenContainer>
  );
}
```

## 自定义：默认排序是按`convId`排序，如果需要可以自行设置。 典型应用：会话粘在顶部。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ConversationListFragment
        sortPolicy={(a: ItemDataType, b: ItemDataType) => {
          if (a.key > b.key) {
            return 1;
          } else if (a.key < b.key) {
            return -1;
          } else {
            return 0;
          }
        }}
      />
    </ScreenContainer>
  );
}
```

## 自定义：自定义会话列表项的样式。 例如，可以自定义显示消息置顶状态和第二中断状态。

**注意** 如果开启侧滑功能，则需要设置侧滑组件的宽度。

```typescript
export default function ChatScreen(): JSX.Element {
  const chatId = "xxx";
  const chatType = 0;
  return (
    <ScreenContainer mode="padding" edges={["right", "left", "bottom"]}>
      <ConversationListFragment
        RenderItem={(props) => {
          return <View />;
        }}
      />
    </ScreenContainer>
  );
}
```

![img](/res/contact_list.png)
![img](/res/conversation_list_slide_menu.png)

https://user-images.githubusercontent.com/11733363/241342423-0a3ac24c-9fae-4961-8395-89a3c2e6ef5e.mov
