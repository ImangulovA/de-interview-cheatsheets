# Build spec: sanitized + bilingual (EN/RU) DE interview cheatsheets

You convert ONE source cheatsheet into a sanitized, bilingual (English + Russian) version
that mirrors the gold-standard reference EXACTLY in mechanism and structure.

## Paths
- GOLD REFERENCE (already done, copy its mechanism exactly): `~/Desktop/de-interview-cheatsheets/cheatsheets/kafka.html`
- Apple-set source dir: `~/Desktop/interview_prep/cheatsheets/`
- Bloomberg-set source dir: `~/Desktop/interview_prep/bloomberg_econ_de/cheatsheets/`
- OUTPUT dir (write here): `~/Desktop/de-interview-cheatsheets/cheatsheets/`

ALWAYS read the gold reference first to copy the exact i18n CSS, the nav toggles markup,
and the combined theme+lang `<script>`. Do NOT invent your own mechanism.

## What to do (in order)
1. Read the gold reference `kafka.html` fully.
2. Read your assigned source file.
3. Produce the output file: sanitized + bilingual, preserving ALL styling, classes, ids,
   layout, and interactivity (flip cards, scroll reveal, nav highlight, collapsibles).

## 1) i18n mechanism (copy verbatim from kafka.html)
- `<html>` tag: `<html lang="en" data-theme="dark" data-lang="en">`
- Add this CSS (once, in the `<style>` block, near the top after the body/font rules):
  ```
  /* ---- i18n language toggle ---- */
  [data-lang="en"] .ru { display: none !important; }
  [data-lang="ru"] .en { display: none !important; }
  ```
- Nav: replace the single theme button with a `.toggles` wrapper holding BOTH buttons:
  ```
  <span class="toggles">
    <button class="toggle-btn" id="langBtn">🌐 RU</button>
    <button class="toggle-btn" id="themeBtn">🌙 Dark</button>
  </span>
  ```
  Add the `.toggles` and `.toggle-btn` CSS from kafka.html (keep `margin-left:auto` on `.toggles`
  so the buttons sit on the right). If the source used `.theme-btn`, replace its CSS with
  `.toggle-btn` rules from the reference. Keep the same nav icon/brand.
- Script: replace the source's theme-only script with the COMBINED theme+lang IIFE from
  kafka.html. Keep ALL other JS in the file (flip cards, IntersectionObserver reveal, nav spy).
  Use the SHARED localStorage keys EXACTLY: `de_cheat_theme` and `de_cheat_lang`.
  Default theme = `dark`, default lang = `en`. Button label shows the language you switch TO
  (EN active → button reads `🌐 RU`).

## 2) Bilingual content rules
Wrap EVERY user-visible text node in paired blocks:
- Inline text → `<span class="en">English</span><span class="ru">Русский</span>`
- Block elements → duplicate the whole element with a class:
  `<ul class="en">…</ul><ul class="ru">…</ul>`, same for `<p>`, `<table>`, `<pre>`,
  `<ol>`, callout `<div class="callout … en">` / `<div class="callout … ru">`, and flip-card faces.
- For headings `<h2><span class="dot">…</span> Title</h2>`: keep the decorative dot once, wrap
  only the title text in `<span class="en">…</span><span class="ru">…</span>`.
- Nav links: wrap link text in en/ru spans (see kafka).
- The number of `.en` blocks MUST equal the number of `.ru` blocks. Verify before finishing.

### Russian translation quality
- Natural, professional technical Russian. This is for a senior data engineer.
- KEEP IN ENGLISH (do not translate): code identifiers, config keys, SQL keywords, CLI flags,
  file/format/tech/product names (Spark, Kafka, Flink, Iceberg, Airflow, Debezium, S3, dbt,
  Snowflake, BigQuery, Hadoop, Presto), and anything inside `<code>`/`<pre>`.
- Translate prose, table headers, list items, callout bodies, quiz cards, captions.
- Code COMMENTS inside `<pre>` may be translated (kafka translated the `# …` comments).
- Keep ascii diagrams: duplicate the `.ascii` block, translate only the human-language labels
  inside it (leader/follower/broker etc.), keep the structure identical.

## 3) Sanitization rules (make it company-neutral & public)
REMOVE / NEUTRALISE the TARGET company everywhere:
- "Apple", "Apple Services Engineering", "ASE", "App Store/iCloud/Apple Music/Apple TV+/Apple Arcade",
  req "200651399-2114", role meta like "London (Hybrid)".
- "Bloomberg", "Bloomberg Terminal", "BQL", "Bloomberg values", Bloomberg-specific role meta.
- Any "tomorrow's recruiter call" / recruiter names / interview-loop logistics specific to a company.
Replace with neutral phrasing: "Data Engineering interview", "Senior/Lead Data Engineer".
- `<title>` → `"<Topic> — Data Engineering Interview Cheatsheet"`.
- Hero `<h1>` highlight → e.g. `"<Topic> — <span class="k">DE Interview Prep</span>"` (RU: "Подготовка к DE-интервью").
- Hero subtitle → generic ("Fast-revision cheatsheet for Data Engineering interviews. Tailored for Senior/Lead DE.").

KEEP AS-IS (only strip the target company from them):
- Personal "Amal anchor" callouts and the personal employer names in them
  (Syntea, IU Group, McMakler, Front Tier, Meta, "3.6B/day", etc.) — these are the user's real
  experience and are intentionally kept.
- "Honest gap statement (rehearse verbatim)" callouts — keep, translate.
- Footer "Built for Amal Imangulov …" — keep the name, but remove "Apple ASE"/"Bloomberg" target tags,
  replace with "Data Engineering interview prep".

## Behavioral files: rename on output
- `behavioral-apple-values.html` → OUTPUT as `behavioral-values.html`, title "Behavioral & Values".
  Generalise Apple-values language to broadly-applicable tech/FAANG values
  (quality, craftsmanship, ownership, collaboration, dealing with ambiguity). Keep STAR structure
  and personal stories.
- `behavioral-mentorship-bloomberg-values.html` → OUTPUT as `behavioral-mentorship-values.html`.
  Generalise Bloomberg values similarly. Keep mentorship content and personal stories.

## Before finishing — self-check
Run these on YOUR output file and confirm:
- No "Apple"/"ASE"/"Bloomberg"/"BQL" remain EXCEPT `-apple-system` in `font-family` (that one is fine).
- `.en` block count == `.ru` block count.
- Exactly one `id="langBtn"` and one `id="themeBtn"`.
- File still has its flip/reveal/nav JS intact.
Report: output filename, en/ru parity counts, and any sanitization edge cases you hit.
