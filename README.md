# 📲 Missed Call Text Back — Self-Hosted

> **Revenue Recovery for Trades** · Built on Twilio + Vercel · ~$5–15/mo vs $297/mo GHL

A lightweight, deployable missed-call SMS autoresponder with optional AI conversation layer. Built for HVAC, plumbing, electrical, and other trades businesses in the Lower Mainland (and anywhere else).

---

## ✅ What It Does

1. Customer calls your Twilio number → goes unanswered
2. Twilio fires a webhook to your Vercel function
3. An SMS instantly goes out: *"Sorry we missed you — how can we help?"*
4. Customer replies → optional AI (Claude) handles the conversation
5. You get notified by SMS or email

---

## 💰 Cost Breakdown

| Item | Cost |
|------|------|
| Twilio phone number | ~$1.15 CAD/mo |
| SMS per message (Twilio) | ~$0.01 CAD each |
| Vercel hosting | Free tier |
| Claude AI replies (optional) | Pay-per-use (~pennies) |
| **Total** | **~$5–15/mo** |

vs. GoHighLevel at **$297 USD/mo**

---

## 🗂 Project Structure

```
missed-call-textback/
├── api/
│   ├── missed-call.js        # Webhook: handles missed call → sends SMS
│   ├── inbound-sms.js        # Webhook: handles customer reply → AI response
│   └── health.js             # Health check endpoint
├── lib/
│   ├── sms.js                # Twilio SMS helper
│   ├── ai.js                 # Claude AI conversation handler
│   └── notify.js             # Owner notification (SMS/email)
├── config/
│   └── client.js             # Per-client configuration (editable)
├── admin/
│   └── index.html            # Simple admin dashboard (optional)
├── scripts/
│   └── test-webhook.sh       # Local testing script
├── .env.example              # Environment variable template
├── vercel.json               # Vercel deployment config
└── package.json
```

---

## 🚀 Quick Deploy (15 minutes)

### 1. Clone & Install
```bash
git clone https://github.com/YOUR_USERNAME/missed-call-textback.git
cd missed-call-textback
npm install
```

### 2. Set Environment Variables
```bash
cp .env.example .env
# Edit .env with your credentials (see below)
```

### 3. Deploy to Vercel
```bash
npm install -g vercel
vercel deploy --prod
```
Copy the deployment URL (e.g. `https://your-app.vercel.app`)

### 4. Configure Twilio
- Go to Twilio Console → Phone Numbers → your number
- Under **Voice & Fax**:
  - Set "A Call Comes In" → Webhook → `https://your-app.vercel.app/api/missed-call`
  - Set "Call Status Changes" → `https://your-app.vercel.app/api/missed-call`
- Under **Messaging**:
  - Set "A Message Comes In" → Webhook → `https://your-app.vercel.app/api/inbound-sms`

### 5. Test It
```bash
bash scripts/test-webhook.sh
```

---

## 🔐 Environment Variables

```env
# Twilio
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=+16045550000

# Business Owner Notification
OWNER_PHONE=+16045559999
OWNER_EMAIL=you@yourdomain.com   # optional

# Claude AI (optional — remove to use static replies)
ANTHROPIC_API_KEY=sk-ant-...

# Client Config
BUSINESS_NAME=Vancouver HVAC Pro
BOOKING_LINK=https://calendly.com/yourbusiness
```

---

## 🤖 AI Mode vs. Static Mode

**Static mode** (no `ANTHROPIC_API_KEY`): Sends a fixed SMS template.

**AI mode** (with key): Claude handles the conversation — answers FAQs, collects name/issue, offers booking link, escalates emergencies.

Set your business context in `config/client.js`.

---

## 🔁 White-Label / Multi-Client

Each client gets their own Vercel deployment with their own `.env`.

```bash
# Deploy client #2
vercel deploy --prod -e BUSINESS_NAME="Delta Plumbing" \
  -e TWILIO_PHONE_NUMBER=+16045550001 \
  -e BOOKING_LINK=https://deltaplumbing.ca/book
```

Charge $97–$197/mo per client. Your cost: ~$10–20/mo. Margin: 85–90%.

---

## 📊 Admin Dashboard

Open `admin/index.html` in a browser or deploy it alongside your API. Shows:
- Messages sent today / this month
- Recent conversations
- Quick config editor

---

## 📋 License

MIT — use it, resell it, modify it. Attribution appreciated but not required.
