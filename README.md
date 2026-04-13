# LiveSlide — Real-Time QR Survey in PowerPoint

Show live audience survey results directly inside a PowerPoint slide.
People scan a QR code → answer on their phone → bars animate in real-time on your screen.

---

## What you need
- A GitHub account (free)
- A Firebase account (free) — for storing responses in real-time
- PowerPoint for Windows or Mac (desktop app, not web)

**Total setup time: ~10 minutes**

---

## Step 1 — Create your Firebase database

1. Go to **https://console.firebase.google.com** and sign in with your Google account
2. Click **"Add project"** → name it anything (e.g. `liveslide`) → click through the setup
3. In the left sidebar, click **"Realtime Database"** → **"Create database"**
   - Choose a location (any region is fine)
   - Select **"Start in test mode"** → click Enable
4. In the left sidebar, click **"Project Settings"** (gear icon)
5. Scroll to **"Your apps"** → click the **`</>`** (web) button
   - App nickname: `liveslide` → click **Register app**
   - Copy the `firebaseConfig` object — you'll need it in the next step

---

## Step 2 — Set up your files

Open **both** `presenter.html` and `survey.html` in a text editor.

At the top of each file, paste your Firebase config:

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "your-actual-api-key",
  authDomain:        "your-project.firebaseapp.com",
  databaseURL:       "https://your-project-default-rtdb.firebaseio.com",
  projectId:         "your-project",
  storageBucket:     "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123:web:abc123"
};
```

In **`presenter.html`** only, also update:
```javascript
const SURVEY_URL = "https://YOURUSERNAME.github.io/liveslide/survey.html";
```

To customize your questions, edit the `QUESTIONS` array in **both** files (keep them identical):
```javascript
const QUESTIONS = [
  { id: "q1", text: "Your question here?", options: ["A", "B", "C", "D"] },
  ...
];
```

---

## Step 3 — Publish to GitHub Pages

1. Go to **github.com** → click **"New repository"**
   - Name it exactly: `liveslide`
   - Set to **Public**
   - Click **Create repository**

2. Upload your 4 files (`presenter.html`, `survey.html`, `manifest.xml`, `README.md`):
   - Click **"uploading an existing file"** → drag all 4 files → click **Commit changes**

3. Go to **Settings → Pages** (left sidebar)
   - Under "Source", select **"Deploy from a branch"**
   - Branch: `main`, folder: `/ (root)` → click **Save**

4. Wait ~60 seconds, then your site is live at:
   `https://YOURUSERNAME.github.io/liveslide/`

   Test it: open `https://YOURUSERNAME.github.io/liveslide/survey.html` in your browser.

---

## Step 4 — Add the live chart to PowerPoint

1. Open **`manifest.xml`** in a text editor
2. Replace both instances of `YOURUSERNAME` with your actual GitHub username → save

3. Open **PowerPoint** (desktop app)
4. Navigate to the slide where you want the live chart
5. Click **Insert** → **Get Add-ins** (or "My Add-ins")
6. Click **"Upload My Add-in"** → browse to `manifest.xml` → click **Upload**
7. The add-in appears as an object in your slide — **resize and position it** like any shape

The chart is now live inside your slide. ✅

---

## Step 5 — Present

1. Share `https://YOURUSERNAME.github.io/liveslide/survey.html` with your audience
   (or just let them scan the QR code shown in the slide)
2. Start your PowerPoint presentation
3. As people respond, the bars animate in real-time in the embedded chart
4. Use **◄ ►** buttons (or ← → arrow keys) to switch between questions
5. The response count updates automatically — no refresh needed

---

## Clearing responses between events

Go to your Firebase console → Realtime Database → click the `responses` node → press Delete.

---

## File overview

| File | Purpose |
|------|---------|
| `presenter.html` | Live chart — embedded in PowerPoint |
| `survey.html` | Survey form — respondents open this |
| `manifest.xml` | Tells PowerPoint where to find the chart |
| `README.md` | This guide |

---

## Troubleshooting

**"Firebase not configured" message** → Check that you pasted the config in both HTML files.

**QR code shows wrong URL** → Update `SURVEY_URL` in `presenter.html`.

**Add-in doesn't load in PowerPoint** → Make sure `manifest.xml` has your GitHub username, and that your GitHub Pages site is live (wait 60–90 seconds after pushing).

**Responses not updating** → Check Firebase → Realtime Database rules are set to test mode (allow read/write). Also verify `databaseURL` in the config includes `-default-rtdb`.
