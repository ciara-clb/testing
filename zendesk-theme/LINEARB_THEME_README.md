# LinearB Support Portal – Enhanced Copenhagen Theme

Built on top of [Zendesk Copenhagen v4.36.1](https://github.com/zendesk/copenhagen_theme).

---

## What's New vs Stock Copenhagen

| Feature | File(s) |
|---|---|
| Alert banners (info / warning / critical / success) | `styles/_linearb-custom.scss`, `script.js`, `templates/home_page.hbs` |
| Live system status bar (Statuspage.io ready) | `styles/_linearb-custom.scss`, `script.js`, `templates/home_page.hbs` |
| "How to Engage with Support" section | `templates/home_page.hbs`, `styles/_linearb-custom.scss` |
| Interactive IVR phone menu widget | `templates/home_page.hbs`, `styles/_linearb-custom.scss` |
| SLA reference table | `templates/home_page.hbs`, `styles/_linearb-custom.scss` |
| Article callout classes (`.lb-callout`) | `styles/_linearb-custom.scss` |
| Embeddable status page widget | `templates/status_page.hbs`, `styles/_linearb-custom.scss`, `script.js` |

---

## Setup

### 1. Upload theme to Zendesk

```bash
# Install Zendesk CLI (zx)
npm install -g @zendesk/zcli

# Package and upload
zcli themes:import --theme-path ./linearb_theme
```

### 2. Configure brand colors in Zendesk

In **Guide Admin → Customize → Colors**, the default LinearB values are already baked into `manifest.json`:

| Token | Value |
|---|---|
| `brand_color` | `#17494D` (LinearB teal) |
| `brand_text_color` | `#FFFFFF` |
| `text_color` | `#2F3941` |
| `link_color` | `#1F73B7` |

---

## Alert Banners

### Showing a banner from the Zendesk Theme script

In **Guide Admin → Customize → Edit Code → script.js** (or via a Guide widget), call:

```javascript
LinearBAlerts.show({
  type:    'critical',           // 'info' | 'warning' | 'critical' | 'success'
  message: 'We are investigating elevated error rates on Jira sync.',
  ctaText: 'View incident',      // optional
  ctaHref: 'https://status.linearb.io/incidents/xyz'  // optional
});
```

To hide programmatically:
```javascript
LinearBAlerts.hide();
```

### Connecting to Statuspage.io for automatic banners

1. Open `script.js`
2. Find `const STATUS_API_URL = '';`
3. Replace with your Statuspage.io summary endpoint:
   ```javascript
   const STATUS_API_URL = 'https://[your-page-id].statuspage.io/api/v2/summary.json';
   ```

The status bar and alert banner will now **update automatically on every page load**.

---

## IVR Phone Menu

Update the phone number and menu options in `templates/home_page.hbs`, section `lb-ivr-widget`:

```html
<a href="tel:+18005551234" class="lb-ivr-number__link">+1 (800) 555-1234</a>
```

Edit the `<ol class="lb-ivr-menu__list">` items to match your actual IVR tree.

---

## Adding Article Callout Boxes

In any Zendesk article, switch to **HTML source mode** and use:

```html
<div class="lb-callout lb-callout--warning">
  <strong>Maintenance Window</strong>
  The platform will be unavailable Saturday May 3rd, 2–4am ET.
</div>
```

Available types: `lb-callout--info`, `lb-callout--warning`, `lb-callout--critical`, `lb-callout--success`

---

## Embeddable Status Page

1. Create a new Zendesk Guide article at `/hc/articles/[id]-status`
2. In HTML source mode, paste the contents of `templates/status_page.hbs`
3. The component statuses are static by default — update them manually, or wire
   up the `lb-status-components` div dynamically using `LinearBAlerts` + a
   Statuspage fetch in `script.js`

---

## Updating the Engage with Support Section

All content lives in `templates/home_page.hbs` under the `lb-engage-section` block.

To add a new card, copy a `<div class="lb-engage-card">` block and customise the icon, title, description, tips and CTA.

---

## File Structure (new files only)

```
linearb_theme/
├── styles/
│   └── _linearb-custom.scss   ← All new component styles (do not edit core _*.scss)
├── templates/
│   ├── home_page.hbs           ← Modified — adds alert banner, status bar, engage section
│   └── status_page.hbs        ← New — embeddable status page template
└── script.js                  ← Appended — LinearBAlerts + StatusBar JS
```
