---
name: web-browser
description: Navigate and interact with web pages using OpenClaw browser automation. Use when user wants to browse websites, extract content, interact with pages (click, fill forms, scroll), take screenshots, or search the web.
---

# Web Browser

Use the `browser` tool to control a headless browser for web navigation and automation.

## Quick Start

1. **Start browser**: `browser(action="start", profile="openclaw")`
2. **Navigate to URL**: `browser(action="navigate", url="https://example.com")`
3. **Take snapshot**: `browser(action="snapshot")` — shows clickable elements
4. **Interact**: Use `browser(action="act", request={...})` to click, type, hover, etc.

## Model-Specific: qwen3.5-plus, kimi-k2.5

**When using qwen3.5-plus or kimi-k2.5, leverage visual analysis with screenshots:**

1. **When to take screenshots:**
   - Page contains images, charts, graphs, or visual data you need to interpret
   - Complex UI layouts where snapshot alone doesn't capture the full context
   - Verifying visual elements (colors, layout, design patterns)
   - Reading text from images or non-selectable UI elements
   - Debugging page rendering issues or unexpected behavior
   - When snapshot refs are unclear or elements aren't properly labeled

2. **How to use screenshots:**
   ```json
   {"action": "screenshot", "type": "png"}
   ```
   - The screenshot will be attached to your tool result
   - Analyze the visual content directly from the image
   - Combine with `snapshot` for element refs when you need to interact

3. **Workflow pattern:**
   - Navigate → Screenshot → Analyze visual content → Snapshot → Interact with refs
   - For complex pages: Screenshot first to understand layout, then snapshot to get actionable refs

## Common Actions

### Navigate to a URL
```json
{"action": "navigate", "url": "https://google.com"}
```

### Click an element
```json
{"action": "act", "request": {"kind": "click", "ref": "element-ref"}}
```

### Type text into an input
```json
{"action": "act", "request": {"kind": "type", "ref": "input-ref", "text": "search query"}}
```

### Take a screenshot
```json
{"action": "screenshot"}
```

### Get page snapshot (interactive UI)
```json
{"action": "snapshot"}
```

### Extract page content
```json
{"action": "navigate", "url": "https://example.com", "loadState": "domcontentloaded"}
```
Then use `browser(action="snapshot")` to see clickable elements with refs.

## Profile Options

- `profile="openclaw"` — Isolated OpenClaw-managed browser
- `profile="chrome"` — Your existing Chrome tabs (requires toolbar attachment)

## Tips

- Use `loadState: "domcontentloaded"` for faster navigation
- Use `ref` from snapshot results to reliably target elements
- `browser(action="tabs")` to list open tabs
- `browser(action="close")` to close the browser when done

## Handling Cookie Consent Dialogs

When a cookie consent banner or popup appears and blocks page interaction, **always agree to allow all cookies** to proceed. These dialogs often have buttons like:

- "Accept All" / "Accept Cookies"
- "Allow All" / "Allow Cookies"
- "I Accept" / "Agree"
- "OK" / "Got it"
- "Essentials Only" (if Accept All isn't available)
- "Manage Settings" / "Customize" → then find "Accept All"

### Handling Pattern

1. **Take a snapshot** to identify the cookie dialog elements
2. **Look for common button texts** that accept cookies (case-insensitive)
3. **Click the most prominent accept button** — usually the primary/featured button
4. **If there are options**, always choose the most permissive: "Accept All" > "Allow All" > "Essential Only"
5. **Dismiss any secondary dialogs** that appear after accepting (e.g., "Do you want to save these settings?")

### Common Button Ref Patterns

When searching for accept buttons, look for refs containing:
- `accept`, `allow`, `agree`, `ok`, `confirm`
- `cookie`, `consent`, `tracking`
- Avoid: `reject`, `decline`, `deny`, `manage`, `customize`

If the first click doesn't close the dialog, try clicking again or look for a secondary button.
