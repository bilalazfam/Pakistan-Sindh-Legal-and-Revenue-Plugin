# Using Legal Data Hunter to find and verify authorities

Legal Data Hunter (LDH) is the connected legal database. It is the tool that keeps this skill citation-safe:
use it to **find** on-point judgments and to **verify** any citation before it enters the table of
authorities. Prefer it over generic web search for anything touching statutes or case law.

The tools are deferred — load them with `tool_search` (e.g. query "Legal Data Hunter search") before the
first call in a session. The five you need:

- `discover_countries` — authoritative list of jurisdiction codes.
- `discover_sources` — the sources (courts/registries) available for a country, with document counts and
  `data_types` (which of `case_law` / `legislation` / `doctrine` each covers).
- `search` — hybrid semantic + keyword search over the corpus.
- `resolve_reference` — pin a specific citation to the exact document (this is verification, not search).
- `get_document` — pull the full text of a document by `source` + `source_id`.

## First-time setup in a session

1. Call `discover_countries` and find Pakistan's code (expected `PK`). Don't assume — confirm it.
2. Call `discover_sources(country_code="PK")` to see which Pakistani courts/registries and namespaces are
   actually indexed, their document counts, and date coverage. This tells you what you can realistically
   rely on and what will need a fallback.

### What the PK corpus actually holds (as observed — re-check, it grows)

- **Case law:** Islamabad High Court (`PK/IHC`, ~29.6k, the bulk), Peshawar High Court (`PK/PHC`, ~4k,
  often with reporter citations in the text), Supreme Court (`PK/SupremeCourt-HuggingFace`, ~1.4k, binding
  but small and with some data-quality noise). **No Sindh High Court source. No PLD/SCMR reporter series.**
- **Legislation:** `PK/PakistanLaws-HuggingFace` (~900, from official pakistancode.gov.pk — reliable, with
  citable URLs); a small Balochistan set.
- **Doctrine:** FBR tax and SECP only.

Implication for a Sindh matter: LDH gives you binding **Supreme Court** authority, **persuasive** IHC/PHC
authority, and the **exact statute text** — but not Sindh HC judgments or reporter-cited cases. Tell the
user that, and don't imply Sindh authority was searched when the corpus contains none.

## Finding authorities (step 3 of the workflow)

Use `search` with `country=["PK"]` and set `namespace` deliberately:

- `namespace="legislation"` to locate the statute text/section.
- `namespace="case_law"` to find judgments construing it. Run the search once per namespace when you need
  both — a single call only searches one.

Tips:
- **Query in natural language**, phrased around the legal issue and the statutory provision, e.g.
  "specific performance readiness and willingness Specific Relief Act section 12" or "oral gift hiba proof
  delivery of possession registration".
- `court_tier=1` filters to the **Supreme Court** (tier 1 = supreme, 2 = appellate, 3 = first instance) —
  use it to surface binding authority first, then widen if needed.
- `alpha` weights semantic vs keyword (default 0.7; lower it toward 0 when you're chasing an exact term or
  section number, raise it toward 1 for concept searches).
- `top_k` up to 100; start around 10–20 and refine.
- Use `date_start` / `date_end` when you need the *current* state of the law or the latest word on a
  conflict.

For each promising hit, read enough to state the **ratio** accurately — call `get_document(source,
source_id, include_full_text=True)` to pull the full judgment rather than relying on the 2 KB snippet, since
the holding and its reasoning are what you're mapping onto the facts.

## Verifying a citation (the non-negotiable step)

The reliable way to verify a Pakistani authority is to **retrieve and read it**, not to resolve a citation
string. So:

1. **Find it with `search`** — by party name and the legal issue (e.g. "Ashoo Bibi gift specific relief" or
   "oral gift delivery of possession"). Search is the workhorse and it performs well on this corpus.
2. **Open it with `get_document(source, source_id, include_full_text=True)`** and read the relevant part.
   A case only counts as verified once you have read the passage that supports your proposition. Cite it by
   the reporter citation if the text carries one, otherwise by case number + court + date + the returned URL.

`resolve_reference` is a **secondary, opportunistic** tool here — try it when you have a clean citation, but
do not rely on it and do not let it gate what you cite:

- Pass the citation as `reference`, always with `hint_country="PK"` and `hint_type` when known.
- **A null result means nothing.** On this corpus `resolve_reference` returns `resolved: false` even for real,
  reported judgments — including cases that `search` surfaces with their exact reporter citation. So never
  downgrade or drop a case just because `resolve_reference` failed. Fall back to `search` + `get_document`.
- If both `search` and `resolve_reference` fail to surface a remembered or user-supplied citation, put it
  under **"To confirm against the reporter / official record"** and say LDH couldn't retrieve it — never
  present it as verified, never bin it silently.

Set `include_full_text=True` (on `resolve_reference` or `get_document`) when you need the operative paragraph
to quote or pin-cite.

## When LDH can't help (and its limits are real)

The Pakistan corpus is **incomplete and not up to date**. In testing it missed a binding, directly-on-point
Supreme Court judgment purely because the case was decided after the dataset was built — and the Supreme
Court set is small to begin with. Two consequences:

- **An empty or thin result never proves there is no authority.** Do not imply the search was exhaustive.
  Report what you searched and what came back, and flag that the newest or most on-point Supreme Court
  authority may simply not be in the corpus.
- **Prefer the user's own materials for currency.** If the question turns on the current state of the law,
  ask whether the user holds a recent judgment or brief (they often do), and check the reporters / official
  SC and High Court sites for anything decided recently.

Otherwise: if Pakistan isn't well covered for a given source, or a search/resolve comes back empty, say so
plainly. State the statutory analysis (statute text is stable and safe) and clearly mark the case law as
**to be confirmed** — against the official SC/High Court websites or the reporters — rather than presenting
remembered citations as if checked. If a source looks broken or mislabelled, flag it with
`report_source_issue`.
