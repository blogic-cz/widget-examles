# BIP Widget Integration Examples

A collection of examples showing how to integrate the BIP chatbot widget into your website. The widget uses a direct instance API after creation.

**✨ LIVE DEMO AVAILABLE ✨**

**View the working demo deployed on GitHub Pages: [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/)**

## 🚀 Quick Start

### 1. Basic Integration

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

      const initialConfig = {
        token: "YOUR_JWT_TOKEN_HERE", // Mandatory: Replace with a valid token
        tenantCode: "YOUR_TENANT_CODE_HERE", // Mandatory: Replace with your tenant code
        assistantId: "YOUR_ASSISTANT_ID_HERE", // Optional: Specify your assistant
        showDefaultTrigger: true, // Optional: Show the default button
      };
      chatWidgetInstance.init(initialConfig);

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

That's it! If `showDefaultTrigger` is `true` (the default), the widget will appear.

### 2. Control the Widget

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

| Command (`commandName`) | Description                                                                | Example Call with `processCommand`                                                                         |
| ----------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `init`                  | Initialize/Re-initialize widget (token and tenantCode are mandatory)       | `instance.processCommand(["init", { token: "jwt", tenantCode: "xyz", showDefaultTrigger: true }])`         |
| `open`                  | Open the chat widget                                                       | `instance.processCommand(["open"])`                                                                        |
| `close`                 | Close the chat widget                                                      | `instance.processCommand(["close"])`                                                                       |
| `setContext`            | Add or update context data (ID, data object, description)                  | `instance.processCommand(["setContext", "user_profile", { name: "John" }, "User profile"])`                |
| `getContext`            | Retrieve a specific context by ID, or all contexts if ID is omitted        | `instance.processCommand(["getContext", "user_profile"])` or `instance.processCommand(["getContext"])`     |
| `clearContext`          | Clear a specific context by ID, or all contexts if ID is omitted           | `instance.processCommand(["clearContext", "user_profile"])` or `instance.processCommand(["clearContext"])` |
| `searchContext`         | Search within stored contexts using a query string                         | `instance.processCommand(["searchContext", "find_this_text"])`                                             |
| `removeContext`         | Remove a specific context by its ID                                        | `instance.processCommand(["removeContext", "context_id_to_delete"])`                                       |
| `setAssistantId`        | Set the Assistant ID for the conversation                                  | `instance.processCommand(["setAssistantId", "your_assistant_id"])`                                         |
| `getAssistantId`        | Get the current Assistant ID                                               | `instance.processCommand(["getAssistantId"])`                                                              |
| `addMessage`            | Add a message to the conversation history (e.g., prefill user input)       | `instance.processCommand(["addMessage", { type: "user", text: "Hello!" }])`                                |
| `getMessages`           | Retrieve all messages in the current conversation                          | `instance.processCommand(["getMessages"])`                                                                 |
| `getConversationState`  | Get the current state of the conversation (status, assistantId, msg count) | `instance.processCommand(["getConversationState"])`                                                        |
| `triggerCompletion`     | Triggers a response from the assistant                                     | `instance.processCommand(["triggerCompletion"])`                                                           |
| `help`                  | (If implemented) Get a list of available commands                          | `instance.processCommand(["help"])`                                                                        |

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
chatWidgetInstance.init(config);
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
chatWidgetInstance.init(config);

// Then use your own buttons to control the widget via the instance
document.getElementById("my-chat-button").onclick = () => {
  chatWidgetInstance.processCommand(["open"]);
};
```

## 🔧 Complete Working Example

**👉 See [demo.test.html](./demo.test.html) for a complete, working implementation** that includes:

**🚀 You can also see this demo live at [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/)**

- ✅ Both integration modes (default button vs custom trigger)
- ✅ Mode switching between different configurations using `chatWidgetInstance.init()`
- ✅ Context management examples
- ✅ All available API commands with visual feedback using `chatWidgetInstance.processCommand()`
- ✅ Event listeners (e.g., `chatWidgetInstance.addEventListener(...)`)
- ✅ Real-time status monitoring

**Just open `demo.test.html` in your browser** - it connects to the live widget and demonstrates all features based on the manual instantiation pattern.

## 💡 Key Benefits

- **🔧 Direct API Control** - Interact with a concrete widget instance.
- **📱 Responsive** - Works on desktop and mobile.
- **🎨 Customizable** - Hide default button, use your own styling.
- **🧠 Context-aware** - Pass user/page data for better AI responses.
- **⚡ Flexible Initialization** - Configure the widget exactly as needed at creation time.

## 🛠️ Advanced Usage

For advanced features like event listeners, context search, and dynamic configuration changes, **check the JavaScript section in [demo.test.html](./demo.test.html)**. It demonstrates the direct use of the `chatWidgetInstance`.

## 📞 Need Help?

1. **Start with** [demo.test.html](./demo.test.html) - it's a complete working example of manual instantiation. **(Live version: [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/))**
2. **Copy the patterns** you see in the demo file for creating and interacting with `chatWidgetInstance`.
3. **Modify** the configuration and context data for your use case.

The API via the widget instance is designed to be straightforward and reliable.
