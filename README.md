# Portfol — Reverse Portfolio Planner

Single-file HTML app. No build step. Deploy via GitHub Pages.

---

## 1. Firebase setup

1. Go to [console.firebase.google.com](https://console.firebase.google.com) → **Add project**
2. In the project, go to **Authentication → Sign-in method** → enable **Google**
3. Go to **Firestore Database** → **Create database** (start in production mode)
4. Go to **Project settings → Your apps → Add app → Web**
5. Copy the config object — it looks like:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project-id.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project-id.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123...:web:abc..."
};
```

6. Open `index.html` and replace the placeholder block near line 330 with your real config.

### Firestore security rules

In Firebase console → Firestore → Rules, paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/plans/{planId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### Authorised domains

Firebase → Authentication → Settings → **Authorised domains** → add your GitHub Pages domain:
`yourusername.github.io`

---

## 2. GitHub Pages deployment

```bash
# In your repo root (e.g. yuutwtm.github.io/portfol or a new repo)
cp index.html .nojekyll ./your-repo/

cd your-repo
git add .
git commit -m "init: portfolio planner"
git push origin main
```

Then: GitHub repo → **Settings → Pages → Source: main branch / root** → Save.

Your app will be live at `https://yourusername.github.io/repo-name/`

---

## 3. Exchange rate

Uses `api.exchangerate-api.com` (free tier, no key needed). Refreshes every 5 minutes. Falls back to ₩1,350 if the API is unreachable.

---

## 4. Data sources

Benchmark data embedded in the app is drawn from:

- **Federal Reserve Survey of Consumer Finances (SCF) 2022** — triennial survey of U.S. household finances. Median retirement savings and net worth by age cohort. https://www.federalreserve.gov/econres/scfindex.htm
- **Vanguard How America Saves 2023** — 401(k) contribution rates and balances by age. https://institutional.vanguard.com
- **Fidelity Retirement Savings Assessment 2023** — savings benchmarks by age/income.

---

## 5. Customisation notes

- To change the default 25-year horizon, edit `initDates()` in the script section
- Allocation glide path is in `calcAllocation()` — adjust the equity/bond splits to taste
- Korean translations are in the `T.ko` object near the top of the script
