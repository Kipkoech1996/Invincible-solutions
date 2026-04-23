# Vortex — Registration Site with Netlify Blobs

A production-ready registration website hosted on Netlify, using **Netlify Blobs** as the serverless database.

---

## 🗂 Project Structure

```
registration-site/
├── index.html                        ← Registration form (frontend)
├── admin.html                        ← Admin dashboard (view/delete registrations)
├── netlify.toml                      ← Netlify config (functions, redirects)
├── package.json
└── netlify/
    └── functions/
        ├── register.mjs              ← POST  /api/register
        ├── list-users.mjs            ← GET   /api/list-users
        └── delete-user.mjs          ← DELETE /api/delete-user?id=<uuid>
```

---

## 🚀 Deploy to Netlify (5 minutes)

### Option A — Netlify CLI (recommended)

```bash
# 1. Install dependencies
npm install

# 2. Install Netlify CLI globally (if not already)
npm install -g netlify-cli

# 3. Login to Netlify
netlify login

# 4. Create a new Netlify site
netlify sites:create --name your-site-name

# 5. Deploy!
netlify deploy --prod
```

### Option B — GitHub + Netlify UI

1. Push this folder to a GitHub repository
2. Go to https://app.netlify.com → **Add new site → Import an existing project**
3. Connect your GitHub repo
4. Build settings are auto-detected from `netlify.toml`
5. Click **Deploy site** — done!

---

## 🧪 Local Development

```bash
npm install
netlify dev
```

This starts a local server at `http://localhost:8888` with:
- The HTML pages served statically
- Netlify Functions running at `/.netlify/functions/*`
- Netlify Blobs using a local emulated store

---

## 🗄 How Netlify Blobs Works

Netlify Blobs is a key-value blob storage built into every Netlify site — no separate database to provision.

| Blob key              | Value                          | Purpose                     |
|-----------------------|--------------------------------|-----------------------------|
| `user:<uuid>`         | JSON user object               | User record                 |
| `email:<email>`       | UUID string                    | Email deduplication index   |

### Store name used: `"users"`

No environment variables needed for blob access in production — Netlify injects credentials automatically at runtime.

---

## 🔌 API Endpoints

| Method   | Path                          | Description              |
|----------|-------------------------------|--------------------------|
| `POST`   | `/.netlify/functions/register`    | Register new user        |
| `GET`    | `/.netlify/functions/list-users`  | List all registrations   |
| `DELETE` | `/.netlify/functions/delete-user?id=<uuid>` | Delete a user |

### POST `/register` — request body

```json
{
  "firstName":  "Ada",
  "lastName":   "Lovelace",
  "email":      "ada@example.com",
  "phone":      "+254 700 000 000",
  "role":       "developer",
  "password":   "securePassword123",
  "newsletter": true
}
```

---

## 🔒 Security Notes

- Passwords are hashed with **bcrypt** (cost factor 10) before storage
- Password hashes are **never** returned in API responses
- Duplicate emails are rejected with a `409` status
- Input validation runs on both client and server side

---

## 🛠 Tech Stack

| Layer     | Technology             |
|-----------|------------------------|
| Frontend  | Vanilla HTML/CSS/JS    |
| Functions | Netlify Functions (ESM)|
| Database  | Netlify Blobs          |
| Auth      | bcryptjs + uuid        |
| Hosting   | Netlify                |
