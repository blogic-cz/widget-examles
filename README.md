# BIP Widget Integration Examples

A collection of examples showing how to integrate the BIP chatbot widget into your website. The widget uses a simple **push pattern API** (similar to Google Analytics) that works reliably across different environments.

**✨ LIVE DEMO AVAILABLE ✨**

**View the working demo deployed on GitHub Pages: [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/)**

## 🚀 Quick Start

### 1. Basic Integration

Add this to your HTML `<head>` section:

```html
<!-- Initialize BIP queue early -->
<script>
  window.BIP = window.BIP || [];
  BIP.push(["init", { showDefaultTrigger: true }]);
</script>

<!-- Load the widget script -->
<script src="https://chat-widget-test.hc1.blogic.ai/widget.iife.js"></script>
```

That's it! The widget will appear as a floating button in the bottom-right corner.

### 2. Control the Widget

```javascript
// Open the chat
BIP.push(["open"]);

// Close the chat
BIP.push(["close"]);

// Add context for better AI responses
BIP.push([
  "setContext",
  "user_profile",
  {
    userId: "123",
    name: "John Doe",
    role: "admin",
  },
]);
```

## 📋 Available Commands

| Command                | Description                                                                | Example                                                                      |
| ---------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `init`                 | Initialize widget with configuration                                       | `BIP.push(["init", { showDefaultTrigger: true }])`                           |
| `open`                 | Open the chat widget                                                       | `BIP.push(["open"])`                                                         |
| `close`                | Close the chat widget                                                      | `BIP.push(["close"])`                                                        |
| `setContext`           | Add or update context data (ID, data object)                               | `BIP.push(["setContext", "user_profile", { name: "John" }])`                 |
| `getContext`           | Retrieve a specific context by ID, or all contexts if ID is omitted        | `BIP.push(["getContext", "user_profile"])` or `BIP.push(["getContext"])`     |
| `clearContext`         | Clear a specific context by ID, or all contexts if ID is omitted           | `BIP.push(["clearContext", "user_profile"])` or `BIP.push(["clearContext"])` |
| `searchContext`        | Search within stored contexts using a query string                         | `BIP.push(["searchContext", "find_this_text"])`                              |
| `removeContext`        | Remove a specific context by its ID                                        | `BIP.push(["removeContext", "context_id_to_delete"])`                        |
| `setAssistantId`       | Set the Assistant ID for the conversation                                  | `BIP.push(["setAssistantId", "your_assistant_id"])`                          |
| `getAssistantId`       | Get the current Assistant ID                                               | `BIP.push(["getAssistantId"])`                                               |
| `addMessage`           | Add a message to the conversation history (e.g., prefill user input)       | `BIP.push(["addMessage", { role: "user", content: "Hello there!" }])`        |
| `getMessages`          | Retrieve all messages in the current conversation                          | `BIP.push(["getMessages"])`                                                  |
| `getConversationState` | Get the current state of the conversation (status, assistantId, msg count) | `BIP.push(["getConversationState"])`                                         |
| `help`                 | Get a list of available commands and their descriptions                    | `BIP.push(["help"])`                                                         |

## 🛠️ Integration Modes

### Mode 1: Default Floating Button

The widget shows its own blue icon that users can click to open chat.

```javascript
BIP.push(["init", { showDefaultTrigger: true }]);
```

### Mode 2: Custom Trigger

Hide the default button and use your own triggers.

```javascript
BIP.push(["init", { showDefaultTrigger: false }]);

// Then use your own buttons
document.getElementById("my-chat-button").onclick = () => {
  BIP.push(["open"]);
};
```

## 🔧 Complete Working Example

**👉 See [demo.test.html](./demo.test.html) for a complete, working implementation** that includes:

**🚀 You can also see this demo live at [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/)**

- ✅ Both integration modes (default button vs custom trigger)
- ✅ Mode switching between different configurations
- ✅ Context management examples (user data, page info)
- ✅ All available API commands with visual feedback
- ✅ Event listeners for advanced integration
- ✅ Real-time status monitoring

**Just open `demo.test.html` in your browser** - it connects to the live widget and demonstrates all features.

## 💡 Key Benefits

- **🚀 Works immediately** - Commands queue automatically before script loads
- **🔧 Simple API** - Just push commands to an array
- **📱 Responsive** - Works on desktop and mobile
- **🎨 Customizable** - Hide default button, use your own styling
- **🧠 Context-aware** - Pass user/page data for better AI responses
- **⚡ Fast integration** - 2-line setup for basic functionality

## 🛠️ Advanced Usage

For advanced features like event listeners, context search, and dynamic configuration changes, **check the JavaScript section in [demo.test.html](./demo.test.html)**.

The demo includes:

- Event handling for widget state changes
- Context management with search and filtering
- Mode switching and real-time configuration
- Error handling and status monitoring

## 📞 Need Help?

1. **Start with** [demo.test.html](./demo.test.html) - it's a complete working example. **(Live version: [https://blogic-cz.github.io/widget-examles/](https://blogic-cz.github.io/widget-examles/))**
2. **Copy the patterns** you see in the demo file
3. **Modify** the configuration and context data for your use case

The push pattern API is designed to be simple and reliable - if it works in the demo, it will work in your application!
