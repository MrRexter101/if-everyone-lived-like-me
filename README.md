# If Everyone Lived Like Me

A 2-minute quiz about convenience-store plastic habits (bags, straws, cutlery, takeaway boxes). It shows your yearly plastic use, what that would add up to if everyone lived like you, a "what if" slider, a 30-day challenge with points and badges, and a sample policy dashboard. English and Thai.

It is one static file (`index.html`) with no build step and no outside requests, so it runs on GitHub Pages as-is.

> **Status: prototype.** Several numbers are placeholders and the dashboard and community counters use mock data. See [Before going public](#before-going-public).

## Host it on GitHub Pages

1. Create a GitHub repo and push this folder to the `main` branch.
2. In the repo go to **Settings → Pages**.
3. Under **Build and deployment**, set Source to **Deploy from a branch**, Branch to **main**, folder **/ (root)**, then Save.
4. After a minute the site is live at `https://<your-username>.github.io/<repo-name>/`.

With the GitHub CLI, steps 1 to 3 are:

```bash
gh repo create if-everyone-lived-like-me --public --source . --push
gh api -X POST repos/{owner}/if-everyone-lived-like-me/pages -f "source[branch]=main" -f "source[path]=/"
```

`.nojekyll` tells GitHub to serve the files directly instead of running them through Jekyll (faster deploys, no surprises).

## Run it locally

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

## Editing numbers

Every number lives in the `CONFIG` object at the top of the `<script>` in `index.html`. Change values there; nothing else needs to change.

| Key | What it is | Status |
|---|---|---|
| `bagG` | Average plastic bag weight (g) | Placeholder |
| `cutleryG` | Grams per straw, cup or cutlery item | Placeholder |
| `containerG` | Grams per takeaway container | Placeholder |
| `population` | People used for the "if everyone" scale-up | Placeholder |
| `bagShare`, `ownBagCut`, `cutleryPerVisit`, `takeawayPerWeek` | Values behind Never / Sometimes / Usually / Always (range midpoints) | Placeholder |
| `persona` | Yearly bag thresholds for Eco Champion / Occasional Carrier / Bag Hoarder | Placeholder |
| `communityPeople`, `communityBags` | Starting numbers on the landing page counter | Mock |
| `sample` | All dashboard and policy-brief data | Mock |

## How the result is calculated

- Yearly bags = visits per week × 52 × share of visits taking a bag × (1 − own-bag reduction)
- Weight (g) = bags × `bagG` + cutlery items × `cutleryG` + takeaway containers × `containerG`
- Scale-up (tonnes) = personal grams × `population` ÷ 1,000,000

Equivalents such as "X km of bags" are left out on purpose until there is a sourced conversion.

## Data and privacy

Nothing leaves the browser. Everything is kept in the visitor's own `localStorage`:

- `ielm`: points, streak, badges, chosen goal.
- `ielm_results`: anonymous quiz answers, saved only if the visitor ticks the consent box.

To collect real responses, replace `saveResult()` with a `POST` to a database or form backend. The privacy notice text in the `S.en.privT` and `S.th.privT` strings is a plain-language draft for Thailand's PDPA (Personal Data Protection Act) and should be replaced with reviewed legal text before real data is collected.

## Before going public

- The landing page says "12,480 people have taken the quiz · 96,300 bags avoided". Those are mock numbers (`communityPeople`, `communityBags`). Set them to 0 or hide the line until there is real data.
- Replace the placeholder values in `CONFIG` with sourced figures and cite the sources.
- The leaderboard ("Chula", "Thammasat") and the dashboard are sample data.
- Some text is English only, even in Thai mode: the "How we calculated this" panel, the policy asks list, the policy brief and the share card.
- The share card image does not include the site URL. Add it once the final address is known.
