# NFL Video Search Widget

A lightweight, customizable video search widget built for NFL club websites. No third-party search platform required — just a GitHub repo, a nightly workflow, and a single HTML file to embed.

Built by the Philadelphia Eagles digital team.

---

## How It Works

1. A GitHub Actions workflow scrapes your team's video sitemap every night and builds a `videos.json` index file (title, thumbnail, date, description) hosted on GitHub Pages.
2. The search widget loads that JSON in the browser and runs instant fuzzy search — no page reloads, no server calls.
3. Fans search by player name, moment, topic, or keyword and get ranked results in real time.

---

## What's Included

| File | Description |
|---|---|
| `search.html` | The search widget — drop this into your CMS |
| `build-video-index.yml` | GitHub Actions workflow that builds the nightly video index |

---

## Setup Instructions

### Step 1 — Create a GitHub Repo

- Create a new **public** GitHub repository for your club
- Go to **Settings → Pages** and enable GitHub Pages, deploying from the `main` branch
- This is where your `videos.json` index will be hosted

### Step 2 — Add the Workflow

- In your repo, create the folder path `.github/workflows/`
- Upload `build-video-index.yml` into that folder
- Open the file and update the sitemap URL on this line:

```js
const url = `https://www.philadelphiaeagles.com/sitemap/html/videos/${year}/${month}`;
```

Replace `philadelphiaeagles.com` with your team's domain. Example:

```js
const url = `https://www.giants.com/sitemap/html/videos/${year}/${month}`;
```

- The workflow runs automatically every night at 4:45 AM UTC
- You can also trigger it manually via **Actions → Build Video Index → Run workflow**
- After the first run, `videos.json` will appear in your repo and be accessible at:

```
https://YOUR-ORG.github.io/YOUR-REPO/videos.json
```

### Step 3 — Customize the Widget

Open `search.html` and follow the `STEP` comments throughout the file. Here's a summary of what to update:

| Step | What to Change |
|---|---|
| STEP 1 | Hero image preload URL |
| STEP 2 | GA4 Measurement ID |
| STEP 3 | Hero/player image URL |
| STEP 4 | Heading text and team logo URL |
| STEP 5 | Subtitle/hint text |
| STEP 6 | Popular suggestion pill labels |
| STEP 7 | Refine-by sub-pills (optional) |
| STEP 8 | Primary brand color (find/replace `#004c54`) |
| STEP 9 | `VIDEO_INDEX_URL` — point to your GitHub Pages JSON |
| STEP 10 | `SECONDARY_PILLS` — sub-pill options per primary pill |
| STEP 11 | "No results" message and links |
| STEP 12 | `CATEGORY_SLUGS` — any category-level video URL slugs to exclude |

### Step 4 — Set Up GA4 Custom Dimensions

The widget sends two custom events to GA4:

**`video_search`** — fires when a user searches
- Parameter: `search_term`

**`video_click`** — fires when a user clicks a video result
- Parameters: `search_term`, `video_title`, `video_url`

To see these in GA4 reports, register the parameters as custom dimensions:
1. Go to **GA4 → Admin → Custom Definitions → Custom Dimensions**
2. Add `search_term`, `video_title`, and `video_url` as event-scoped dimensions

### Step 5 — Embed the Widget

Copy the HTML, CSS, and JavaScript from `search.html` and paste it into your CMS wherever you want the widget to appear on your site.

> **Note:** The `<head>` section contains the GA4 script and image preload tag. If your CMS already loads GA4 globally, you can skip the GA4 snippet and only keep the preload link.

---

## Scroll Offset

The widget auto-scrolls to results when a user searches. The default offset is `122px` to account for a sticky navigation bar. Adjust this value in the JS to match your site's nav height:

```js
const offset = 122; // Update to match your sticky nav height
```

---

## Nightly Index Schedule

The workflow runs at `4:45 AM UTC` (~midnight ET). To change the schedule, update the cron expression in `build-video-index.yml`:

```yaml
- cron: '45 4 * * *'
```

---

## Questions

Reach out to the Philadelphia Eagles digital team.
