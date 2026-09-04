# Fitlog

A single-file workout, step and food tracker. No build step, no dependencies, no accounts.
Data is saved in your browser's localStorage.

## Files

- `index.html` — the whole app
- `sw.js` — service worker, makes it open with no signal
- `README.md` — this file

## Put it on GitHub Pages

1. Create a new **public** repository. Free GitHub accounts can only publish Pages from public repos.
   Call it `fitlog`.
2. Upload `index.html` and `sw.js` to the root of the repo. Drag and drop on github.com works —
   click **Add file → Upload files**, then **Commit changes**.
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, set Source to **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
5. Wait about a minute. Your URL will be:

   ```
   https://<your-username>.github.io/fitlog/
   ```

## Put it on your phone's home screen

- **Android / Chrome:** open the URL, tap the three-dot menu, **Add to Home screen**.
- **iPhone / Safari:** open the URL, tap the share button, **Add to Home Screen**.

It then opens fullscreen with no browser bar and works without a connection.

## Backups matter

localStorage lives in one browser on one device. Clearing site data, switching phones or using a
different browser means starting empty. There is no sync.

On the **Progress** tab, under *Your data*:

- **Download backup** saves a `.json` file of everything.
- **Restore backup** loads one back in.

Do it monthly, or after any session you would be annoyed to lose.

## Sync your log into a GitHub repo

Optional. Turns the browser-only log into one JSON file in a repo, so a second device can pick it up.

**Use a second, private repo for the data.** The app repo has to be public for free Pages. Your
weight, food and training log should not be.

1. Create a **private** repo, e.g. `fitlog-data`. Tick "Add a README" so the branch exists.
2. Make a fine-grained token: GitHub → Settings → Developer settings → Personal access tokens →
   Fine-grained tokens → Generate new token.
   - Repository access: **Only select repositories** → `fitlog-data`
   - Permissions → Repository permissions → **Contents: Read and write**
   - Expiry: 90 days or a year. Note the date — sync stops dead when it expires.
3. Copy the token once. GitHub will not show it again.
4. In the app: **Progress → Sync to GitHub**. Enter your username, `fitlog-data`, `data.json`,
   `main`, and the token. Tap **Connect and pull**.

After that it pushes about 20 seconds after any change, and immediately when you finish a workout.
Each push is a commit, so the repo doubles as a full version history of your training.

### What to know before you turn it on

- **The token sits in this browser's localStorage.** Anyone with your unlocked phone can read it.
  A fine-grained token scoped to one private repo means the worst case is your log, not your account.
- **Never paste the token into `index.html`.** It would be published to the world. The app asks for
  it at runtime for exactly this reason.
- **Merging is additive.** Pull adds sessions the device doesn't have and fills empty days. If the
  same date has a value on both sides, the device you're holding wins. Editing the same day on two
  phones without pulling first will lose one of them.
- **Pull before you log** when switching devices. The app pulls on launch, but only if it has signal.
- Revoke the token on GitHub if you lose the phone. **Disconnect and forget token** clears it locally.

## Changing the plan

Everything is in `index.html`, near the top of the `<script>` block:

- `PROFILE` — current weight, goal weight, timeline text
- `PLAN` — the four training days (three gym, one home bodyweight circuit), exercises, sets and target reps
- `STEP_TARGET` — currently 8000
- `KCAL_TARGET`, `PROTEIN_TARGET` — currently 2200 and 170
- `WEEK`, `MEAL_MODES`, `SHOPPING` — the meal plan

Edit, commit, and the live site updates in about a minute. If your phone shows the old version,
close the app fully and reopen it — the service worker refreshes on the next launch.
