# Ping My Heart 💕

A cute, interactive date invitation app that lets you create secret, shareable invite links for someone special. Features a multi-step reveal experience — from envelope opening to a personalized message to the actual date invitation — complete with confetti, floating hearts, and a sneaky "No" button that runs away.

🌐 **Live Demo:** [https://pingmyheart.onrender.com](https://pingmyheart.onrender.com)

---

## ✨ Features

- **User Authentication** — Register and log in to manage your invites securely via session-based auth.
- **Secret Invite Links** — Generate unique, shareable links using `nanoid` that only the recipient needs to open.
- **3-Step Reveal Experience**
  1. An animated envelope the recipient clicks to open.
  2. A notebook-style personal message from the sender.
  3. The actual date invitation with Yes / No buttons.
- **Runaway "No" Button** — The "No" button dodges the cursor up to 20 times with increasingly dramatic plea messages before finally giving up.
- **Background Music** — Optionally upload an audio file that plays along with the invite experience.
- **Acceptance Celebration** — When someone accepts, the sender's dashboard fires off confetti and a toast notification.
- **Real-Time Status Polling** — The dashboard polls every 5 seconds to reflect the latest invite status (Pending / Accepted / Rejected).
- **Security Hardened** — Built with `helmet`, `express-rate-limit`, `express-validator`, `xss-clean`, CORS, and file-type validation on uploads.

---

## 🛠️ Tech Stack

| Layer      | Technology                                  |
|------------|---------------------------------------------|
| Runtime    | Node.js 18                                  |
| Framework  | Express.js                                  |
| IDs        | nanoid                                      |
| Uploads    | multer (disk storage, audio-only, 10 MB cap)|
| Security   | helmet, express-rate-limit, express-validator, xss-clean, cors |
| Dev        | nodemon                                     |
| Frontend   | Vanilla HTML / CSS / JS + canvas-confetti   |
| Storage    | In-memory (Maps) — no database required     |

---

## 📁 Project Structure

```
ping-my-heart/
├── public/
│   ├── index.html          # Redirects to login
│   ├── login.html          # Login page
│   ├── register.html       # Registration page
│   ├── dashboard.html      # Sender's invite management dashboard
│   ├── date.html           # Recipient's invite experience
│   └── uploads/            # Audio files uploaded by users (auto-created)
├── server.js               # Express server — API routes, auth, middleware
├── package.json
├── package-lock.json
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- [Node.js 18.x](https://nodejs.org)
- [npm](https://www.npmjs.com)

### Installation

```bash
git clone <your-repo-url>
cd ping-my-heart
npm install
```

### Run Locally

```bash
# Development (auto-reload on save)
npm run dev

# Production
npm start
```

The server starts on **http://localhost:3000** by default.

### Environment Variables

Create a `.env` file in the root if you want to override the default port:

```env
PORT=3000
```

---

## 📡 API Reference

All routes are served by `server.js`. Authentication is done via an `Authorization` header containing the session ID returned at login.

### Auth

| Method | Endpoint        | Auth? | Description                          |
|--------|-----------------|-------|--------------------------------------|
| POST   | `/api/register` | No    | Create a new account                 |
| POST   | `/api/login`    | No    | Log in and receive a session ID      |

### Invites

| Method | Endpoint              | Auth? | Description                                      |
|--------|-----------------------|-------|--------------------------------------------------|
| POST   | `/api/upload-audio`   | Yes   | Upload a background music file (audio only, ≤10 MB) |
| POST   | `/api/generate-link`  | Yes   | Create a new invite and get back a unique link ID |
| GET    | `/api/invites`        | Yes   | Fetch all invites belonging to the logged-in user |

### Recipient (Public)

| Method | Endpoint          | Auth? | Description                              |
|--------|-------------------|-------|------------------------------------------|
| GET    | `/api/get-link`   | No    | Fetch invite details by link ID          |
| POST   | `/api/respond`    | No    | Submit a Yes or No response to an invite |

---

## 🔒 Security Notes

- **Passwords are stored in plain text in memory.** This is a demo/fun project. For any production use, hash passwords with `bcrypt` or equivalent and persist data in a proper database.
- **In-memory storage** means all data is lost on server restart. Swap the `Map`-based stores for a database (e.g., MongoDB, PostgreSQL) for persistence.
- Audio uploads are restricted to `audio/*` MIME types and capped at 10 MB.
- Input is sanitised via `express-validator` (escaping & trimming) and `xss-clean`.

---

## 🎨 How It Works (User Flow)

```
Sender                                          Recipient
──────                                          ─────────
1. Register / Login
2. Fill in name, optional message & music  →    
3. Click "Generate Secret Link"            →    
4. Copy & share the link                   →    Opens link in browser
                                           ←    Clicks envelope to open
                                           ←    Reads notebook message
                                           ←    Sees the date invitation
                                           ←    Clicks Yes 🎉 or dodges No 😂
5. Dashboard polls & shows updated status  ←    
```

---

## 🤝 Contributing

Feel free to fork, tweak, and make it your own! If you add features or fix bugs, pull requests are welcome.

---

## ❤️ Made with love

Because sometimes the best way to ask someone out… is to make them chase a button first.
