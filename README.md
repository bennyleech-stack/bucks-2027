# Bucks 2027 vote

A one-page vote for the bucks weekend over the Canberra Day long weekend (Sat 6 – Mon 8 March 2027).
Each mate ranks Gold Coast, Melbourne and Adelaide, builds their ideal weekend, and sends a plain-text
reply to Ben on WhatsApp. Replies get tallied with `tally.html` or by pasting them into Claude (see `parse.md`).

| File | What it is |
|---|---|
| `index.html` | The page you send to the group. Inline CSS and JS, no build step. |
| `tally.html` | Private counting tool. Not linked from the main page. Paste all replies in, press Tally. |
| `parse.md` | The exact reply format, for parsing replies by hand or with Claude. |

## Editing the content

Everything lives in the `<script>` block near the bottom of `index.html`.

- **WhatsApp number**: `BEN_WHATSAPP` at the top of the script. Digits only, international format
  (`61410039467`). Set it to `''` and WhatsApp will ask the sender which chat to use.
- **Destinations**: the `DESTS` array. Each destination has:
  - `id`: short key used for saved drafts. Don't change it once people have started voting.
  - `name`: shown on the page and sent in the reply. If you rename one, update `DESTS` in `tally.html` too.
  - `cost`, `tagline`, and `info` (the race-card rows: `[label, text]` pairs).
  - `slots`: six arrays of options, one per time slot, in the order of `SLOTS`. **The first option in each
    array is the suggested plan** and is what every dropdown starts on. "Other (type it)" is added automatically.
- **Time slots**: `SLOTS`. `key` is what's sent in the reply; `label` is what's shown. Keep the keys in step
  with `SLOTS` in `tally.html`.
- **Step 1 choices and must-haves**: `STATUS`, `BUDGET`, `ARRIVE`, `MUST`. Budget options have a `label`
  (shown, with en dashes) and a `msg` (sent, plain hyphens). Keep `BUDGET`/`ARRIVE` in `tally.html` matching the `msg` values.
- **Footer and header text**: plain HTML in the page body.

Changing an option's text mid-vote is fine, but replies already sent keep the old wording, so the tally will
count them separately. Drafts saved on people's phones repair themselves if an option index no longer exists.

If you change the reply format itself, bump `BUCKS VOTE v1` to `v2` in both files and in `parse.md`.

## Testing locally

Open `index.html` in a browser, or run `python3 -m http.server` in this folder and visit
`http://localhost:8000`. The copy button needs `localhost` or HTTPS; on `file://` it falls back to
selecting the text.

## Redeploying (GitHub Pages)

The site is served by GitHub Pages from the `main` branch, root folder.

1. Edit the files and commit.
2. `git push origin main`
3. Pages rebuilds in about a minute. Check **Settings → Pages** or the **Actions** tab if it doesn't update.
4. Hard-refresh the page (or add `?v=2` to the URL) to dodge a cached copy.

First-time setup: **Settings → Pages → Build and deployment → Source: Deploy from a branch → `main` / `(root)` → Save.**

Live page: `https://bennyleech-stack.github.io/bucks-2027/`
Tally: `https://bennyleech-stack.github.io/bucks-2027/tally.html`

## Privacy

No tracking, no analytics, no backend. Answers are saved only in the sender's own browser (localStorage)
until they send them. Both pages are marked `noindex`, but the URL is public, so treat it as unlisted rather than private.
