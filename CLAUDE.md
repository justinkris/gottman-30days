# Gottman 30 Days — Project Guide

## What this is
A personal, self-hosted 30-day relationship challenge journal for Justin and Kate, based on the Gottman Institute's 30 Days to a Better Relationship programme. It's a single HTML app (`index.html`) plus per-day completion pages (`day-N-complete.html`).

## File structure
```
gottman-30days/
├── index.html              # Main app — grid of all 30 days, each day's detail view
├── day-1-complete.html     # Day 1 responses, encouragement, illustrations
├── day-2-complete.html     # Day 2 responses, encouragement, illustrations
├── CLAUDE.md
├── response-prompts.txt    # AI image prompts for generating day-N-response.png files
├── responses/              # Saved .txt response files (DAY-N-Response_Him/Her.txt)
└── images/
    ├── day-N.png           # Day illustration (banner image shown at top of each day)
    ├── day-N-response.png  # Couple illustration for the response section of day N
    ├── day-N-kate.png      # Kate's personal illustration for day-N-complete.html
    ├── day-N-justin.png    # Justin's personal illustration for day-N-complete.html
    ├── Justin.png          # Justin's reference photo (used for AI image prompts)
    └── Kate.png            # Kate's reference photo (used for AI image prompts)
```

## index.html — key patterns

### Colours (CSS variables)
```
--cream: #F9F4EC    --parchment: #EDE4D3
--rose: #9B4E63     --rose-dark: #6E2E42    --rose-light: #C8899A
--sage: #5E7F6B     --brown: #2A1A12        --tan: #B89A7A    --gold: #C4933F
```

### Data arrays (top of `<script>`)
- `illustrations[]` — `{ img, prompt }` per day (0–30), references `images/day-N.png`
- `days[]` — `{ label, title, date, content }` per day (0–30)
- `bisayaTranslations[]` / `cebuanoTranslations[]` — optional translated content per day

### Key functions
- `showDay(n)` — renders a day's detail view; template includes `day-illustration-wrap`, response columns, couple photo section, and a "Day N Complete" link button
- `saveHim()` / `saveHer()` — saves textarea content as a .txt file download
- `loadCoupleImage(e)` — handles uploading the per-day couple illustration
- `copyCouplePrompt()` — copies an auto-generated AI image prompt to clipboard (uses day title)
- `openLightbox(src)` / `closeLightbox()` — full-screen image overlay on click

### Day illustration display
- Days 0–7: normal width (inside `max-width: 680px` detail body)
- Days 8–30: full-bleed (`class="day-illustration-wrap full-bleed"`) — extends past the content padding

### Couple photo section (per day)
- Placeholder auto-loads `images/day-N-response.png` on open; falls back to empty if missing
- File upload replaces the placeholder for the session

## day-N-complete.html — structure

Each day's completion page follows this pattern:
- **Header** — eyebrow label, day title, date, "Day N Complete ✓" badge
- **Response grid** — two-column CSS subgrid; Kate left, Justin right
  - Response card header (her/him colour tint)
  - Response body (italic quote)
  - Encouragement (personalised note for each person)
  - Illustration section (image or upload placeholder)
- **Completion button** — links back to `index.html`

### Adding a new day-complete page
1. Read the responses from `responses/DAY-N-Response_Her.txt` and `responses/DAY-N-Response_Him.txt`
2. Copy `day-1-complete.html` as the template
3. Update: title, header eyebrow/title/date/badge, response day labels, both response texts, both encouragement notes
4. Set `src="images/day-N-kate.png"` and `src="images/day-N-justin.png"` if images exist; otherwise leave as empty placeholder
5. Update the completion button href and label

## Languages
Kate writes responses in **Bisaya/Cebuano**. Justin writes in **English**.
Day-complete pages include a language toggle so each response card can be read in either English or Cebuano.

## AI image workflow
1. Open `response-prompts.txt` for the auto-generated prompt for the day
2. Attach `images/Justin.png` and `images/Kate.png` as reference photos in the AI tool
3. Save the output as `images/day-N-kate.png` or `images/day-N-justin.png`
4. The completion page will display it automatically if the file is present and `has-image` is set
