# 💣 PAYORBIT — COMPLETE PROJECT BLUEPRINT

> **What is PayOrbit?**  
> A Splitwise-like app where friends can track shared expenses, settle debts, and manage personal spending — available as both a **mobile app** and a **web app**.

---

## 📐 ARCHITECTURE OVERVIEW

```mermaid
graph TB
    subgraph "Frontend - What Users See"
        MA["📱 Mobile App - React Native + Expo"]
        WA["🌐 Web App - Next.js"]
    end

    subgraph "Backend - Brain of the App"
        API["⚙️ Express.js API Server"]
        AUTH["🔐 Auth Middleware - JWT"]
        SE["🧮 Split Engine"]
        IE["🤖 Insights Engine"]
    end

    subgraph "Database and Storage"
        DB["🗄️ PostgreSQL via Prisma ORM"]
        CACHE["⚡ Redis - Caching + Rate Limit"]
        STORE["📦 Cloudinary - Image Storage"]
    end

    subgraph "External Services"
        PAY["💳 Razorpay - Payments"]
        EMAIL["📧 Resend - Email via SMTP"]
        PUSH["🔔 Firebase FCM - Push Notifications"]
        OCR["🧾 Tesseract.js - Receipt Scanning"]
    end

    MA --> API
    WA --> API
    API --> AUTH
    API --> SE
    API --> IE
    API --> DB
    API --> CACHE
    API --> STORE
    API --> PAY
    API --> EMAIL
    API --> PUSH
    API --> OCR
```

### How Data Flows (Simple Explanation)

1. **User opens app** → React Native (mobile) or Next.js (web)
2. **User does something** (e.g. adds expense) → sends HTTP request to Express.js backend
3. **Backend validates** → JWT token check, Zod input validation
4. **Backend processes** → Split Engine calculates who owes what
5. **Backend saves** → Prisma writes to PostgreSQL database
6. **Backend responds** → JSON response back to frontend
7. **Side effects** → Email notification sent via SMTP, push via FCM

---

## 🔍 COMPETITIVE ANALYSIS

### 🇮🇳 Indian Market

| App | What It Does | Strengths | Weaknesses | PayOrbit's Edge |
|-----|-------------|-----------|------------|----------------|
| **Splitwise** | Expense splitting + group tracking | Market leader, huge user base, polished UI, offline sync | **Freemium model** — charts/receipt scanning locked behind ₹400/mo Pro plan. No UPI integration. Foreign company, servers outside India | We offer analytics + receipt scanning + AI insights **for free**. Indian payment (Razorpay UPI) built-in |
| **Settle Up** | Group expense splitting | Clean UI, offline-first, free with no ads | No payment integration at all. No analytics. Basic feature set. Small team = slow updates | We have payments, analytics, insights, goals — full ecosystem |
| **Walnut** (by Axio) | Personal expense tracker | Auto-reads SMS to track spends, good category analytics | **Only personal expenses** — no group splitting at all. Privacy concerns (reads all SMS). Recently pivoted to credit | We combine personal + group expenses in one app |
| **Khatabook** | Business ledger/accounting | Popular among shopkeepers (50M+ downloads), simple design | **Business-focused** — not built for friends splitting bills. No analytics. No payment settlements | Different target audience entirely — we target students/friends |
| **CRED** | Credit card payments + rewards | Beautiful UI, rewards system, trusted brand | Not an expense splitter. Only credit card focused. Requires high credit score to join | We're complementary — users can use CRED for payments, PayOrbit for splitting |
| **mPokket / KreditBee** | Student loans/lending | Targets college students | Lending apps, not expense management | Completely different category |

### 🌍 International Market

| App | Country | What It Does | How PayOrbit Compares |
|-----|---------|-------------|----------------------|
| **Splitwise** | USA (Global) | The gold standard for expense splitting | We match core features + add AI insights + Indian payments. They charge for features we give free |
| **Tricount** | Belgium (EU) | Group expense splitting, popular in Europe | No payment integration, no analytics, no AI. Simple but limited |
| **Tab** | USA | Bill splitting with photo scanning | Only restaurant bills. Very narrow use case. We cover all expense types |
| **Spliit** | France | Open-source expense splitter | Web-only, no mobile app, no payments, no analytics |
| **Venmo** | USA | Peer-to-peer payments with social feed | Payment-first, splitting is secondary. Not available in India |
| **Revolut / Wise** | UK/EU | Multi-currency with bill splitting | Banking app with splitting as a feature. Overkill for students |

### 💡 PayOrbit's Competitive Advantages

| Advantage | Why It Matters |
|-----------|---------------|
| **All-in-one** | Personal tracking + group splitting + payments + analytics in ONE app (competitors do 1-2 of these) |
| **AI Insights FREE** | Splitwise charges ₹400/mo for charts. We give AI-powered insights for free |
| **Indian-first payments** | UPI/Razorpay built-in. Splitwise still doesn't support UPI natively |
| **Student-focused pricing** | No subscriptions, no premium walls. Free features that competitors lock behind paywalls |
| **Receipt scanning FREE** | Splitwise Pro feature (₹400/mo). We use Tesseract.js — zero cost |
| **Offline support** | Works without internet — important in India where connectivity varies |
| **Open to grow** | Goals, advisor, disputes — features no competitor in India offers together |

### 🎯 Our Target Users vs Competitors

```
Splitwise  → Everyone (but charges for good features)
Walnut     → Individual expense trackers
Khatabook  → Shopkeepers / small businesses
CRED       → Credit card users with high scores

PayOrbit   → College students & young professionals who:
             ✓ Split bills with friends daily
             ✓ Want to track personal spending too
             ✓ Need UPI payments built-in
             ✓ Can't afford ₹400/mo subscriptions
             ✓ Want smart insights without paying extra
```

---

## 🛠️ TECH STACK — TOOLS, WHY, AND ALTERNATIVES

### Frontend

| Tool | Why We Use It | Cost | Alternative | Why Not Alternative |
|------|--------------|------|-------------|-------------------|
| **React Native + Expo** | One codebase for iOS + Android. Expo gives easy build/deploy | **Free** | Flutter | Dart language has smaller JS community; RN uses JavaScript which we already know |
| **Next.js** | Web app with SSR, routing, great DX | **Free** | Vite + React | No SSR out of box; Next.js is more production-ready |
| **Zustand** | Simple state management (like a global variable store) | **Free** | Redux | Redux has too much boilerplate code for our scale |
| **React Query (TanStack)** | Automatic API caching, refetching, loading states | **Free** | SWR | React Query has better devtools and mutation support |
| **React Navigation** | Screen navigation in mobile app | **Free** | Expo Router | Either works; React Navigation has more community resources |
| **Victory Native / Recharts** | Charts for analytics dashboard | **Free** | Chart.js | Victory has better React Native support |
| **Lottie** | Smooth micro-animations (loading, success, etc.) | **Free** | CSS animations | Lottie gives designer-quality animations easily |

### Backend

| Tool | Why We Use It | Cost | Alternative | Why Not Alternative |
|------|--------------|------|-------------|-------------------|
| **Node.js + Express** | Fast, simple, huge community. We already know JS | **Free** | Fastify | Express has more tutorials/community for beginners |
| **Prisma ORM** | Write database queries in JavaScript instead of raw SQL | **Free** | Drizzle ORM | Prisma has better docs, auto-migrations, studio GUI |
| **PostgreSQL** | Reliable, free, handles complex queries well | **Free** (local) / **Free tier** on Supabase/Neon | MySQL | PostgreSQL handles JSON, arrays better |
| **JWT (jsonwebtoken)** | Stateless authentication tokens | **Free** | Passport.js | JWT is simpler; Passport adds unnecessary complexity for our API |
| **Zod** | Input validation — checks if user sent correct data format | **Free** | Joi | Zod has TypeScript-first design, smaller bundle |
| **bcrypt** | Password hashing (one-way encryption) | **Free** | Argon2 | bcrypt is battle-tested and widely used |

### External Services — Smart Budget (Quality First, Cost Second)

The goal is NOT "₹0 at all costs" — the goal is a **production-quality app that doesn't feel broken or slow**. Here's what to use and when to upgrade:

#### 🟢 Genuinely Free (No compromise)

These services are **fully featured on free tier** — no catch, no quality loss:

| Service | Purpose | Free Tier Limit | Quality Impact |
|---------|---------|----------------|----------------|
| **Firebase FCM** | Push notifications to phones | **Unlimited** — completely free | ✅ Zero compromise. Same as what WhatsApp uses |
| **Vercel** | Host Next.js web app | 100GB bandwidth/mo, auto SSL, edge CDN | ✅ Production-grade. Handles thousands of users |
| **Tesseract.js** | OCR for receipt scanning | **Unlimited** — runs on your server | ✅ Good accuracy for printed receipts. No API calls needed |
| **Cloudinary** | Store receipt images | 25GB storage, 25K transforms/mo | ✅ More than enough for thousands of receipts |
| **Upstash Redis** | Caching + rate limiting | 10K commands/day | ✅ Fine for dev + early production |

#### 🟡 Free to Start, Upgrade When You Grow

These work on free tier for development/testing, but you'll want to pay a small amount for production:

| Service | Free Tier | Free Tier Problem | Recommended Paid Plan | Monthly Cost |
|---------|-----------|-------------------|----------------------|-------------|
| **Render** (backend hosting) | 750 hrs/mo | ⚠️ **Spins down after 15 min idle** — first request takes 30-50 seconds. Users will think app is broken | Render Starter — always on, auto-scaling | **$7/mo (~₹580)** |
| **Supabase** (PostgreSQL DB) | 500MB, 2 projects | ⚠️ **Pauses after 1 week of inactivity** on free tier. 500MB fills up fast with activity logs | Supabase Pro — 8GB, no pausing, daily backups | **$25/mo (~₹2,100)** BUT free tier works for first 6 months easily |
| **Neon** (PostgreSQL DB — alternative) | 512MB, 1 project | ⚠️ Scales to zero (cold start delay) | Neon Launch — always on, 10GB | **$19/mo (~₹1,600)** |
| **Resend** (email) | 100 emails/day | ⚠️ 100/day is tight if you have 50+ active users sending reminders | Resend Pro — 50K emails/mo | **$20/mo (~₹1,700)** BUT 100/day is fine until 50+ users |

#### 🔴 Always Paid (But Worth It)

| Service | What You Pay | Why It's Worth It |
|---------|-------------|------------------|
| **Razorpay** | 2% per transaction (no monthly fee) | Only pay when money moves. UPI, cards, netbanking — all work out of the box |
| **Google Play Store** | ₹2,000 one-time | Required to publish Android app. One-time, lifetime |
| **Apple Developer** | $99/year (~₹8,300) | Only if you want iOS App Store. Skip initially, use TestFlight for testing |

#### 💰 Realistic Budget Breakdown

| Phase | Monthly Cost | What You Get |
|-------|-------------|-------------|
| **Development** (building the app) | **₹0** | Free tiers are perfect for dev/testing |
| **Soft Launch** (10-50 users) | **~₹580/mo** | Upgrade only Render to avoid cold starts |
| **Production** (50-500 users) | **~₹2,000-3,000/mo** | Render + Supabase Pro + more email capacity |
| **Scale** (500+ users) | **~₹5,000-8,000/mo** | Everything upgraded, Redis paid, CDN |

> **💡 BOTTOM LINE**: Start with ₹0 during development. When you launch to real users, budget **₹500-600/mo minimum** (just for Render) so your app doesn't feel slow. That's less than a pizza dinner.

---

## 📡 PROTOCOLS EXPLAINED (Simple Language)

| Protocol | What It Does | Where We Use It |
|----------|-------------|-----------------|
| **HTTPS** | Secure web communication (encrypted HTTP) | All API calls between app and backend |
| **SMTP** (Simple Mail Transfer Protocol) | The protocol used to SEND emails. Think of it like the postal system for emails | Sending OTP, payment reminders, settlement confirmations via Resend |
| **WebSocket** | Two-way real-time connection (server can push data to client without client asking) | Real-time notifications, live expense updates in groups |
| **FCM (Firebase Cloud Messaging)** | Google's protocol for sending push notifications to phones | "You have a pending payment" notifications |
| **OAuth 2.0** | "Login with Google/GitHub" — lets users sign in without creating a password | Social login (future feature) |
| **JWT** | A signed token (like a digital ID card) that proves who the user is | Every authenticated API request carries this token |

> **📝 NOTE — SMTP vs HTTPS**: When you send an email reminder, your backend talks to Resend's servers using SMTP (port 587 with TLS encryption), NOT HTTPS. SMTP is specifically designed for email delivery. Resend also offers an HTTPS API wrapper, but underneath it still uses SMTP.

---

## 🗄️ DATABASE DESIGN

```mermaid
erDiagram
    User ||--o{ Expense : creates
    User ||--o{ ExpenseSplit : participates
    User ||--o{ Group : creates
    User }o--o{ GroupMember : belongs_to
    User ||--o{ Settlement : pays
    User ||--o{ Settlement : receives
    User ||--o{ Activity : logs
    User ||--o{ Goal : sets
    Group ||--o{ Expense : contains
    Group ||--o{ GroupMember : has
    Expense ||--o{ ExpenseSplit : splits_into
    Expense ||--o{ Settlement : settles

    User {
        int id PK
        string name
        string email
        string phone
        string passwordHash
        string avatarUrl
        datetime createdAt
    }

    Expense {
        int id PK
        string description
        decimal totalAmount
        string splitType
        string category
        bool isPersonal
        int createdById FK
        int groupId FK
        datetime createdAt
    }

    ExpenseSplit {
        int id PK
        int expenseId FK
        int userId FK
        decimal amountOwed
        bool isPaid
    }

    Group {
        int id PK
        string name
        int createdById FK
        datetime createdAt
    }

    GroupMember {
        int id PK
        int groupId FK
        int userId FK
        string role
    }

    Settlement {
        int id PK
        int payerId FK
        int receiverId FK
        decimal amount
        string status
        int expenseId FK
        datetime createdAt
    }

    Activity {
        int id PK
        string type
        string message
        int userId FK
        int groupId FK
        datetime createdAt
    }

    Goal {
        int id PK
        string title
        decimal targetAmount
        decimal savedAmount
        datetime deadline
        int userId FK
    }
```

---

## 📱 FEATURE-BY-FEATURE BREAKDOWN

---

### 🧑‍💻 1. AUTH & USER SYSTEM

**What it does**: Lets users create accounts, log in, and manage their profile.

#### How It Works

```
User types email + password → Backend hashes password with bcrypt → Saves to DB
User logs in → Backend checks hash → Creates JWT token → Sends token back
Every future request → App sends JWT in header → Backend verifies → Allows access
```

#### Screens

| Screen | Mobile (React Native) | Web (Next.js) |
|--------|----------------------|---------------|
| **Login** | Full-screen form with email/phone + password, animated logo at top, "Login with Google" button | Clean centered card layout, same fields |
| **Signup** | Step-by-step: name → email → password → phone (optional) | Single form with all fields |
| **Profile** | Avatar (tap to change), name edit, phone edit, logout button at bottom | Settings page with sidebar navigation |

#### API Endpoints

| Method | Route | What It Does |
|--------|-------|-------------|
| POST | `/auth/signup` | Create new account |
| POST | `/auth/login` | Get JWT token |
| GET | `/auth/me` | Get current user profile |
| PUT | `/auth/profile` | Update name/avatar/phone |

#### Security Measures
- Passwords are **never stored as plain text** — always hashed with bcrypt (10 salt rounds)
- JWT tokens **expire after 7 days** — user must re-login
- Rate limiting: **5 login attempts per 15 minutes** (prevents brute force)
- Input validation with Zod (prevents SQL injection, XSS)

---

### 💸 2. EXPENSE SYSTEM (CORE)

**What it does**: The heart of PayOrbit. Add expenses and split them between people.

#### Split Engine Logic (How Splitting Works)

```
EQUAL SPLIT:
  Total = ₹600, 3 people → Each owes ₹200

CUSTOM SPLIT:
  Total = ₹600
  Person A: ₹300, Person B: ₹200, Person C: ₹100
  (Must add up to ₹600 or backend rejects it)

PERSONAL (no split):
  Only you, no group, just tracking your own spend
```

#### Screens

| Screen | Mobile | Web |
|--------|--------|-----|
| **Add Expense** | Bottom sheet modal: amount input (big numbers), description, category picker (chips/tags), split type toggle, member selector | Side panel or modal dialog with same fields |
| **Expense Detail** | Full screen: shows who paid, who owes what, split breakdown, notes, category badge, timestamp | Card view with expandable split details |

#### Categories (with emoji icons)

🍕 Food · 🚗 Travel · 🏠 Rent · 🎬 Entertainment · 🛒 Shopping · 💊 Health · 📚 Education · 💡 Utilities · 🎁 Gifts · 📦 Other

#### API Endpoints

| Method | Route | What It Does |
|--------|-------|-------------|
| POST | `/expense` | Create expense (with split calculation) |
| GET | `/expense` | List expenses (with pagination) |
| GET | `/expense/:id` | Single expense detail |
| PUT | `/expense/:id` | Edit expense |
| DELETE | `/expense/:id` | Delete expense |

---

### 👥 3. GROUP SYSTEM

**What it does**: Create groups (like "Goa Trip", "Flat Rent") and track shared expenses within them.

#### Screens

| Screen | Mobile | Web |
|--------|--------|-----|
| **Group List** | Card list with group name, member avatars (stacked circles), total balance shown | Grid/list view with search and filter |
| **Group Dashboard** | Header: group name + member count. Tabs: Expenses / Balances / Settings. FAB button to add expense | Three-column layout: expenses list, balance summary, group settings |
| **Add Members** | Search by email/phone, show user card, tap to add | Same with autocomplete dropdown |

#### API Endpoints

| Method | Route | What It Does |
|--------|-------|-------------|
| POST | `/group` | Create group |
| GET | `/group` | List user's groups |
| GET | `/group/:id` | Group details + members |
| POST | `/group/:id/member` | Add member |
| DELETE | `/group/:id/member/:userId` | Remove member |

---

### 📊 4. BALANCE SYSTEM

**What it does**: Shows who owes whom and how much. The "main screen" of the app.

#### Simplified Debts Algorithm

```
Without simplification:
  A owes B ₹500
  B owes A ₹300
  = 2 transactions

With simplification:
  A owes B ₹200
  = 1 transaction (much cleaner!)
```

The backend runs a **net balance algorithm**: for each pair of users, it calculates the net difference and shows only the minimum number of transactions needed.

#### Screens

| Screen | Mobile | Web |
|--------|--------|-----|
| **Dashboard** | Top: total you owe / total owed to you. Cards for each person with their balance (green = they owe you, red = you owe them). Pull-to-refresh | Dashboard with summary cards at top, detailed balance list below |

#### API Endpoints

| Method | Route | What It Does |
|--------|-------|-------------|
| GET | `/balances` | All balances for current user |
| GET | `/balances/group/:id` | Balances within a specific group |

---

### 📜 5. ACTIVITY FEED

**What it does**: A timeline showing everything that happened — who added what expense, who settled up, etc.

#### Activity Types
- `EXPENSE_ADDED` — "Abhinav added ₹500 for Dinner"
- `EXPENSE_UPDATED` — "Abhinav updated Pizza expense"
- `EXPENSE_DELETED` — "Abhinav deleted Taxi expense"
- `SETTLEMENT_MADE` — "Rahul paid ₹300 to Abhinav"
- `MEMBER_ADDED` — "Priya was added to Goa Trip"
- `MEMBER_REMOVED` — "Rohit was removed from Flat Rent"

#### Screen

| Screen | Mobile | Web |
|--------|--------|-----|
| **Activity Timeline** | Vertical timeline with icons, colored dots per type, timestamp, tap to see related expense | Same layout, wider cards |

#### API Endpoint

| Method | Route | What It Does |
|--------|-------|-------------|
| GET | `/activity` | Paginated activity feed |

---

### 💰 6. SETTLEMENT SYSTEM

**What it does**: Mark debts as paid. Support full and partial settlements.

#### Flow

```
1. User sees "You owe Rahul ₹500"
2. Taps "Settle Up"
3. Chooses: Full (₹500) or Partial (₹200)
4. Confirms → Backend creates Settlement record
5. Balance updates automatically
6. Activity logged: "You paid ₹500 to Rahul"
7. Rahul gets push notification
```

#### Screens

| Screen | Mobile | Web |
|--------|--------|-----|
| **Settlement Modal** | Bottom sheet: shows amount owed, slider/input for partial amount, "Settle" button | Modal dialog with same |
| **Payment Confirmation** | Success animation (Lottie checkmark), "Done" button | Same with confetti animation |

#### API Endpoints

| Method | Route | What It Does |
|--------|-------|-------------|
| POST | `/settlement` | Create settlement |
| GET | `/settlement` | List settlements |
| PUT | `/settlement/:id` | Update (mark verified) |

---

### 📈 7. ANALYTICS

**What it does**: Visual charts showing spending patterns — category-wise, monthly trends.

#### Data Processing (Backend)
- Groups personal expenses by category → pie chart data
- Groups by month (last 12) → line chart data
- Zero-fills missing months so charts don't have gaps

#### Screens

| Screen | Mobile | Web |
|--------|--------|-----|
| **Charts Dashboard** | Scrollable: Pie chart (category breakdown) → Line chart (monthly trend) → Summary cards (total spent, top category, this vs last month) | Grid layout: charts side by side, summary below |

#### API Endpoints

| Method | Route | What It Does |
|--------|-------|-------------|
| GET | `/analytics/categories` | Category breakdown |
| GET | `/analytics/monthly` | Last 12 months trend |
| GET | `/analytics/summary` | Quick summary stats |

---

### 🔔 8. SMART REMINDERS

**What it does**: Sends notifications for pending payments.

#### How Notifications Work

```
Backend → Firebase FCM → Google's servers → User's phone
Backend → Resend (SMTP) → Email server → User's inbox
```

#### Types of Reminders
- **Pending payment** (you owe someone > 3 days)
- **Settlement received** (someone paid you)
- **New expense added** (in your group)
- **Weekly summary** (your spending this week)

#### Screen

| Screen | Mobile | Web |
|--------|--------|-----|
| **Notification Panel** | Bell icon in header with badge count. Dropdown list of notifications. Tap to navigate to related screen | Dropdown panel from bell icon in navbar |

---

### 🤖 9. AI INSIGHTS

**What it does**: Rule-based smart tips about spending (no paid AI API needed!).

#### How It Works (No ChatGPT Needed!)

We use **deterministic rules** — simple if/else logic that generates human-readable insights:

```javascript
// Rule 1: Monthly spike
if (thisMonth > lastMonth * 1.3) → "You spent 30% more this month!"

// Rule 2: Category dominance  
if (topCategory > 40% of total) → "Food takes up 45% of your spending"

// Rule 3: Recent burst
if (last3Days > monthlyAvg * 0.5) → "Heavy spending in last 3 days"
```

#### Screen

| Screen | Mobile | Web |
|--------|--------|-----|
| **Insights Cards** | Horizontal scrollable cards on dashboard with emoji + message + color coding | Same cards in a grid |

#### API Endpoint

| Method | Route | What It Does |
|--------|-------|-------------|
| GET | `/insights` | Get rule-based spending insights |

---

### 🧾 10. RECEIPT SCANNING

**What it does**: Take a photo of a receipt → auto-fill expense amount and description.

#### How It Works

```
User takes photo → Image uploaded to Cloudinary → 
Cloudinary URL sent to backend → Tesseract.js extracts text →
Regex finds amount (₹ or total) → Returns pre-filled data to app
```

**Tesseract.js** is FREE and runs on the server — no Google/AWS API costs!

#### Screen

| Screen | Mobile | Web |
|--------|--------|-----|
| **Upload Receipt** | Camera button on Add Expense screen → capture/pick image → loading spinner → fields auto-fill | File upload area (drag and drop) → same processing |

---

### 💳 11. PAYMENT SYSTEM (Razorpay)

**What it does**: Actually send money to settle debts — not just mark as paid.

#### How Razorpay Works

```
1. Frontend calls backend: "I want to pay ₹500 to Rahul"
2. Backend creates Razorpay Order (server-side) → returns order_id
3. Frontend opens Razorpay checkout (their pre-built UI)
4. User pays via UPI/Card/Netbanking
5. Razorpay sends webhook to backend: "Payment successful"
6. Backend verifies signature → creates Settlement → updates balances
```

#### Cost
- **2% per transaction** (Razorpay takes this fee)
- No monthly/setup fee
- Alternative: **Cashfree** (1.9%) or **Juspay** (varies)

#### Screens

| Screen | Mobile | Web |
|--------|--------|-----|
| **Payment Screen** | Razorpay's built-in checkout (we don't build this UI) | Same embedded checkout |
| **Payment Success** | Lottie animation + receipt summary | Same |

---

### ⚠️ 12. DISPUTE SYSTEM

**What it does**: If someone adds a wrong expense, others can flag/dispute it.

#### Flow

```
1. User sees expense they disagree with
2. Taps "Report" button → selects reason
3. Expense gets flagged → creator gets notified
4. Creator can edit/delete OR dismiss dispute
5. Activity logged
```

#### Screens

| Screen | Mobile | Web |
|--------|--------|-----|
| **Report Button** | Three-dot menu on expense card → "Report/Dispute" option | Same |
| **Dispute Page** | Form: reason dropdown + optional comment + submit | Same in modal |

---

### 📶 13. OFFLINE SUPPORT

**What it does**: Add expenses even without internet. Syncs when you're back online.

#### How It Works

```
No internet → Expense saved to AsyncStorage (mobile) / IndexedDB (web)
Internet returns → App detects → Sends all queued expenses to backend
If conflict (someone else edited) → show conflict resolution screen
```

#### Libraries Used
- **AsyncStorage** (React Native) — simple key-value storage on phone
- **IndexedDB** (Web) — browser's built-in database
- **NetInfo** (React Native) — detects online/offline status

---

## 🎁 EXTRA FEATURES (SUGGESTIONS)

### 🎯 Goals / Wishlist System
- Set savings goals ("New Laptop — ₹50,000")
- Track progress with a progress bar
- Get a plan: "Save ₹8,333/month for 6 months"
- Smart suggestions: "Reduce Food spending by ₹1,000/month"

### 🧠 Smart Advisor
- Actionable spending recommendations
- "Try to keep Food under ₹4,000 this month"
- Different from Insights (insights = past, advisor = future)

### 🌙 Dark Mode
- Toggle between light/dark themes
- Uses system preference by default

### 📤 Export to PDF/CSV
- Download expense reports
- Use **jsPDF** (free) for PDF generation
- **csv-stringify** for CSV export

### 🗣️ Multi-Language Support (i18n)
- Hindi, English to start
- Use **react-i18next** library (free)

### 👥 Settle All Button
- One-tap settle all debts with one person

### 📊 Group Analytics
- Per-group spending charts
- "Who spends the most in this group?"

---

## 🏗️ FOLDER STRUCTURE

```
PayOrbit/
├── backend/
│   ├── prisma/
│   │   └── schema.prisma
│   ├── src/
│   │   ├── app.js
│   │   ├── middleware/
│   │   │   └── auth.js
│   │   └── utils/
│   │       ├── prisma.js
│   │       ├── splitEngine.js
│   │       ├── validators.js
│   │       ├── insights.js
│   │       └── activity.js
│   ├── package.json
│   └── .env
│
├── mobile/
│   ├── src/
│   │   ├── screens/
│   │   ├── components/
│   │   ├── navigation/
│   │   ├── services/
│   │   ├── store/
│   │   ├── hooks/
│   │   └── utils/
│   ├── app.json
│   └── package.json
│
├── web/
│   ├── src/
│   │   ├── app/
│   │   ├── components/
│   │   ├── services/
│   │   ├── store/
│   │   └── styles/
│   ├── next.config.js
│   └── package.json
│
└── docs/
    └── PAYORBIT_COMPLETE_BLUEPRINT.md
```

---

## 🔐 ENVIRONMENT VARIABLES

```env
# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/payorbit

# Auth
JWT_SECRET=your-super-secret-key-here
JWT_EXPIRES_IN=7d

# Razorpay (Payment)
RAZORPAY_KEY_ID=rzp_test_xxxxx
RAZORPAY_KEY_SECRET=xxxxx

# Resend (Email via SMTP)
RESEND_API_KEY=re_xxxxx

# Cloudinary (Image Storage)
CLOUDINARY_CLOUD_NAME=xxxxx
CLOUDINARY_API_KEY=xxxxx
CLOUDINARY_API_SECRET=xxxxx

# Firebase (Push Notifications)
FIREBASE_PROJECT_ID=xxxxx
FIREBASE_PRIVATE_KEY=xxxxx

# Redis (Caching)
REDIS_URL=redis://default:xxxxx@xxxxx.upstash.io:6379

# App
PORT=3000
NODE_ENV=development
```

---

## 🚀 DEPLOYMENT PLAN (Smart Budget)

### Development Phase (₹0/mo)

| Component | Where | Tier | Cost |
|-----------|-------|------|------|
| Backend API | **Render** | Free (cold starts OK for testing) | ₹0 |
| Web App | **Vercel** | Free (production quality!) | ₹0 |
| Mobile App | **Expo Go** (dev testing) | Free | ₹0 |
| Database | **Supabase** | Free (500MB) | ₹0 |
| Redis | **Upstash** | Free (10K/day) | ₹0 |
| Images | **Cloudinary** | Free (25GB) | ₹0 |
| Emails | **Resend** | Free (100/day) | ₹0 |

### Production Launch (₹580-2,500/mo)

| Component | Where | Tier | Cost |
|-----------|-------|------|------|
| Backend API | **Render** | **Starter (always on)** | ~₹580/mo |
| Web App | **Vercel** | Free (still fine!) | ₹0 |
| Mobile App | **Expo EAS** + Play Store | One-time ₹2,000 for Play Store | ₹0/mo |
| Database | **Supabase** | Free → Pro when needed | ₹0 → ₹2,100/mo |
| Redis | **Upstash** | Free → Pay-as-you-go | ₹0 → ~₹200/mo |
| Images | **Cloudinary** | Free (still fine!) | ₹0 |
| Emails | **Resend** | Free → Pro when 100/day isn't enough | ₹0 → ₹1,700/mo |

> **⚠️ IMPORTANT**: Don't ship a production app on Render free tier — the 30-50 second cold start will make users think the app is broken. The ₹580/mo Render Starter plan is the ONE upgrade you should make on day one of launch. Everything else can stay free until you have 50+ active users.

---

## 🤖 AI/LLM INTEGRATION — Gemini vs OpenAI

Since we're planning to use AI for **personalized spending analysis** (not just rule-based insights), we need an LLM API. Here's the honest comparison:

### API Cost Comparison

| Model | Input Cost (per 1M tokens) | Output Cost (per 1M tokens) | Quality | Speed |
|-------|---------------------------|----------------------------|---------|-------|
| **Gemini 2.0 Flash** | **FREE** (15 RPM, 1M TPM) / $0.10 paid | **FREE** / $0.40 paid | Very good for our use case | ⚡ Very fast |
| **Gemini 2.0 Flash Lite** | **FREE** (30 RPM) / $0.025 paid | **FREE** / $0.10 paid | Good enough for simple insights | ⚡⚡ Fastest |
| **Gemini 1.5 Pro** | $1.25 | $5.00 | Excellent | Medium |
| **OpenAI GPT-4o** | $2.50 | $10.00 | Excellent | Medium |
| **OpenAI GPT-4o-mini** | $0.15 | $0.60 | Good | Fast |
| **OpenAI GPT-3.5 Turbo** | $0.50 | $1.50 | Decent | Fast |
| **Claude 3.5 Haiku** | $0.25 | $1.25 | Good | Fast |

### 🏆 OUR RECOMMENDATION: **Gemini 2.0 Flash**

**Why Gemini over OpenAI for us:**

| Factor | Gemini 2.0 Flash | OpenAI GPT-4o-mini |
|--------|------------------|--------------------|
| **Free tier** | ✅ 15 requests/minute, 1M tokens/minute — genuinely usable | ❌ No free tier. Pay from request #1 |
| **Cost when paid** | $0.10 / 1M input tokens | $0.15 / 1M input tokens |
| **Quality** | Excellent for structured data analysis | Slightly better at creative text |
| **Speed** | Very fast | Fast |
| **Indian data centers** | ✅ Google has Mumbai region | ❌ No Indian region |
| **College student verdict** | **Start FREE, pay pennies later** | Pay from day 1 |

### How We Use AI in PayOrbit

```
┌─────────────────────────────────────────────────┐
│         WHAT WE SEND TO GEMINI                  │
├─────────────────────────────────────────────────┤
│ • Monthly spending summary (aggregated numbers) │
│ • Category breakdown (Food: ₹5K, Travel: ₹2K)  │
│ • Spending trends (this month vs last 3 months) │
│ • Goal progress (saved ₹10K of ₹50K target)     │
│                                                 │
│ ⚠️ We NEVER send:                              │
│ • Individual transaction details                │
│ • Personal info (name, email, phone)            │
│ • Group member data                             │
│ • Payment information                           │
└─────────────────────────────────────────────────┘
          ↓
┌─────────────────────────────────────────────────┐
│         WHAT GEMINI RETURNS                     │
├─────────────────────────────────────────────────┤
│ • "Your food spending jumped 40% this month.    │
│   Try cooking 2 meals/week at home to save ~₹2K"│
│ • "At your current rate, you'll hit your laptop │
│   goal in 8 months. Cut entertainment by 15%    │
│   to reach it in 5 months."                     │
│ • "You spend most on weekends. Setting a ₹500   │
│   weekend budget could save ₹3K/month."         │
└─────────────────────────────────────────────────┘
```

### 💡 Smart Strategies to Minimize AI Costs

**These strategies keep quality HIGH while costs stay near ₹0:**

| Strategy | How It Works | Savings |
|----------|-------------|--------|
| **1. Cache insights for 24 hours** | Generate AI insights once/day per user, store in Redis. Serve from cache for repeat views. A user checking insights 10 times/day = 1 API call, not 10 | **90% reduction** |
| **2. Batch analysis** | Don't call AI on every expense. Run a nightly cron job that analyzes all active users at once | **95% reduction** — 1 batch call vs 100 individual calls |
| **3. Hybrid approach** | Use **rule-based insights** (free, instant) for simple stuff. Use **AI only** for personalized advice that rules can't generate | **70% reduction** — AI handles only complex insights |
| **4. Token-efficient prompts** | Send aggregated numbers, not raw data. "Food: ₹5000, Travel: ₹2000" instead of 50 individual transactions | **80% reduction** in input tokens |
| **5. Gemini free tier first** | 15 RPM × 60 min × 24 hrs = 21,600 free requests/day. With 24h caching, that's enough for 21,600 daily active users FOR FREE | **100% free** for first 20K users |
| **6. Fallback chain** | Try Gemini free tier → If rate limited, serve cached → If no cache, serve rule-based insights. User always sees something | **Zero downtime**, near-zero cost |

### Real Cost Estimation

| Users | AI Calls/Day (with caching) | Tokens/Call | Monthly Cost (Gemini Flash) |
|-------|---------------------------|-------------|---------------------------|
| 10 | 10 | ~500 | **₹0** (free tier) |
| 100 | 100 | ~500 | **₹0** (free tier) |
| 1,000 | 1,000 | ~500 | **₹0** (free tier — 15RPM handles this) |
| 10,000 | 10,000 | ~500 | **~₹40/mo** ($0.50) |
| 50,000 | 50,000 | ~500 | **~₹200/mo** ($2.50) |

> **🔑 KEY INSIGHT**: With proper caching + batching, you can serve AI insights to **1,000+ users completely FREE** on Gemini's free tier. Even at 50,000 users, it's ₹200/mo. OpenAI would cost 3x-5x more for the same thing.

### Personal Use (For Ourselves)

Since we're also using this for personal expense tracking:
- Gemini free tier gives us **unlimited personal use**
- Our personal insights won't even dent the 15 RPM limit
- No API key cost for personal use — ever

---

## 💡 SUGGESTED BUDGET — WHAT WE RECOMMEND

*(This doesn't replace the budget above — this is our recommendation on what to actually follow)*

### Phase-by-Phase Recommendation

#### Phase 1: Development (Month 1-2) — **₹0/mo**
```
✅ FOLLOW THIS:
• Everything on free tier — no exceptions
• Gemini 2.0 Flash free tier for AI
• Supabase free for database
• Render free for backend (cold starts are OK for testing)
• Focus on building, not spending
```

#### Phase 2: Friends & Family Beta (Month 3) — **₹580/mo**
```
✅ FOLLOW THIS:
• Upgrade ONLY Render to Starter (₹580/mo) — this is non-negotiable
• Everything else stays free
• Gemini free tier handles 10-50 beta users easily
• Total: ₹580/mo

❌ DON'T DO THIS:
• Don't upgrade Supabase yet (500MB is plenty for <50 users)
• Don't pay for Resend yet (100 emails/day is fine)
• Don't buy OpenAI API keys (Gemini free tier is enough)
```

#### Phase 3: Public Launch (Month 4-5) — **₹1,000-1,500/mo**
```
✅ FOLLOW THIS:
• Render Starter: ₹580/mo
• Upstash Redis paid: ₹200/mo (for AI caching + rate limiting)
• Gemini API paid tier (if hitting free limits): ~₹100-200/mo
• Total: ~₹1,000-1,500/mo

❌ DON'T DO THIS:
• Don't upgrade Supabase until 500MB is actually full
• Don't switch to OpenAI "because it sounds better"
• Don't pay for receipt scanning (Tesseract.js is free forever)
```

#### Phase 4: Growth (Month 6+) — **₹2,500-4,000/mo**
```
✅ FOLLOW THIS:
• Render Starter: ₹580/mo
• Supabase Pro: ₹2,100/mo (when DB gets close to 500MB)
• Upstash Redis: ₹200/mo
• Gemini paid: ₹100-200/mo
• Total: ~₹3,000-3,500/mo

❌ DON'T DO THIS:
• Don't upgrade Resend until you actually hit 100 emails/day
• Don't add Apple Developer ($99/yr) until you have iOS demand
```

### Why This Budget Works

| Decision | Why |
|----------|-----|
| **Gemini over OpenAI** | Free tier covers 1000+ users. OpenAI has no free tier. Gemini Flash quality is equivalent for structured data analysis |
| **Render Starter as first upgrade** | Cold starts kill user experience. This ₹580 is the most impactful money you'll spend |
| **Keep Supabase free as long as possible** | 500MB stores ~100K expense records. You won't fill this for months |
| **Redis for AI caching** | ₹200/mo Redis saves ₹2000+/mo in AI API calls by caching insights |
| **Skip Apple Developer initially** | ₹8,300/yr for iOS when 95% of Indian users are on Android? Wait for demand |
| **Never pay for receipt OCR** | Tesseract.js runs on your server for free. Google Vision API would cost ₹100+/mo for no real quality improvement on printed receipts |

---

## 📋 BUILD ORDER (RECOMMENDED)

| Phase | What to Build | Timeline |
|-------|-------------|----------|
| **Phase 1** | Auth + Expense + Groups + Balances + Dashboard | Week 1-2 |
| **Phase 2** | Activity Feed + Settlement + Profile | Week 3 |
| **Phase 3** | Analytics + Insights + Goals | Week 4 |
| **Phase 4** | Payment (Razorpay) + Notifications | Week 5 |
| **Phase 5** | Receipt Scanning + Disputes + Offline | Week 6 |
| **Polish** | Dark mode, animations, testing, deployment | Week 7 |

---

## ⭐ KEY HIGHLIGHTS (Don't Miss These)

If you read nothing else in this document, read these:

### 🔴 Critical Decisions

1. **Gemini 2.0 Flash is the AI backbone** — FREE for 1000+ users. Don't waste money on OpenAI. The free tier gives 15 requests/minute = 21,600/day. With 24-hour caching, that's 21,600 daily users for ₹0.

2. **Render free tier is a TRAP for production** — 30-50 second cold starts = users leave. ₹580/mo Starter plan is mandatory for launch. This is your single most important infrastructure spend.

3. **Splitwise charges ₹400/mo for features we give free** — Charts, receipt scanning, advanced insights. This is our #1 competitive advantage in India. Never paywall these.

4. **Cache everything, call AI once** — One Gemini call per user per day, cached in Redis. 10 views = 1 API call. This single pattern saves 90% of AI costs.

5. **India = Android first** — 95% of our target users are on Android. Skip Apple Developer ($99/yr) until you have real iOS demand. Save ₹8,300/year.

### 🟢 What Makes PayOrbit Special

| What | Why It's Special |
|------|------------------|
| **AI insights at ₹0** | Competitors charge ₹400/mo. We use Gemini free tier + rule-based fallback |
| **UPI payments built-in** | Splitwise STILL doesn't support UPI natively. We use Razorpay |
| **Personal + Group in one app** | Walnut = only personal. Splitwise = mainly groups. We do both |
| **Receipt scanning free** | Tesseract.js runs locally. Splitwise charges for this |
| **Offline-first** | Works in low connectivity — critical for Indian students |

### 🟡 Biggest Risks to Watch

| Risk | Mitigation |
|------|------------|
| Gemini free tier gets removed/reduced | Fallback to rule-based insights (already built). Gemini paid tier is still 3x cheaper than OpenAI |
| Supabase free tier database fills up | Monitor with dashboard. At 400MB, start cleaning old activity logs. Upgrade only when actually needed |
| Razorpay 2% fee eats into settlements | This is only for real money transfers. Most users will just "mark as paid" (₹0 cost) |
| Users compare to Splitwise UI quality | Invest in UI polish. Lottie animations + good design system make a huge difference |

---

## 🧭 WHAT TO FOLLOW AND WHY

### Development Philosophy

```
┌────────────────────────────────────────────────────────┐
│                   THE PAYORBIT WAY                     │
│                                                        │
│   1. Build features first, optimize later              │
│   2. Free tier first, upgrade only when it breaks      │
│   3. Cache aggressively, call APIs lazily              │
│   4. Indian market first, global later                 │
│   5. Android first, iOS when demanded                  │
│   6. Rule-based first, AI for personalization only     │
│   7. Never paywall core features                       │
│   8. UX > Features — a polished 5-feature app beats   │
│      a broken 15-feature app                           │
└────────────────────────────────────────────────────────┘
```

### Architecture Decisions — Why We Chose This Way

| Decision | Why | Alternative We Rejected |
|----------|-----|------------------------|
| **Monolith (single app.js) first** | Faster to build, easier to debug, deploy one thing. Microservices are overkill for <10K users | Microservices — adds complexity, DevOps overhead, harder to debug for 2-person team |
| **PostgreSQL over MongoDB** | Expense splitting needs relational data (users ↔ groups ↔ expenses). SQL joins are natural here | MongoDB — document DB is bad for relational queries. Calculating balances across groups is painful |
| **Prisma over raw SQL** | Type-safe queries, auto-migrations, visual studio. We're JS developers, not DBA experts | Raw SQL — error-prone, no migration system, easy to write insecure queries |
| **JWT over sessions** | Stateless — no server memory needed. Works perfectly for mobile + web. Easy to scale | Server sessions — needs Redis for session store, doesn't work well with mobile apps |
| **Zustand over Redux** | 10x less boilerplate. Our state is simple — user, expenses, groups. Redux is overengineered for this | Redux — 3 files (actions, reducers, store) for every feature. Zustand = 1 file |
| **React Native over Flutter** | Same language (JS/TS) as our web app and backend. One language for everything | Flutter — new language (Dart), separate ecosystem, team needs to learn from scratch |
| **Gemini over OpenAI** | Free tier covers our scale. Google infrastructure in India (Mumbai). Cost is 3x lower at scale | OpenAI — no free tier, no Indian data center, higher cost per token |
| **Rule-based + AI hybrid** | Rules handle 70% of insights instantly (free). AI handles 30% personalized advice. Best of both worlds | AI-only — expensive, slow, single point of failure. Rules-only — generic, not personalized |

### Code Practices to Follow

| Practice | Why |
|----------|-----|
| **Zod validation on EVERY endpoint** | One unvalidated input = SQL injection or crash. Zod catches bad data before it touches DB |
| **Prisma transactions for money operations** | Expense creation = create expense + create splits + update balances. If any step fails, ALL must rollback |
| **Rate limiting on auth endpoints** | Without it, someone can try 10,000 passwords/second. 5 attempts/15 min = safe |
| **JWT expiry at 7 days** | Too short (1 hour) = annoying re-logins. Too long (30 days) = security risk. 7 days = sweet spot |
| **Uppercase categories before saving** | "food" vs "Food" vs "FOOD" breaks analytics grouping. Always normalize |
| **Cache AI insights in Redis (24h TTL)** | User checks insights 10 times/day. Without cache = 10 API calls. With cache = 1 call + 9 instant responses |
| **Activity logging on every mutation** | Every expense/settlement/member change = activity record. This IS the audit trail. Never skip it |

---

> **Built with ❤️ by college students, for college students.**  
> Every tool chosen for maximum quality at minimum cost.  
> Free where it doesn't compromise UX. Smart upgrades where it matters.  
> No unnecessary paid APIs. No vendor lock-in. No broken user experience.
