# BIP Widget Integration Examples

A collection of examples showing how to integrate the BIP chatbot widget into your website. The widget uses a direct instance API after creation and supports **multiple independent instances** on the same page.

## ✨ LIVE DEMOS AVAILABLE ✨

**View the working demos deployed on GitHub Pages:**

- **Single Widget Demo:** [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/)
- **Multi-Widget Demo:** [https://blogic-cz.github.io/widget-examles/demo.multiwidget.html](https://blogic-cz.github.io/widget-examles/demo.multiwidget.html)

## 🚀 Quick Start

### 1. Basic Integration (Single Widget)

First, include the widget script in your HTML (usually before the closing `</body>` tag):

```html
<script src="https://chat-widget-test.hc1.blogic.ai/widget.iife.js"></script>
```

Then, in a subsequent `<script>` tag, create and initialize the widget:

```html
<script>
  // Wait for the DOM to be fully loaded to ensure the script above has executed
  document.addEventListener("DOMContentLoaded", () => {
    if (
      window.BIPWidget &&
      typeof window.BIPWidget.createChatWidget === "function"
    ) {
      const chatWidgetInstance = window.BIPWidget.createChatWidget();

      // The widget automatically gets a unique instanceId
      console.log("Widget instance ID:", chatWidgetInstance.instanceId);

      const initialConfig = {
        token: "YOUR_JWT_TOKEN_HERE", // Mandatory: Replace with a valid token
        tenantCode: "YOUR_TENANT_CODE_HERE", // Mandatory: Replace with your tenant code
        assistantId: "YOUR_ASSISTANT_ID_HERE", // Mandatory: Specify your assistant
        showDefaultTrigger: true, // Optional: Show the default button
        position: "bottom-right", // Optional: "bottom-right" (default), "bottom-center", "bottom-left"
      };
      // Initialize using the command pattern (recommended)
      chatWidgetInstance.processCommand(["init", initialConfig]);

      // Example: Open the widget after initialization
      // chatWidgetInstance.processCommand(["open"]);
    } else {
      console.error(
        "BIPWidget factory not found. Ensure widget.iife.js is loaded correctly."
      );
    }
  });
</script>
```

### 2. Multi-Instance Integration (New!)

You can now create **multiple independent widget instances** on the same page without conflicts:

```html
<script>
  document.addEventListener("DOMContentLoaded", () => {
    if (
      window.BIPWidget &&
      typeof window.BIPWidget.createChatWidget === "function"
    ) {
      // Create multiple independent instances - IDs are automatically generated
      const chatWidgetInstance1 = window.BIPWidget.createChatWidget();
      const chatWidgetInstance2 = window.BIPWidget.createChatWidget();
      const chatWidgetInstance3 = window.BIPWidget.createChatWidget();

      // Each widget has a unique instanceId that you can access if needed
      console.log("Widget 1 ID:", chatWidgetInstance1.instanceId);
      console.log("Widget 2 ID:", chatWidgetInstance2.instanceId);
      console.log("Widget 3 ID:", chatWidgetInstance3.instanceId);

      // Configure each widget independently
      const config1 = {
        token: "YOUR_JWT_TOKEN_HERE",
        tenantCode: "YOUR_TENANT_CODE_HERE",
        assistantId: "YOUR_ASSISTANT_ID_HERE",
        showDefaultTrigger: true,
        position: "bottom-right",
      };

      const config2 = {
        token: "YOUR_JWT_TOKEN_HERE",
        tenantCode: "YOUR_TENANT_CODE_HERE",
        assistantId: "YOUR_ASSISTANT_ID_HERE",
        showDefaultTrigger: false, // Custom trigger only
        position: "bottom-left",
      };

      const config3 = {
        token: "YOUR_JWT_TOKEN_HERE",
        tenantCode: "YOUR_TENANT_CODE_HERE",
        assistantId: "YOUR_ASSISTANT_ID_HERE",
        showDefaultTrigger: true,
        position: "bottom-center",
      };

      // Initialize all widgets
      chatWidgetInstance1.processCommand(["init", config1]);
      chatWidgetInstance2.processCommand(["init", config2]);
      chatWidgetInstance3.processCommand(["init", config3]);

      // Control each widget independently
      document.getElementById("open-widget-1").onclick = () => {
        chatWidgetInstance1.processCommand(["open"]);
      };

      document.getElementById("open-widget-2").onclick = () => {
        chatWidgetInstance2.processCommand(["open"]);
      };
    }
  });
</script>
```

**Key Benefits of Multi-Instance Support:**

- ✅ **Independent State**: Each widget has its own conversation history, context, and settings
- ✅ **Isolated DOM**: No ID conflicts or CSS interference between instances
- ✅ **Separate Event Systems**: Events from one widget don't affect others
- ✅ **Different Positions**: Each widget can be positioned independently
- ✅ **Mixed Configurations**: Some can have default triggers, others custom triggers only
- ✅ **Automatic ID Management**: Unique IDs are automatically generated and accessible via `.instanceId`

**Instance ID Access:**

```javascript
// Create widgets - IDs are automatically generated
const supportWidget = window.BIPWidget.createChatWidget();
const salesWidget = window.BIPWidget.createChatWidget();

// Access the auto-generated IDs if needed for logging, analytics, etc.
console.log("Support widget ID:", supportWidget.instanceId);
console.log("Sales widget ID:", salesWidget.instanceId);

// Use widgets normally - no need to manage IDs manually
supportWidget.processCommand(["init", supportConfig]);
salesWidget.processCommand(["init", salesConfig]);
```

That's it! If `showDefaultTrigger` is `true` (the default), each widget will appear with its own floating button.

### 3. Control the Widget

Once you have `chatWidgetInstance`:

```javascript
// Open the chat
chatWidgetInstance.processCommand(["open"]);

// Close the chat
chatWidgetInstance.processCommand(["close"]);

// Add context for better AI responses
chatWidgetInstance.processCommand([
  "setContext",
  "user_profile",
  {
    userId: "123",
    name: "John Doe",
    role: "admin",
  },
  "User profile",
]);
```

**Note:** The `processCommand` method takes an array where the first element is the command name, followed by its arguments.

## 📋 Available Commands

All commands are invoked using `chatWidgetInstance.processCommand([commandName, ...args])`.

| Command (`commandName`)     | Description                                                                | Example Call with `processCommand`                                                                         |
| --------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `init`                      | Initialize/Re-initialize widget (token and tenantCode are mandatory)       | `instance.processCommand(["init", { token: "jwt", tenantCode: "xyz", showDefaultTrigger: true }])`         |
| `open`                      | Open the chat widget                                                       | `instance.processCommand(["open"])`                                                                        |
| `close`                     | Close the chat widget                                                      | `instance.processCommand(["close"])`                                                                       |
| `setPosition`               | Change widget position dynamically without re-initialization               | `instance.processCommand(["setPosition", "bottom-left"])`                                                  |
| `setContext`                | Add or update context data (ID, data object, description)                  | `instance.processCommand(["setContext", "user_profile", { name: "John" }, "User profile"])`                |
| `getContext`                | Retrieve a specific context by ID, or all contexts if ID is omitted        | `instance.processCommand(["getContext", "user_profile"])` or `instance.processCommand(["getContext"])`     |
| `clearContext`              | Clear a specific context by ID, or all contexts if ID is omitted           | `instance.processCommand(["clearContext", "user_profile"])` or `instance.processCommand(["clearContext"])` |
| `searchContext`             | Search within stored contexts using a query string                         | `instance.processCommand(["searchContext", "find_this_text"])`                                             |
| `removeContext`             | Remove a specific context by its ID                                        | `instance.processCommand(["removeContext", "context_id_to_delete"])`                                       |
| `setAssistantId`            | Set the Assistant ID for the conversation                                  | `instance.processCommand(["setAssistantId", "your_assistant_id"])`                                         |
| `getAssistantId`            | Get the current Assistant ID                                               | `instance.processCommand(["getAssistantId"])`                                                              |
| `addMessage`                | Add a message to the conversation history (e.g., prefill user input)       | `instance.processCommand(["addMessage", { type: "user", text: "Hello!" }])`                                |
| `getMessages`               | Retrieve all messages in the current conversation                          | `instance.processCommand(["getMessages"])`                                                                 |
| `getConversationState`      | Get the current state of the conversation (status, assistantId, msg count) | `instance.processCommand(["getConversationState"])`                                                        |
| `triggerCompletion`         | Triggers a response from the assistant                                     | `instance.processCommand(["triggerCompletion"])`                                                           |
| `deleteMessage`             | Delete a specific message from the conversation by its ID                  | `instance.processCommand(["deleteMessage", "message_id_to_delete"])`                                       |
| `updateConversationHistory` | Update the entire conversation history with new message array              | `instance.processCommand(["updateConversationHistory", messageArray])`                                     |
| `destroy`                   | Destroy the widget and clean up all resources                              | `instance.processCommand(["destroy"])`                                                                     |
| `help`                      | (If implemented) Get a list of available commands                          | `instance.processCommand(["help"])`                                                                        |

## 🔔 Event Listening

The widget fires events that you can listen to for real-time updates:

```javascript
// Listen for conversation updates (new messages, content streaming)
chatWidgetInstance.addEventListener("conversationUpdate", (event) => {
  const { action, data, messageId, content, type } = event.detail;

  switch (action) {
    case "addMessage":
      console.log(`New ${type} message added:`, data);
      break;
    case "appendContent":
      console.log(`Content appended to ${type} message:`, messageId, content);
      break;
    case "streamStart":
      console.log(`${type} response streaming started for message:`, messageId);
      break;
    case "streamEnd":
      console.log(
        `${type} response streaming finished for message:`,
        messageId
      );
      break;
  }
});

// Listen for widget open/close events
chatWidgetInstance.addEventListener("open", (event) => {
  console.log("Widget opened:", event.detail.isOpen);
});

chatWidgetInstance.addEventListener("close", (event) => {
  console.log("Widget closed:", event.detail.isOpen);
});

// Listen for context updates
chatWidgetInstance.addEventListener("contextUpdate", (event) => {
  console.log("Contexts updated:", event.detail.contexts);
});
```

### Available Events

| Event Name                | Description                                              | Event Detail Structure                                                                                                                              |
| ------------------------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `conversationUpdate`      | Fired when messages are added or content is streamed     | `{ action: "addMessage" \| "appendContent" \| "streamStart" \| "streamEnd", type: "user" \| "assistant" \| "system", data?, messageId?, content? }` |
| `open`                    | Fired when the widget is opened                          | `{ isOpen: true }`                                                                                                                                  |
| `close`                   | Fired when the widget is closed                          | `{ isOpen: false }`                                                                                                                                 |
| `contextUpdate`           | Fired when context data is updated                       | `{ contexts: Record<string, ContextData> }`                                                                                                         |
| `contextResult`           | Fired in response to context-related commands            | Various structures depending on the context action                                                                                                  |
| `conversationQueryResult` | Fired in response to conversation-related query commands | Various structures depending on the query action                                                                                                    |

## 🛠️ Integration Modes

### Mode 1: Default Floating Button

Configure during initialization:

```javascript
// Assuming chatWidgetInstance is already created as shown in Quick Start
const config = {
  token: "YOUR_JWT_TOKEN_HERE",
  tenantCode: "YOUR_TENANT_CODE_HERE",
  showDefaultTrigger: true,
  assistantId: "YOUR_ASSISTANT_ID_HERE",
};
// Use command pattern (recommended)
chatWidgetInstance.processCommand(["init", config]);
```

### Mode 2: Custom Trigger

Hide the default button and use your own triggers:

```javascript
// Assuming chatWidgetInstance is already created
const config = {
  token: "YOUR_JWT_TOKEN_HERE",
  tenantCode: "YOUR_TENANT_CODE_HERE",
  showDefaultTrigger: false,
  assistantId: "YOUR_ASSISTANT_ID_HERE",
};
// Use command pattern (recommended)
chatWidgetInstance.processCommand(["init", config]);

// Then use your own buttons to control the widget via the instance
document.getElementById("my-chat-button").onclick = () => {
  chatWidgetInstance.processCommand(["open"]);
};
```

## 🔧 Complete Working Examples

### Single Widget Demo

**👉 See [demo.test.html](./demo.test.html) for a complete, working implementation** that includes:

**🚀 You can also see this demo live at [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/)**

- ✅ Both integration modes (default button vs custom trigger)
- ✅ Mode switching between different configurations using `chatWidgetInstance.processCommand(["init", config])`
- ✅ Context management examples
- ✅ All available API commands with visual feedback using `chatWidgetInstance.processCommand()`
- ✅ Event listeners for all events including `conversationUpdate` (e.g., `chatWidgetInstance.addEventListener(...)`)
- ✅ Real-time status monitoring with visual feedback for message streaming
- ✅ Demonstration of conversation event handling patterns

**Just open `demo.test.html` in your browser** - it connects to the live widget and demonstrates all features based on the manual instantiation pattern.

### Multi-Widget Demo (New!)

**👉 See [demo.multiwidget.html](./demo.multiwidget.html) for a multi-instance demonstration** that shows:

**🚀 You can also see this demo live at [https://blogic-cz.github.io/widget-examles/demo.multiwidget.html](https://blogic-cz.github.io/widget-examles/demo.multiwidget.html)**

- ✅ **Three independent widget instances** running simultaneously
- ✅ **Different positions**: bottom-right, bottom-left, bottom-center
- ✅ **Mixed trigger modes**: some with default triggers, some with custom triggers only
- ✅ **Independent state management**: each widget maintains its own conversation history
- ✅ **Isolated event systems**: events are properly scoped to each instance
- ✅ **Batch operations**: open/close all widgets, check all widget states
- ✅ **Real-time status monitoring** for all instances

```html
<!-- Simple multi-widget setup example -->
<script>
  const widget1 = window.BIPWidget.createChatWidget();
  const widget2 = window.BIPWidget.createChatWidget();

  // Optional: Log the auto-generated instance IDs
  console.log("Widget 1 ID:", widget1.instanceId);
  console.log("Widget 2 ID:", widget2.instanceId);

  widget1.processCommand([
    "init",
    {
      token: "YOUR_TOKEN",
      tenantCode: "YOUR_TENANT",
      assistantId: "YOUR_ASSISTANT",
      showDefaultTrigger: true,
      position: "bottom-right",
    },
  ]);

  widget2.processCommand([
    "init",
    {
      token: "YOUR_TOKEN",
      tenantCode: "YOUR_TENANT",
      assistantId: "YOUR_ASSISTANT",
      showDefaultTrigger: false,
      position: "bottom-left",
    },
  ]);
</script>
```

## 💡 Key Benefits

- **🔧 Direct API Control** - Interact with concrete widget instances.
- **📱 Responsive** - Works on desktop and mobile.
- **🎨 Customizable** - Hide default button, use your own styling.
- **🧠 Context-aware** - Pass user/page data for better AI responses.
- **⚡ Flexible Initialization** - Configure widgets exactly as needed at creation time.
- **🚀 Multi-Instance Support** - Run multiple independent widgets on the same page without conflicts.

## 🛠️ Advanced Usage

For advanced features like event listeners, context search, dynamic configuration changes, and multi-instance management:

1. **Single Widget:** Check the JavaScript section in [demo.test.html](./demo.test.html)
2. **Multi-Widget:** Check the JavaScript section in [demo.multiwidget.html](./demo.multiwidget.html)

Both demonstrate the direct use of `chatWidgetInstance` for different scenarios.

## 📞 Need Help?

1. **Start with** the demos:
   - **Single widget:** [demo.test.html](./demo.test.html) **(Live: [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/))**
   - **Multi-widget:** [demo.multiwidget.html](./demo.multiwidget.html) **(Live: [https://blogic-cz.github.io/widget-examles/demo.multiwidget.html](https://blogic-cz.github.io/widget-examles/demo.multiwidget.html))**
2. **Copy the patterns** you see in the demo files for creating and interacting with `chatWidgetInstance`.
3. **Modify** the configuration and context data for your use case.

The API via the widget instance is designed to be straightforward and reliable for both single and multi-instance usage.
