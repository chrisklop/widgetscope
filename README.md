# WidgetScope - Webex Connect Debug Dashboard

A simple, single-file debug dashboard for testing Webex Connect (IMI Chat) chatbot widgets. No build process, no dependencies - just open and go.

## 5-Minute Setup

### Step 1: Get Your Widget Credentials

You need two values from Webex Engage:

1. Log in to **Webex Engage Admin Console**
2. Go to **Assets** → **LiveChat**
3. Click on your LiveChat asset (or create one)
4. Go to the **Installation** tab
5. Find the code snippet - it looks like this:

```html
<div id="divicw"
     data-bind="XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"  <!-- This is your BIND ID -->
     data-guid="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"> <!-- This is your GUID -->
</div>
<script src="https://media.imi.chat/widget/js/imichatinit.js"></script>
```

Copy these two values:
- **GUID** = the `data-guid` value
- **Bind ID** = the `data-bind` value

### Step 2: Open WidgetScope

**Option A: Use the hosted version**
```
https://widgetscope.vercel.app?guid=YOUR-GUID&bind=YOUR-BIND-ID
```

**Option B: Run locally**
1. Download `index.html`
2. Open it in your browser
3. Enter your credentials in the setup modal

### Step 3: Start Debugging

That's it! The widget should appear on the right side of the screen. Use the dashboard panels to:
- Watch PostMessages for widget communication
- Monitor network requests
- Check console output
- View widget configuration

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

Click the **Configure Widget** button in the sidebar to update your credentials.

---

## Dashboard Features

### Left Sidebar
- **Status indicators** - Widget and script load status
- **Session info** - Session ID, duration, fingerprint
- **Filter controls** - Hide noise from browser extensions
- **Widget config** - Parsed configuration from the widget

### Console Logs
Captures `console.log`, `console.error`, `console.warn` from your page.

### PostMessages
Shows all cross-origin messages. Look for:
- `loadstyles` - Widget configuration received
- `setlocal` - Widget storing data
- Messages from `media.imi.chat` (tagged as WIDGET)

### Network
Tracks XHR and Fetch requests with timing.

### Widget Events
Lifecycle events like script loading and DOM changes.

### Quick Actions
| Button | What it does |
|--------|--------------|
| **Inspect** | Shows current widget DOM state |
| **Ping** | Tests connectivity to IMI servers |
| **Full Report** | Copies debug report to clipboard |
| **Copy Config** | Copies widget config JSON |

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
