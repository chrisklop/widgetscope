# WidgetScope - Webex Connect Debug Dashboard

A simple, single-file debug dashboard for testing Webex Connect (IMI Chat) chatbot widgets. No build process, no dependencies - just open and go.

> **Disclaimer:** This is not an official Cisco or Webex product. I'm just a Webex Collaboration Engineer who wanted more visibility when configuring webchat. Feel free to reach out at [chris@klopconsulting.com](mailto:chris@klopconsulting.com) with feedback or if you get stuck — no guarantees but I'm happy to try to help!

## 5-Minute Setup

### Step 1: Whitelist the Domain

**Important:** The widget won't load unless you configure the domain in two places:

#### A. Webex Engage (LiveChat Asset)
1. Log in to **Webex Engage Admin Console**
2. Go to **Assets** → **Channel Assets** → **LiveChat**
3. Click **Edit** on your LiveChat asset
4. Go to the **Websites** tab
5. Add: `widgetscope.vercel.app` (no https://)
6. Save

#### B. Webex Connect Flow (Custom Variables)
1. Go to **Webex Connect** → **Services** → click your service name
2. Click **Flows** → click your flow name
3. Click **Edit** (top right)
4. Click **Settings** button (top right) — make sure no node is selected!
5. Go to **Custom Variables**
6. Set `liveChatDomain` to include your domain

### Step 2: Get Your Widget Credentials

While you're in the LiveChat asset settings:

1. Go to the **Installation** tab
2. Copy the entire installation code snippet

### Step 3: Open WidgetScope

1. Go to [widgetscope.vercel.app](https://widgetscope.vercel.app)
2. **Paste the entire installation snippet** into the "Quick Setup" box
3. Click "Save & Load Widget"

That's it! WidgetScope automatically extracts the GUID and Bind ID from your snippet.

**Alternative options:**
- Pass credentials via URL: `https://widgetscope.vercel.app?guid=YOUR-GUID&bind=YOUR-BIND-ID`
- Download `index.html` and run locally
- Enter GUID and Bind ID manually if you prefer

### Step 4: Start Debugging

The widget should appear on the right side of the screen. Use the dashboard panels to:
- Watch PostMessages for widget communication
- Monitor network requests and XHR/Fetch calls
- Check console output for errors
- View widget configuration
- **Monitor WebSocket connections** in real-time
- **Track MQTT protocol messages** (if your widget uses MQTT)

---

## Don't Have a Widget Yet?

### Quick Start with Webex Connect

1. **Get Access**: Sign up at [Webex Connect](https://www.webexconnect.io/) or contact Cisco sales
2. **Follow the Lab**: Complete the official setup lab at **[Webex CC Digital Lab](https://webexcc.github.io/pages/Digital/)** - this walks through creating LiveChat assets step-by-step
3. **Create a LiveChat Asset**: In Webex Engage Admin Console → Assets → LiveChat → Add
4. **Whitelist Your Domain**: Add your website domain (without http://) in the asset settings
5. **Copy the Installation Snippet**: Get your GUID and Bind ID from the Installation tab

### Official Documentation

| Resource | Description |
|----------|-------------|
| [Webex CC Digital Lab](https://webexcc.github.io/pages/Digital/) | Step-by-step setup tutorial |
| [LiveChat Channel Setup](https://docs.imi.chat/docs/livechat-channel) | Detailed configuration guide |
| [Webex Engage Developer Guide](https://docs.imi.chat/reference/introduction) | API reference |
| [Widget SDK Methods](https://docs.imi.chat/reference/imichatwidgetshow) | JavaScript SDK docs |
| [Cisco Community Forum](https://community.cisco.com/t5/webex-connect/bd-p/connect) | Get help from the community |

---

## Configuration Options

### URL Parameters (Recommended for Testing)

```
index.html?guid=YOUR-GUID&bind=YOUR-BIND-ID&org=Your%20Company%20Name
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| `guid` | Yes | Widget GUID from Webex Engage |
| `bind` | Yes | Data Bind ID from installation snippet |
| `org` | No | Your organization name (shown in sidebar) |

### Setup Modal

If you open the page without URL parameters, a setup modal appears automatically. Enter your credentials and click "Save & Load Widget". Your settings are saved to localStorage.

### Change Settings Anytime

Click the **Configure** button in the nav bar to update your credentials.

---

## Dashboard Features

### Top Navigation Bar

The nav bar provides quick access to all actions and shows connection status at a glance.

| Button | What it does |
|--------|--------------|
| **Inspect** | Shows current widget DOM state |
| **Ping** | Tests connectivity to IMI servers |
| **Full Report** | Copies comprehensive debug report to clipboard |
| **Config** | Copies widget config JSON |
| **Reload** | Reloads the widget |
| **Clear** | Clears all log panels |
| **Export** | Downloads debug data as JSON file |
| **Configure** | Opens the configuration modal |

The **Status Orb** in the nav bar shows connection health:
- **Green** - Widget loaded and connected
- **Yellow** - Warning (check logs)
- **Red** - Error or disconnected

### Left Sidebar
- **Widget Status** - Script and widget load status indicators
- **Session Info** - Session ID, start time, duration
- **Filter Controls** - Hide noise from browser extensions or debug messages
- **Widget Config** - Parsed configuration received from the widget (appears after widget loads)
- **Safari Debug Tools** (collapsible sections):
  - **Visibility** - Page visibility state tracking for background tab issues
  - **Browser** - Safari version detection and WebKit bug warnings
  - **Network** - Online/offline status monitoring
  - **Storage** - localStorage, sessionStorage, cookies, IndexedDB diagnostics

### Center Panels

#### Console Logs
Captures `console.log`, `console.error`, `console.warn` from your page. Color-coded by type.

#### PostMessages
Shows all cross-origin messages between frames. Look for:
- `loadstyles` - Widget configuration received (this is the big one!)
- `setlocal` - Widget storing data to localStorage
- Messages from `media.imi.chat` (tagged as WIDGET)

#### Network
Tracks XHR and Fetch requests with timing and status codes.

#### Widget Events
Lifecycle events like script loading, DOM mutations, and state changes.

### Right Column - WebSocket & MQTT Monitors

A dedicated split panel for real-time connection monitoring:

#### WebSocket Monitor (top)
- **Active connections** - Currently open WebSocket connections
- **Message counts** - Sent/received message totals
- **Zombie detection** - Flags connections that appear open but have no activity for 60+ seconds
- **Event log** - Real-time log of open, close, error, and message events

#### MQTT Monitor (bottom)
If your Webex Connect asset uses MQTT as the transfer protocol, this panel decodes and displays:
- **Connection state** - Connecting, Connected, or Disconnected
- **Packet types** - CONNECT, CONNACK, PUBLISH, SUBSCRIBE, PINGREQ, etc.
- **Topics** - MQTT topics being published/subscribed
- **QoS levels** - Quality of Service for each message
- **Payloads** - Message content (JSON auto-formatted)

### Far Right
The right 25% of the screen is reserved for the actual chat widget to appear.

---

## Troubleshooting

### Widget Not Appearing?

1. **Check your GUID and Bind ID** - Make sure they're correct (no extra spaces)
2. **Whitelist your domain** - In Webex Engage, add your domain WITHOUT http://
3. **Check the Console Logs panel** - Look for errors
4. **Click Ping** - Verify you can reach IMI servers

### Widget Loads But No Response?

1. **Check `agent_avail`** in the Widget Config section - if it's 0, no agents are online
2. **Verify your bot flow** is published in Webex Connect
3. **Check webhook configuration** in your bot integration

### Common Errors

| Error | Solution |
|-------|----------|
| 404 on imichatinit.js | Check if script URL is blocked |
| CORS errors | Domain not whitelisted in Webex Engage |
| Widget GUID not found | Verify GUID matches your asset |
| loadstyles not received | Check GUID and Bind ID are correct |

### Safari-Specific Issues

Safari has known issues with WebSocket connections, especially in background tabs. WidgetScope helps diagnose these:

| Issue | What to Check |
|-------|---------------|
| Widget disconnects when tab is hidden | Check **Visibility** panel - correlate with WebSocket close events |
| WebSocket closes without `onclose` event | **Browser** panel shows if Safari < 17.3 (WebKit bug #247943) |
| Storage errors in iframe | **Storage** panel shows ITP restrictions |
| Connection drops on iOS | Check **Visibility** + **Network** panels for state changes |

### WebSocket/MQTT Issues

| Issue | What to Check |
|-------|---------------|
| No WebSocket connections | Widget may not have loaded - check Console Logs |
| WebSocket shows "zombie" | Connection is stale - may need reconnection logic |
| MQTT not detected | Your asset may use WebSocket protocol instead of MQTT |
| MQTT CONNACK failure | Check return code in MQTT panel - usually auth issue |

---

## Deployment

This is a single HTML file with no dependencies. Deploy anywhere:

```bash
# Vercel
vercel --prod

# Netlify
netlify deploy --prod

# GitHub Pages
# Just commit and enable Pages in repo settings

# Or just open the file locally!
```

---

## Using in Your Own Website

Once you've tested with WidgetScope, add the widget to your site:

```html
<!-- Add before closing </body> tag -->
<div id="divicw"
     data-bind="YOUR-BIND-ID"
     data-guid="YOUR-GUID">
</div>
<script src="https://media.imi.chat/widget/js/imichatinit.js"></script>
```

For advanced usage (custom buttons, passing user data), see the [SDK documentation](https://docs.imi.chat/reference/introduction).

---

## License

MIT License - Use freely, modify, distribute.

## Resources

- [Webex CC Digital Lab](https://webexcc.github.io/pages/Digital/) - Official setup tutorial
- [Webex Connect Platform](https://www.webexconnect.io/)
- [Developer Documentation](https://docs.imi.chat/)
- [Cisco Community](https://community.cisco.com/t5/webex-connect/bd-p/connect)
