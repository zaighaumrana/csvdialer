# Cold Call Dialer

A fast browser-based cold call dialer connected to Google Sheets in real time.  
Hosted on GitHub Pages — one URL, works from anywhere, any device.

---

## How it works

```
Google Sheet  ←→  Apps Script (bridge)  ←→  Dialer (GitHub Pages URL)
```

- Loads your leads directly from your Google Sheet tab
- Notes and checkboxes save back to the Sheet automatically as you work
- Protected by Google account login — only people you've shared the Sheet with can access it
- Each person connects their own Sheet to the same dialer URL

---

## Security

- **Who can access the data?** Only people signed into a Google account AND who have access to the Sheet
- **Is the Script URL in the code?** No — it's stored only in your browser's localStorage, never committed to GitHub
- **Is the repo safe to make public?** Yes — there are no credentials or data in the code

---

## Setup — One time per person (~5 minutes)

### Step 1 — Add the Apps Script to your Google Sheet

1. Open your Google Sheet
2. Click **Extensions → Apps Script**
3. Delete any existing code in `Code.gs`
4. Copy the entire contents of `Code.gs` from this repo and paste it in
5. Click **Save** (Ctrl+S)

### Step 2 — Deploy as a Web App

1. Click **Deploy → New deployment**
2. Click the ⚙ gear icon next to "Select type" → choose **Web app**
3. Set:
   - **Description:** Dialer (anything works)
   - **Execute as:** Me
   - **Who has access:** Anyone with a Google account
4. Click **Deploy**
5. Click **Authorize access** → select your Google account → click **Allow**
6. Copy the **Web app URL** — it looks like:
   ```
   https://script.google.com/macros/s/AKfycb.../exec
   ```

> ⚠️ Important: Every time you edit `Code.gs`, you must create a **New deployment** (not update existing) to push the changes live.

### Step 3 — Open the dialer and connect

1. Open the dialer: `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME`
2. Paste your Apps Script URL in the setup screen
3. Click **Connect & Sign in with Google** — Google may ask you to sign in
4. Your sheet tabs appear in the dropdown at the top
5. Select a tab → click **Load Tab**
6. The dialer auto-jumps to the first lead without a note — you're ready to call

Your Script URL is saved in this browser. Next time you open the dialer it connects automatically.

---

## GitHub Pages deployment

1. Create a new GitHub repository (can be public — no sensitive data is in the code)
2. Upload `index.html` and `README.md` to the repo
3. Go to **Settings → Pages**
4. Source: **Deploy from a branch** → Branch: `main` → Folder: `/ (root)` → **Save**
5. Wait ~1 minute → your URL is: `https://YOUR-USERNAME.github.io/REPO-NAME`
6. Share this URL with your team

---

## For colleagues (what they do)

Each colleague gets their **own Google Sheet** document shared to their email. They:

1. Open the shared dialer URL
2. Follow Steps 1–3 above for **their own Sheet** (paste `Code.gs`, deploy, get their URL)
3. Paste their own Script URL into the dialer setup screen once
4. Start working — their data is completely separate from others

You (admin) can monitor anyone's progress by opening their Google Sheet directly.

---

## Optional: restrict to specific emails only

In `Code.gs`, there is a commented-out whitelist section. Uncomment it and add your team's emails to ensure that even if someone outside your team has a Google account, they cannot access the data:

```javascript
const ALLOWED = [
  'zaighum@gmail.com',
  'anas@gmail.com',
  'colleague2@gmail.com',
];
```

After editing, create a **New deployment** to apply the change.

---

## Usage

| Action | Keyboard |
|---|---|
| Next lead | `↓` or `J` |
| Previous lead | `↑` or `K` |
| Copy number | `C` |
| Tag: PK CE | `1` |
| Tag: PK NI | `2` |
| Tag: VM | `3` |
| Tag: NA | `4` |
| Tag: CD | `5` |
| Tag: DNC | `6` |

**Note is required** after every call — the indicator turns green once filled.  
**Auto-saves** — every change syncs to Google Sheets within ~1 second.  
**Auto-resume** — loading a tab jumps straight to the first row without a note.

---

## Tag reference

| Tag | Meaning |
|---|---|
| PK CE | Call picked, person ended abruptly |
| PK NI | Call picked, explicitly not interested |
| VM | Went to voicemail |
| NA | No answer |
| CD | Call disconnected during dial |
| DNC | Do not call (requested by person) |
| custom | Anything you type manually |

---

## Column mapping

The Apps Script auto-detects columns by header name (case-insensitive). Supported header names:

| Field | Accepted header names |
|---|---|
| MC Number | MC Number, MC#, MC |
| Entity | Entity, EntityStatus, Entity Status |
| Status | Status |
| Power | Power |
| Driver | Driver |
| Company | Company Name, Company |
| Address | Physical Address, Address |
| Phone | Phone Number, Phone |
| Note | Note, Notes |
| Email | Email Address, Email Addr, Email |
| State | State |
| Email sent | Email sent, Email_sent |
| Follow up | Follow up mail, Follow up, Followup |

Column **order does not matter** — detection is by name only.
