# MailMerge Pro

**AI-powered Chrome extension for bulk email campaigns. Connect Google Sheets, personalize emails with variables, and track opens/replies in real-time. Features analytics dashboard, AI composer, scheduling, and CSV export. Built with Gmail API, Sheets API, and Gemini for enterprise-grade email automation.**

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Chrome](https://img.shields.io/badge/Chrome-Extension-yellow)

## ✨ Features

### 📧 Bulk Email Sending
- Send personalized emails to hundreds of contacts from a single compose
- Dynamic variable insertion: `{FirstName}`, `{Company}`, `{Email}`, etc.
- Configurable send delays to avoid Gmail rate limits
- Built-in drafts and template management
- Test email before sending full campaign

### 📊 Google Sheets Integration
- Connect any Google Sheet as your contact list
- Auto-detect columns and variable tokens
- Real-time preview of how each email will look
- No CSV uploads needed—works directly with Sheets
- Column mapping with manual override option

### 🎯 Open & Reply Tracking
- Pixel-based open detection (know when recipients open emails)
- Automatic reply detection using Gmail API
- Track engagement per campaign and per recipient
- Multi-open detection (see how many times someone opened)
- Background task checks for replies every 15 minutes

### 📈 Analytics Dashboard
- Campaign management with detailed stats
- Timeline charts showing opens by hour
- Recipient table with search/filter functionality
- Send reminders to unopened emails
- Export data as CSV for further analysis

### 🤖 AI Email Composer (Optional)
- Gemini API integration for auto-generating templates
- Supports tone customization (professional, casual, urgent, etc.)
- Intelligent variable suggestions
- Free tier: 50 requests/month

### 🔒 Privacy & Security
- All data stored locally in Chrome storage
- OAuth 2.0 authentication for Gmail
- No tracking of personal data
- No third-party servers (except Gmail API)

## 📦 Tech Stack

| Component | Technology |
|-----------|-----------|
| **Frontend** | HTML, CSS, JavaScript (Vanilla) |
| **Backend** | Chrome APIs (identity, storage, alarms) |
| **APIs** | Gmail REST API v1, Google Sheets API v4, Gemini API |
| **UI Library** | Chart.js, Lucide Icons |
| **Design** | Dark theme, glassmorphism UI |

## 🚀 Installation

### From Source

1. **Clone the repository:**
   ```bash
   git clone https://github.com/AyushAuraLabs/BulkG.git
   cd mailmerge-pro
   ```

2. **Setup Google Cloud OAuth:**
   - Go to [Google Cloud Console](https://console.cloud.google.com)
   - Create new project
   - Enable APIs: Gmail API, Google Sheets API (optional: Gemini API)
   - Create OAuth 2.0 credential (Web application)
   - Add authorized redirect URI: `https://gooonlmmkohodfdnbdlnkbliamfaqcna.chromiumapp.org/`
   - Copy Client ID

3. **Update manifest.json:**
   ```json
   "oauth2": {
     "client_id": "YOUR_CLIENT_ID_HERE"
   }
   ```

4. **Load in Chrome:**
   - Open `chrome://extensions`
   - Enable "Developer mode"
   - Click "Load unpacked"
   - Select this folder
   - Extension ID will be generated

5. **Configure OAuth Consent Screen:**
   - In Google Cloud Console, go to OAuth Consent Screen
   - Add authorized domains: `google.com`
   - Add test users (your Gmail account)
   - Scopes: gmail.send, gmail.readonly, gmail.modify, gmail.compose, spreadsheets.readonly, userinfo.email, userinfo.profile

## 📖 Usage

### Quick Start

1. **Open Gmail** in Chrome
2. Click **MailMerge Pro** sidebar (right side)
3. Click **"Compose Bulk Email"**
4. **Authenticate** with Google (one-time)
5. **Connect Google Sheet:**
   - Paste sheet URL containing contacts
   - Click "Sync Sheet"
   - Confirm column mapping

6. **Write Email Template:**
   - Subject: `Hello {FirstName}!`
   - Body: Use `{VariableName}` for personalization
   - Click "Preview" to see all emails

7. **Send Campaign:**
   - Configure delay between emails (1-5 seconds recommended)
   - Enable tracking (optional)
   - Click "Send All"

8. **Track Results:**
   - Go to Analytics tab
   - Select campaign
   - View opens, replies, engagement timeline
   - Send reminders to unopened recipients

### Advanced Features

**AI Email Composer:**
- Get Gemini API key from [Google AI Studio](https://aistudio.google.com/app/apikey)
- Add to Settings page
- Click "AI Compose" in editor

**Custom Variables:**
- System variables: `{date}`, `{time}`, `{serial_number}`, `{unsubscribe_link}`
- Sheet columns: Any column name becomes `{ColumnName}`

**Batch Operations:**
- Select multiple recipients
- Apply tags: interested, not_interested, follow_up, unsubscribe
- Filter by tags in analytics

## 📁 Project Structure

```
mailmerge-pro/
├── manifest.json              # Extension configuration
├── popup/                     # Main popup UI
│   ├── popup.html
│   ├── popup.css
│   └── popup.js
├── sidebar/                   # 6-step wizard for email composition
│   ├── sidebar.html
│   ├── sidebar.css
│   └── sidebar.js
├── dashboard/                 # Main control dashboard
│   ├── dashboard.html
│   ├── dashboard.css
│   └── dashboard.js
├── pages/                     # Additional pages
│   ├── tracking.html          # Analytics dashboard
│   ├── settings.html          # Configuration
│   └── *.css, *.js
├── background/                # Service worker
│   └── service-worker.js      # Background tasks, reply polling
├── content/                   # Content scripts
│   └── gmail-injector.js      # Sidebar injection
├── utils/                     # Utility modules
│   ├── gmail-api.js           # Gmail API client
│   ├── sheets-api.js          # Google Sheets API client
│   ├── template-engine.js     # Variable interpolation
│   ├── tracking-engine.js     # Campaign tracking
│   └── ai-composer.js         # Gemini AI integration
├── assets/                    # Icons and images
│   └── icons/
├── README.md                  # This file
└── .gitignore                 # Git ignore rules
```

## 🔐 OAuth Setup Details

### Web Application Credentials (Recommended)

```
Application Type: Web application
Authorized Redirect URI: https://gooonlmmkohodfdnbdlnkbliamfaqcna.chromiumapp.org/
Scopes Required:
- gmail.send
- gmail.readonly
- gmail.modify
- gmail.compose
- spreadsheets.readonly
- userinfo.email
- userinfo.profile
```

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| "OAuth client not found" | Check Client ID in manifest.json matches Google Cloud |
| "invalid_client" error | Ensure test user email is added in OAuth Consent Screen |
| Sidebar not appearing | Make sure extension is loaded: `chrome://extensions` |
| Can't access Google Sheet | Add sheets.readonly scope in manifest + Google Cloud Console |
| Emails not sending | Check Gmail API is enabled in Google Cloud Console |
| Tracking not working | Verify tracking server URL or use local storage mode |

## 📊 Analytics Data Format

Campaign data stored in `chrome.storage.local`:
```javascript
{
  campaignId: "camp_12345",
  subject: "Hello {FirstName}",
  body: "...",
  sentAt: 1650000000000,
  recipients: [
    {
      email: "user@example.com",
      name: "John Doe",
      msgId: "gmail_msg_123",
      opened: true,
      openedAt: 1650000300000,
      replied: true,
      repliedAt: 1650001000000,
      replySnippet: "Thanks for reaching out...",
      tags: ["interested"]
    }
  ],
  stats: {
    total: 100,
    opened: 45,
    replied: 12,
    notOpened: 55
  }
}
```

## 📝 License

MIT License - see LICENSE file for details

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create feature branch (`git checkout -b feature/NewFeature`)
3. Commit changes (`git commit -m 'Add NewFeature'`)
4. Push to branch (`git push origin feature/NewFeature`)
5. Open a Pull Request

## 📧 Support

For issues, questions, or suggestions:
- Open an GitHub issue
- Email: ayushsingrockavenger8@gmail.com

## 🙏 Acknowledgments

- Gmail API documentation
- Google Sheets API documentation
- Chart.js for analytics visualization
- Lucide Icons for UI icons

---

**Made with ❤️ by Ayush Aura Labs**

⭐ Star us on GitHub if you find this useful!
# 🚀 MailMerge Pro — Chrome Extension

**Production-grade bulk email sender with Google Sheets integration, open tracking, and reply detection.**

A senior full-stack engineer's solution for sending hundreds of personalized emails in one click, with live tracking dashboard and intelligent reply monitoring.

---

## 📋 Features

- ✉️ **Bulk Email Sending** — Send to hundreds of recipients from Google Sheets with full personalization
- 🔗 **Google Sheets Integration** — Pull contact data and variables directly from your spreadsheets
- 👁️ **Open Tracking** — See exactly who opened your emails and when (1×1 pixel technique)
- 💬 **Reply Detection** — Automatic polling detects replies using Gmail API
- ✨ **AI Email Composer** — Generate templates using Google Gemini API
- 📊 **Live Dashboard** — Real-time stats, charts, and campaign analytics
- 🛡️ **Error Handling** — Comprehensive error management with user-friendly messages
- 🎨 **Premium Design** — Dark mode, modern glassmorphism UI with smooth animations

---

## 🏗️ Architecture

```
Chrome Extension (Manifest V3)
├── Content Script (gmail-injector.js) → Injects sidebar into Gmail
├── Service Worker → Background tasks (reply polling every 15 min)
├── Popup (190×300px) → Quick stats and navigation
├── Sidebar (380px) → 6-step email composition workflow
├── Dashboard → Campaign overview and settings
├── Tracking Pages → Opens & replies analytics
└── Utils → API clients and business logic
```

---

## 🔧 Setup Instructions

### 1. **Prerequisites**
- Google Account with Gmail and Google Sheets access
- Chrome browser
- Node/npm (optional, for development)

### 2. **Create Google OAuth 2.0 Credentials**

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a **new project**
3. Enable APIs:
   - Gmail API
   - Google Sheets API
   - Google Identity API

4. Create **OAuth 2.0 Client ID** (Desktop application)
5. Copy the Client ID and add to [`manifest.json`](manifest.json):
   ```json
   "oauth2": {
     "client_id": "YOUR_CLIENT_ID.apps.googleusercontent.com"
   }
   ```

### 3. **Get Gemini API Key (Optional, for AI Compose)**

1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a free API key
3. Configure in Settings tab

### 4. **Setup Tracking Server (Optional)**

For open tracking, you need a simple server to log pixel requests:

**Firebase Cloud Function (Free):**
```javascript
exports.trackPixel = functions.https.onRequest((req, res) => {
  const campaignId = req.path.split('/')[1];
  const recipientEmail = Buffer.from(req.path.split('/')[2], 'base64').toString();
  
  // Store in Firestore
  admin.firestore().collection('opens').add({
    campaignId,
    email: recipientEmail,
    timestamp: new Date(),
    ipAddress: req.ip
  });
  
  // Return 1×1 pixel
  res.set('Content-Type', 'image/png');
  res.send(Buffer.from('iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNkYPhfDwAChwGA60e6kgAAAABJRU5ErkJggg==', 'base64'));
});
```

### 5. **Install Extension**

1. Clone/download this repository
2. Open `chrome://extensions`
3. Enable **Developer mode** (top-right)
4. Click **Load unpacked**
5. Select the `mailmerge-pro` folder

✅ Extension now appears in Chrome toolbar!

---

## 📖 How to Use

### **Step 1: Connect Google Sheet**
1. Click extension icon → **"Compose Bulk Email"**
2. In the sidebar, paste your Google Sheets URL
3. Extension auto-detects columns and recipients

### **Step 2: Map Columns (Auto)**
- `Email` column detected as recipient field ✅
- Other columns become personalization variables: `{name}`, `{company}`, etc.

### **Step 3: Compose Email**
- Write subject with variables: `Hi {name}, opportunity from {company}`
- Write body (supports rich text)
- Click variable chips to insert `{tokens}`
- Click **✨ AI Compose** to generate with Gemini

### **Step 4: Preview**
- See email preview for first recipient
- Navigate through rows to verify personalization
- Use **🧪 Test Email** to send to yourself

### **Step 5: Configure Sending**
- Select recipient range: **All**, **Rows X to Y**, or **Custom filter**
- Set delay between emails (Gmail rate limit friendly): 2-5 seconds
- Enable **👁️ Track Opens** and **💬 Track Replies**

### **Step 6: Send!**
- Click **🚀 SEND NOW**
- Watch live progress log
- Pause/stop anytime
- Error handling shows failed emails with retry

### **View Analytics**
- Click **Tracking Dashboard** tab
- See open rate, reply rate, multi-opens
- Search by name/email
- Export CSV for further analysis

---

## 📊 Dashboard Pages

### 📈 Dashboard
- Overall stats (sent, opened, replied)
- Recent campaigns table
- Sheet preview
- Quick links to all features

### 👁️ Open Tracking
- Campaign selector
- Open timeline chart
- Recipient opens table with timestamps
- Export opens as CSV
- Send reminder to unopened

### 💬 Reply Tracker
- Reply stats (count, rate, avg time)
- Reply log with snippets
- Full reply view modal
- Tag replies (Interested / Not Interested / Follow-up / Unsubscribe)
- Export all replies

### ⚙️ Settings
- Gemini API key configuration
- Tracking server URL
- About page

---

## 📁 Project Structure

```
mailmerge-pro/
├── manifest.json                    # Extension configuration
├── background/
│   └── service-worker.js           # Reply polling (15 min interval)
├── content/
│   └── gmail-injector.js           # Gmail sidebar injection
├── popup/
│   ├── popup.html                  # Quick stats popup
│   ├── popup.css                   # Design system
│   └── popup.js                    # Event handlers
├── sidebar/
│   ├── sidebar.html               # 6-step compose UI
│   ├── sidebar.css                # Sidebar styles
│   └── sidebar.js                 # Workflow logic
├── dashboard/
│   ├── dashboard.html             # Main dashboard
│   ├── dashboard.css              # Dashboard styles
│   └── dashboard.js               # Dashboard logic
├── pages/
│   ├── tracking.html              # Open/reply tracking
│   ├── tracking.css               # Tracking styles
│   ├── tracking.js                # Tracking logic
│   └── settings.html              # Settings page
├── utils/
│   ├── sheets-api.js              # Google Sheets API client
│   ├── gmail-api.js               # Gmail API client
│   ├── template-engine.js         # {variable} rendering
│   ├── tracking-engine.js         # Campaign tracking storage
│   └── ai-composer.js             # Gemini integration
└── assets/
    └── icons/                     # Extension icons
```

---

## 🎨 Design System

**Color Palette (Dark Mode):**
- Background: `#080C18` → `#0F1629`
- Cards: `rgba(255,255,255,0.04)`
- Accent Blue: `#2563EB` (primary)
- Accent Green: `#10B981` (success)
- Accent Red: `#EF4444` (warning)
- Text Primary: `#F1F5F9`
- Text Secondary: `#64748B`

**Typography:**
- DM Sans (body text, 13-16px)
- JetBrains Mono (code, variables, 11-12px)

**Components:**
- Glass cards with backdrop blur
- Gradient buttons with glow
- Smooth transitions (0.2-0.4s cubic-bezier)
- Glassmorphism for layered UI

---

## 🔐 Security & Permissions

This extension requests:
- **Gmail**: Send emails, read threads (reply detection)
- **Sheets**: Read-only access to spreadsheets
- **Identity**: OAuth 2.0 authentication (no credentials stored)
- **Storage**: Local data for campaigns & drafts
- **Alarms**: Background reply polling (15 min intervals)

**Data Stored Locally:**
- Campaign records (recipients, open/reply status)
- Draft emails
- Settings (API keys encrypted recommended)

**No external data** except to Google APIs and your tracking server.

---

## ⚡ Key Classes & APIs

### `SheetsAPI`
```javascript
const sheets = new SheetsAPI();
await sheets.authenticate();
const data = await sheets.fetchSheetData(sheetId);
const vars = sheets.detectVariables(data.headers);
```

### `TemplateEngine`
```javascript
const engine = new TemplateEngine();
const rendered = engine.render('Hi {name}', { name: 'Rahul' });
const validation = engine.validate(subject, body, columns);
const previews = engine.previewAll(subject, body, allRows);
```

### `GmailAPI`
```javascript
const gmail = new GmailAPI(token);
const msgId = await gmail.sendEmail(emailData);
await gmail.sendBulk(emails, { delayMs: 2000, onProgress, onSuccess });
const reply = await gmail.checkForReply(messageId);
```

### `TrackingEngine`
```javascript
const tracking = new TrackingEngine();
await tracking.storeCampaignRecord(campaignId, sentItems);
const campaign = await tracking.getCampaign(campaignId);
const stats = tracking.getStats(campaign);
tracking.downloadCSV(campaignId, campaign);
```

---

## 🐛 Error Handling

| Error | Resolution |
|-------|-----------|
| OAuth token expired | Re-authenticate via sign-out → sign-in |
| Sheet not accessible | Share sheet with "Anyone with link" |
| No email column | Rename a column to "Email" |
| Gmail 429 (Rate Limit) | Auto-retry with 60s pause |
| Network offline | Emails queue, retry when online |
| API quota exceeded | Check Cloud Console usage limits |

---

## 🚀 Deployment Checklist

- [ ] Replace `YOUR_GOOGLE_OAUTH_CLIENT_ID` in manifest.json
- [ ] Set up Gemini API key (optional)
- [ ] Configure tracking server URL
- [ ] Create extension icon (16×16, 48×48, 128×128 PNG)
- [ ] Test all 6 compose steps
- [ ] Verify open tracking with test email
- [ ] Check reply detection with manual reply
- [ ] Test error scenarios (no sheet, invalid export, etc.)
- [ ] Package as .zip for distribution

---

## 📚 Technical Highlights

✅ **Chrome Manifest V3** (latest security standards)
✅ **OAuth 2.0** (no password storage)
✅ **Email tracking** (industry-standard 1×1 pixel)
✅ **Service Worker** (background polling every 15 min)
✅ **chrome.storage.sync** (cross-device persistence)
✅ **Template engine** (regex-based variable interpolation)
✅ **Error recovery** (graceful degradation)
✅ **Rate limiting** (Gmail API friendly)
✅ **Abort controller** (pause/stop sending)
✅ **CSV export** (Campaign analytics)

---

## 📞 Support & Feedback

For issues, feature requests, or contributions:
1. Check this README first
2. Review Chrome console for errors: `Ctrl+Shift+J`
3. Test with sample sheet: [template](https://docs.google.com/spreadsheets/d/1example)

---

## 📄 License

Built as a production-grade project. Use freely for personal/commercial purposes.

---

**Built with ❤️ as a senior full-stack solution for bulk personalized email campaigns.**

v1.0 | 2024
