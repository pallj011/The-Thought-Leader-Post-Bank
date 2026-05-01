# LinkedIn Thought Leader Post Bank

A Claude-powered content pipeline for generating, importing, curating, and exporting satirical LinkedIn posts — with free images, a persistent post bank, and CSV/JSON export for scheduling automation.

---

## What it is

An interactive widget (runs inside Claude.ai) that generates absurdist-but-professional LinkedIn posts in the style of:

> *"43 days ago, I made what seemed like a small decision. I clicked 'just one more Helm upgrade.' What followed was a journey through darkness, confusion, and multiple Availability Zones..."*

Each post follows the full LinkedIn thought-leadership structure: confident setup, escalating diary entries, numbered takeaways, haunted closing line, and hashtags. Tone, category, and hook are all configurable.

---

## Features

### Generate tab
- **6 AI-generated title cards** per run, tailored to category and tone
- Click any card to instantly generate a full 250–400 word post
- **Typewriter streaming** effect as the post is written
- **Regenerate** any post with one click
- **Free Unsplash images** auto-loaded per category — select one to bundle with your post
- Save directly to the Post Bank

### Import tab
Three ways to feed in your own topics:

| Method | How |
|--------|-----|
| Manual entry | Type one topic and hit Add (or Enter) |
| Bulk paste | Paste multiple lines into the textarea |
| File upload | Drag & drop or browse — supports `.txt`, `.csv`, `.json` |

**Input format options:**

```
Plain topic:       Kubernetes upgrade disaster
With lesson hint:  Kubernetes disaster | blast radius management
Full hook:         I clicked one Helm upgrade and lost 43 days | why small changes aren't small
```

**File formats:**
- `.txt` — one topic per line
- `.csv` — columns: `hook`, `lesson` (optional: `category`, `tone`)
- `.json` — array of `{"hook":"...","lesson":"..."}` objects, or a full bank export (supports round-tripping)

Once topics are queued, **Build posts from queue** sends them to the Generate tab as title cards. Claude auto-generates the lesson line for any topics that only have a raw hook.

### Post Bank tab
- Persistent storage across sessions (browser `localStorage`)
- Each saved post shows: image thumbnail, hook, post preview, category/tone/date metadata
- Per-post actions: **Copy post**, **Copy image URL**, **Delete**
- **Export JSON** — full structured data including post text, image URL, hook, lesson, and metadata
- **Export CSV** — flat format for spreadsheet-based scheduling workflows

---

## Tone options

| Tone | Description |
|------|-------------|
| Satirical LinkedIn | Earnest on the surface, absurd at its core — ridiculous events treated with total professional gravity |
| Painfully sincere | Oversharing vulnerable personal details while connecting them to professional growth |
| Peak tech bro | Excessive jargon, move fast mindset, disruption framing, casual arrogance |
| Divorced man energy | Stoic, slightly wounded, finding business wisdom in personal loss, unnervingly calm |
| Max corporate jargon | Synergies, stakeholders, value propositions, leverage, bandwidth, circle back — every sentence |

---

## Category options

| Category | Content focus |
|----------|--------------|
| General hustle & growth | Absurd life/business events twisted into professional insights |
| Platform engineering | Kubernetes, CI/CD, Terraform, DevOps incidents, infrastructure disasters |
| Cybersecurity / DevSecOps | IAM, compliance theater, vulnerability scanning, InfoSec chaos |
| AI/ML & LLMs | Model failures, prompt engineering disasters, MLOps chaos |
| Leadership & management | Team dynamics, leadership failures, corporate culture absurdity |
| Startup / founder brain | VC culture, pivots, burn rate, hustle culture delusions |

---

## Post structure

Every generated post follows this format:

1. **Hook** — the opening premise (1–2 punchy lines)
2. **Setup** — confident line that ends in disaster (`"I had whispered 'this should be fine'…"`)
3. **Escalating diary entries** — 3–5 timeline milestones (`Day 7: / Day 18: / etc.`) with absurdist detail treated as completely real
4. **Pivot** — `"Here's what it taught me about [lesson]:"`
5. **Numbered takeaways** — 4–5 items with bold titles and survivor wisdom
6. **Closing line** — one haunted, deadpan sentence
7. **Hashtags** — 6–8 tags (mix of real LinkedIn hashtags + 1–2 slightly unhinged ones)

---

## Images

Images are sourced from [Unsplash](https://unsplash.com) via their public CDN. They are free for commercial use under the [Unsplash License](https://unsplash.com/license) — no attribution required.

When you select an image and save a post, the image URL is stored alongside the post text in the bank and included in exports.

> **Note for automation pipelines:** Unsplash CDN URLs are stable for linking but may redirect. For fully automated scheduling pipelines, consider downloading images locally or integrating the [Unsplash API](https://unsplash.com/developers) (free tier, 50 req/hour) to get permanent download URLs.

---

## Export formats

### JSON export

Full structured records — one object per saved post:

```json
[
  {
    "id": 1718123456789,
    "hook": "I accidentally ran terraform destroy on production at 3am.",
    "lesson": "Here's what it taught me about blast radius management.",
    "post": "43 days ago, I made what seemed like a small decision...",
    "imageUrl": "https://images.unsplash.com/photo-...",
    "imageCredit": "Unsplash",
    "category": "platform",
    "tone": "satirical",
    "savedAt": "2025-06-15T14:32:11.000Z"
  }
]
```

This format can be re-imported — drop the exported `.json` back into the Import tab to restore or migrate your bank.

### CSV export

Flat format suitable for Buffer, Hootsuite, or any spreadsheet-based scheduler:

```
id,hook,lesson,post,imageUrl,imageCredit,category,tone,savedAt
```

---

## CSV template

Download the template from the Import tab for a pre-formatted starter file:

```csv
hook,lesson,category,tone
"I accidentally ran terraform destroy on production at 3am","Here's what it taught me about blast radius management","platform","satirical"
"My AI model started charging consulting rates","Here's what it taught me about scope creep in autonomous systems","ai","satirical"
```

---

## UI notes

- **Light/dark mode** — the widget inherits your system preference automatically via CSS variables. No toggle needed.
- **Persistent storage** — the Post Bank survives page refreshes and new sessions via `localStorage`. Clearing browser data will clear the bank; use Export JSON to back up before doing so.
- The **author profile** (name, title) displayed in the post preview is currently hardcoded. To change it, the widget code would need to be updated.

---

## Automation workflow (bulk build)

1. Prepare a CSV with your topic ideas (`hook`, `lesson` columns)
2. Import via the **Import tab** → file upload
3. Hit **Build posts from queue** — Claude enriches any hooks missing a lesson line
4. In the **Generate tab**, click through each title card to generate full posts
5. Select an image for each
6. Hit **Save to bank** for each post
7. Export as JSON or CSV for your scheduling tool

---

## Tech stack

| Component | Technology |
|-----------|-----------|
| AI generation | Anthropic Claude API (`claude-sonnet-4-20250514`) |
| Images | Unsplash public CDN (no API key required) |
| Storage | Browser `localStorage` |
| UI | Vanilla HTML/CSS/JS — no framework dependencies |
| Theming | CSS custom properties (auto light/dark via claude.ai design system) |

---

## Limitations

- Runs inside Claude.ai — not a standalone deployable app
- `localStorage` is scoped to the browser/device; use JSON export to move data between devices
- Unsplash CDN URLs (`source.unsplash.com`) serve random images per keyword — the same keyword may return different images on reload
- Post generation requires an active Claude.ai session (API calls are proxied through the widget)