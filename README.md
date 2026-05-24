# Landing-Page
# 🧠 QuizMaster Pro

> **An AI-Powered Quiz & Learning Platform** — Gamified, Proctored, and Scalable

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Stack](https://img.shields.io/badge/stack-React%20%2B%20Node.js%20%2B%20Supabase-brightgreen)
![AI](https://img.shields.io/badge/AI-LLaMA%203.3%2070B-purple)
![License](https://img.shields.io/badge/license-MIT-orange)

---

## 📌 Overview

QuizMaster Pro is a comprehensive full-stack web application delivering an engaging, gamified learning experience through interactive quizzes. It combines cutting-edge AI technology with robust gamification mechanics for students, educators, and competitive learners.

---

## 🚀 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React.js 18, React Router v6, Context API, Custom CSS |
| Backend | Node.js, Express.js, JWT Auth, Nodemailer |
| Database | Supabase PostgreSQL (35+ tables), Row-Level Security |
| AI Engine | LLaMA 3.3 70B via Groq SDK |
| Proctoring | MediaPipe (Browser-based face detection) |
| Payments | Razorpay |
| Storage | Supabase Storage (video + screenshots) |

---

## ✨ Features

### 🤖 AI Quiz Generation
- LLaMA 3.3 70B generates unique MCQs on any topic
- Profile-based personalisation and difficulty control
- AI tutoring and hint system

### 🎮 Gamification & Coin Economy
- Coins earned via quiz completion, daily login, spin wheel, referrals, surveys
- XP levels, badges, and streak milestones (up to 3,000 coins at 60-day streak)
- Freeze credits to protect streaks

### 🔒 AI Proctoring System
- Browser-based real-time face detection via MediaPipe CDN
- Tab switch monitoring, copy/paste blocking, fullscreen enforcement
- Auto-submit on max violations
- Full session video recording to Supabase Storage

### 🛡️ Security
- JWT HS256 authentication (7-day expiry)
- bcryptjs password hashing (12 salt rounds)
- OTP email verification with 10-min expiry & 5-attempt lockout
- Device fingerprinting + new-login alerts
- Rate limiting: 300 req/min per IP, 10 AI req/min per user

### 💰 Monetization
| Plan | Price | Features |
|---|---|---|
| Free | ₹0 | 5 AI quizzes/day, 2 freeze credits/month |
| Pro | ₹300/month | Unlimited AI, 5 freeze credits |
| Elite | ₹500/month | 10 freeze credits, elite events, proctoring |
| 1-Year | ₹3,000/year | All Elite features, best value |

### 📊 Admin Dashboard
- User management, payment approval
- Analytics, bulk email
- Live platform settings (app_settings table)

---

## 🗂️ Project Structure

```
quizmaster-pro/
├── client/                  # React.js Frontend
│   ├── src/
│   │   ├── components/      # Reusable UI components
│   │   ├── pages/           # Route pages
│   │   ├── context/         # Context API state management
│   │   └── styles/          # Custom CSS system
│   └── public/
├── server/                  # Node.js + Express Backend
│   ├── routes/              # 80+ API route files
│   ├── middleware/          # JWT auth, rate limiting, CORS
│   ├── controllers/         # Business logic
│   └── config/              # Supabase, Groq, Razorpay config
└── README.md
```

---

## ⚙️ Installation & Setup

### Prerequisites
- Node.js v18+
- Supabase account
- Groq API key
- Razorpay account

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/quizmaster-pro.git
cd quizmaster-pro
```

### 2. Install Dependencies
```bash
# Backend
cd server
npm install

# Frontend
cd ../client
npm install
```

### 3. Environment Variables

Create a `.env` file in the `/server` directory:

```env
# Server
PORT=5000
NODE_ENV=development

# Supabase
SUPABASE_URL=your_supabase_url
SUPABASE_SERVICE_KEY=your_supabase_service_key

# JWT
JWT_SECRET=your_jwt_secret

# Groq AI
GROQ_API_KEY=your_groq_api_key

# Nodemailer (Gmail)
EMAIL_USER=your_gmail@gmail.com
EMAIL_PASS=your_gmail_app_password

# Razorpay
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
```

Create a `.env` file in the `/client` directory:

```env
REACT_APP_API_URL=http://localhost:5000
REACT_APP_RAZORPAY_KEY_ID=your_razorpay_key_id
```

### 4. Run the App
```bash
# Start Backend
cd server
npm run dev

# Start Frontend (new terminal)
cd client
npm start
```

App runs at `http://localhost:3000` 🚀

---

## 🔌 API Routes Overview

| Group | Routes | Description |
|---|---|---|
| Authentication | 6 | OTP, JWT login, forgot password |
| Quizzes & Attempts | 6 | CRUD, attempts, leaderboards |
| AI Features | 5 | Generate, tutor, quota, save |
| Gamification | 5 | Spin, hints, coins, badges |
| Proctoring | 6 | Sessions, violations, recordings |
| Rewards & Subscriptions | 4 | CPX postback, subscriptions |
| Admin Panel | 48+ | Users, payments, analytics |

**Total: 80+ RESTful endpoints**

---

## 💳 Razorpay Integration

```javascript
// Create Order (Backend)
const razorpay = new Razorpay({
  key_id: process.env.RAZORPAY_KEY_ID,
  key_secret: process.env.RAZORPAY_KEY_SECRET
});

const order = await razorpay.orders.create({
  amount: 30000, // ₹300 in paise
  currency: 'INR',
  receipt: `receipt_${Date.now()}`
});

// Verify Payment (Backend)
const crypto = require('crypto');
const hmac = crypto.createHmac('sha256', process.env.RAZORPAY_KEY_SECRET);
hmac.update(`${order_id}|${payment_id}`);
const signature = hmac.digest('hex');
// Compare with razorpay_signature
```

---

## 🗄️ Key Database Tables

```sql
users              -- coins, xp, streak, subscription_plan
quizzes            -- is_proctored, proctor_config, is_premium
questions          -- quiz_id, hint, correct_answer, options (JSONB)
quiz_attempts      -- score, time_taken, answers (JSONB)
coin_transactions  -- amount, type, balance_after
proctor_sessions   -- status, was_auto_submitted, recording_url
proctor_violations -- violation_type, severity, screenshot_url
streak_history     -- user_id, date, action
notifications      -- type, title, is_read
app_settings       -- key (PK), value (JSONB)
```

---

## 🚀 Deployment

### Frontend → Vercel
```bash
cd client
npm run build
# Deploy /build folder to Vercel
```

### Backend → Render
```bash
# Connect GitHub repo to Render
# Set environment variables in Render dashboard
# Deploy Node.js service
```

---

## 🗺️ Future Roadmap

- [ ] Real-time Multiplayer (WebSocket 1v1 quiz battles)
- [ ] Mobile App (React Native — iOS & Android)
- [ ] Advanced Analytics (per-question heatmaps, learning paths)
- [ ] Social Features (follow system, activity feed)
- [ ] LMS Integration (SCORM export, Google Classroom)

---

## 👨‍💻 Developer

**Harsh Sharma**
📧 harshsharma91000@gmail.com
📅 May 2026

---

## 📄 License

This project is licensed under the MIT License.

---

> *QuizMaster Pro — Production-Grade Full Stack Web Application*
