# TAL Project Tracker

Internal project tracker for Truth Audit Labs. Supabase-backed, real-time synced.

## Architecture

```
GitHub Pages (this repo)     →  UI layer (HTML/CSS/JS)
Supabase (piiddlpimruzgtbarcdi)  →  Data layer (tasks, questions, notes)
```

Team opens one URL. Everyone sees the same data. UI updates are pushed via git.

## First-time setup (already done)

1. Supabase project created with `tracker_data` table
2. Credentials hardcoded in `index.html` (lines 94-95)
3. RLS policy allows read/write via anon key

## Deploying to GitHub Pages

### Option A: GitHub UI (no git needed)

1. Go to [github.com/new](https://github.com/new)
2. Create repo: `tal-tracker` (private)
3. Upload `index.html` via "Add file → Upload files"
4. Go to **Settings → Pages**
5. Source: **Deploy from a branch**
6. Branch: `main`, folder: `/ (root)`
7. Save. URL will be `https://<your-org>.github.io/tal-tracker/`

### Option B: Git CLI

```bash
cd tal-tracker-repo
git init
git add .
git commit -m "TAL tracker v1"
git branch -M main
git remote add origin git@github.com:<your-org>/tal-tracker.git
git push -u origin main
```

Then enable Pages in Settings → Pages → main branch.

## Pushing UI updates

When the HTML file is updated (new features, bug fixes):

```bash
# Replace index.html with the new version
cp /path/to/new/TAL_Tracker_Supabase.html index.html
git add index.html
git commit -m "description of change"
git push
```

GitHub Pages redeploys automatically in ~60 seconds. Team refreshes the page to get the new UI. Data in Supabase is untouched.

## Team usage

1. Open the GitHub Pages URL in any browser
2. Type your name in "Editing as" field (top right)
3. Edit tasks, statuses, dates, notes — auto-saves to Supabase
4. Click ↻ Refresh to pull latest changes from others
5. Click ⚑ Rebaseline after a planning cycle to set current ETAs as new targets

## Key buttons

| Button | What it does |
|--------|-------------|
| ↻ Refresh | Pulls latest data from Supabase |
| ⚑ Rebaseline | Sets Target = ETA for all non-Done tasks, Start = today for Not Started tasks |
| Reset | ⚠ Resets ALL data to defaults for EVERYONE. Use with extreme caution. |

## Data backup

The Supabase table `tracker_data` row 1 contains all data as a JSONB blob. To backup:

```sql
SELECT content FROM tracker_data WHERE id = 1;
```

Copy the JSON output and save it. To restore, paste it back:

```sql
UPDATE tracker_data SET content = '<paste json here>' WHERE id = 1;
```

## Files

```
index.html    — The entire app (single file, zero dependencies)
README.md     — This file
```

## Confidential

TRUTH AUDIT LABS PRIVATE LIMITED — Internal use only.
