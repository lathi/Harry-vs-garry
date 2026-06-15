# ⚽ WC2026 Predictor

A **multi-player FIFA World Cup 2026 group stage score predictor** — compete with friends by predicting the result of all 72 group stage matches in real time, backed by Firebase.

## ✨ Features

- **Any number of players** — friends join by entering their name on first visit
- **Live leaderboard** — rankings update instantly whenever anyone saves a prediction or a result is entered
- **Per-match prediction chips** — see everyone's predicted scores side-by-side on each fixture card
- **Points system** — +10 correct result, +10 exact score, +5 closest consolation
- **Admin mode** — password-protected ability to enter actual match results
- **Automatic data migration** — existing Harry/Garry data is silently converted to the new schema

---

## 🕹️ How to Play

1. Open the app URL
2. Enter your name when prompted (stored locally in your browser)
3. Browse to any **Group tab** (A–L)
4. Enter your predicted score for each match and hit **Save**
5. Check the **🏆 Standings** tab to track the leaderboard

---

## 🏅 Points System

| Points | Achievement |
|--------|-------------|
| **+10** | Correct result (Win / Draw / Loss) |
| **+10** | Exact scoreline |
| **+5**  | Closest prediction — lowest combined goal error, only when nobody gets exact. Ties are shared. |

---

## 🔒 Admin Access

The 🔒 button in the top-right corner unlocks the ability to enter actual match results.

- **Default password:** `wc2026`
- To change it, edit `ADMIN_PASSWORD` near the top of the `<script>` section in `index.html`
- Admin mode stays active for the current browser session (cleared on tab close)

---

## 🚀 Hosting on GitHub Pages

1. **Create a new GitHub repository** (or use this one)
2. Make sure `index.html` is at the **root** of the `main` (or `master`) branch
3. Go to your repo → **Settings → Pages**
4. Under *Source*, select `Deploy from a branch`, choose `main`, folder `/ (root)`
5. Click **Save** — your app will be live at:
   ```
   https://<your-username>.github.io/<repo-name>/
   ```

> [!TIP]
> Share that URL with your friends — they open it, enter their name, and start predicting immediately. No account needed.

---

## 🔥 Firebase Setup

The app uses **Firebase Realtime Database**. To use your own Firebase project:

1. Go to [Firebase Console](https://console.firebase.google.com) → Create or open a project
2. Enable **Realtime Database** (Build → Realtime Database → Create database)
3. Set database rules to allow read/write during the tournament — for example:
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
4. Copy your project's config and replace the `firebaseConfig` object in `index.html`

> [!WARNING]
> The open rules above allow anyone with the URL to read/write. This is fine for a private game with friends. For a public deployment, tighten the rules once the tournament ends.

---

## 📁 Database Schema

```
matches/
  {matchId}/              e.g. "A1", "B3", "L6"
    actual/
      h: number           Home team score
      a: number           Away team score
    predictions/
      {playerName}/       e.g. "Harry", "Garry", "Alice"
        h: string         Predicted home score
        a: string         Predicted away score
```

Points are computed **entirely client-side** from raw data — no points are ever stored in the database.
