# Branch Profitability Challenge

A phone-based individual game for RBG training. Each participant runs one bank branch, picks 2 of 3 client offers per round, and watches their branch stats update. The host runs the game from a laptop/projector and sees a live leaderboard.

| Page | Who uses it | Link |
|---|---|---|
| `host.html` | Facilitator / admin | `https://<your-site>/host.html` |
| `index.html` | Participants (phones) | Shared automatically via QR code + game code |

---

## Step 1 — Put the game online (GitHub Pages, ~5 min)

1. Create a free account at **github.com** → **New repository** → name it e.g. `branch-game` → **Public** → Create.
2. Click **uploading an existing file** → drag in **all files and the `assets` folder** from this zip → **Commit changes**.
3. Open **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main`, folder `/ (root)` → **Save**.
4. After 1–2 minutes your site is live at `https://<your-username>.github.io/branch-game/`.

The site now runs in **Test mode** (one browser only). Open `host.html`, create a game, and open `index.html` in extra tabs to try it as players.

## Step 2 — Connect phones (Firebase, free, ~10 min)

Phones need a shared live database. Firebase's free Spark plan covers classroom-sized groups.

1. Go to **console.firebase.google.com** → **Add project** → any name → Analytics off → Create.
2. **Build → Realtime Database → Create database** → location **Singapore (asia-southeast1)** → start in **locked mode**.
3. In the database, open the **Rules** tab → paste the full content of `database.rules.json` → **Publish**.
4. **Build → Authentication → Get started → Sign-in method → Anonymous → Enable → Save.**
5. **Authentication → Settings → Authorized domains → Add domain** → `<your-username>.github.io`.
6. **Project settings (⚙) → General → Your apps → Web (`</>`)** → register an app → copy the `firebaseConfig` values.
7. In GitHub, open `assets/config.js` → ✏ edit → replace `window.FIREBASE_CONFIG = null;` with your config (an example is in the file; make sure `databaseURL` is included) → Commit.

Reload `host.html`. The top bar now shows **LIVE · Firebase**.

## Step 3 — Run a session

1. Open `host.html` on the projector laptop → **Create game**.
2. **Setup rounds & options** → edit anything → **Save to game**. (Changes are also remembered for your next game.)
3. **Run game** → participants scan the QR code, enter name + employee ID.
4. **Show rules on phones** → participants read the STE rules and tap *I understand* (the ready bar fills).
5. **Start Round 1** → phones show the 3 offers → the timer runs → the round closes automatically (or tap **Close round**).
6. After each round: see how players chose, the Top 5, optionally **Show answer for this round** → **Start next round**.
7. After the last round: **Finish game** → final leaderboard on every phone, plus answers and reasons.
8. **Results → Download CSV** for records (rank, name, employee ID, profit, picks per round).

Run the game from the same laptop browser that created it, because that browser holds the host controls.

## Admin options (Setup tab)

- **Settings:** starting CA/SA/TD, all five rates, reserve %, emergency borrowing %, seconds per round (0 = host closes manually), offers to select, answer reveal after each round or at the end.
- **Rounds:** add, copy, delete, reorder; up to 8 offers per round; product type (hidden from players, controls scoring); amount; risky-client flag with loss %; market event with CA+SA withdrawal %; reasoning text.
- **Calculate strongest path:** checks every possible combination (4.7 million for 14 rounds, about 1 second) and ticks the full-game best picks. Use it after any number change so the answer key always matches the scoring.
- **Export / Import JSON:** save round sets for different batches.

## Scoring (applied automatically)

- Deposit accepted: balance goes up; cost = rate × amount (one time).
- Loan / OD accepted: loans go up; earning = rate × amount (one time). Deposits are processed before loans within a round.
- Lending capacity = (100 − reserve)% × total deposits − loans.
- Loans above capacity are funded by emergency borrowing; cost = EB rate × each **new** rupee borrowed (charged once). New deposits pay it back.
- Risky client: loss = loss % × amount; balances stay the same.
- Market event: CA and SA fall by the withdrawal % before the round; TD stays.
- Unanswered round: branch stays the same.

Default 14-round set (verified): strongest path **₹5,46,900**; best-each-round-only play **₹3,32,375**; always picking A+B **−₹81,075**. Planning across rounds beats round-by-round thinking, and Term Deposits pay off at the Round 8 market event.

## Good to know

- Participants need mobile data or open Wi-Fi; some office networks block `github.io` or Firebase.
- Names and employee IDs are stored in your Firebase project. Check with your InfoSec team before use with live employee data, or ask participants to enter only first names.
- Players with equal profit share the same rank.
