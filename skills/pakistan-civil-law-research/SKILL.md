---
name: "pakistan-civil-law-research"
description: "Research and apply Pakistani civil law: identify the governing statutes, find on-point Supreme Court and High Court judgments with their ratio, and map that law onto specific facts. Produces a research memo plus a table of authorities with holdings. Use this skill WHENEVER the user asks about Pakistani civil law, a civil case, a judgment, or how a statute or precedent applies — including \"what does the law say on X\", \"find judgments on Y\", \"is there a Supreme Court case for this\", how a judgment applies to facts, the ingredients of a valid hiba or specific performance, researching authorities to support a written statement, reply, plaint or application, or to oppose/support a court order, or explaining civil procedure under the CPC 1908. Trigger it even when the user doesn't say \"research\" — any substantive question about Pakistani civil statutes, case law, or applying a judgment should pull in this skill. It does the RESEARCH and ANALYSIS; hand the result to the drafting skill when the user wants a pleading."
---

# Pakistan Civil Law Research

Turn a legal question into a defensible research product: the governing law, the on-point authorities,
and how they apply to the user's facts. The user litigates real matters in Pakistani civil courts, so the
output has to be practitioner-grade and citation-safe — never decorative.

## Scope

Substantive and procedural **civil** law of Pakistan: contract, specific performance, transfer of property,
gift (*hiba*), inheritance/succession, pre-emption, partnership and rendition of accounts, land revenue,
registration, limitation, and civil procedure under the CPC 1908 and Qanun-e-Shahadat Order 1984. Supreme
Court judgments bind nationwide; High Court judgments bind within their province and persuade elsewhere.
For a matter in **Sindh**, treat Sindh statutes and Sindh High Court authority as primary and flag when a
proposition rests only on another High Court.

Be aware of a practical gap: the Legal Data Hunter corpus for Pakistan currently indexes the Supreme Court
(binding, but a thin set), the Islamabad and Peshawar High Courts (persuasive in Sindh), and the official
statute code — but **no Sindh High Court and no PLD/SCMR reporter series**. So the database can give you
binding Supreme Court authority and persuasive High Court authority and the exact statute text, but Sindh
High Court judgments and reporter-cited cases must be confirmed against the reporters or official court
sites outside LDH. Say this to the user when it affects what you can stand behind, rather than implying
Sindh authority was searched when it wasn't.

This skill does research and analysis. It does **not** draft the final pleading — when the user wants a
plaint, written statement, application, or affidavit produced, do the research here and hand the authorities
to the `sindh-civil-court-drafting` skill.

## The one rule that matters most: never invent a citation

A fabricated case citation put before a court is a professional catastrophe. So:

- **Do not state a case name, citation, or "held that…" from memory as if verified.** Memory is a starting
  point for *what to search for*, not a source you can cite.
- **Verify by retrieving and reading the judgment**, not by trusting a citation string. The reliable path in
  Legal Data Hunter is `search` to find the judgment, then `get_document(..., include_full_text=True)` to
  read it and confirm it actually holds what you say. Only a case you have pulled up and read goes in the
  table of authorities, cited by the identifier and URL LDH returns (see `references/legal-data-hunter.md`).
- **A failed lookup does not mean a case is fake.** `resolve_reference` is unreliable for Pakistani reporter
  citations — it returns nothing even for real, reported judgments (including ones sitting in the corpus). So
  never conclude "unverified, therefore drop it" from a null `resolve_reference` result. When a remembered or
  user-supplied citation won't surface, treat it as a **lead to search for** by party name and issue; if it
  still won't surface, list it under **"To confirm against the reporter / official record"** with a note that
  LDH could not retrieve it — do not present it as verified, and do not silently discard it either.
- Keep the verified table and the "to confirm" list strictly separate. Never blend an unretrieved case into
  the verified table.
- Quote the *ratio* in a few of the court's own words only when the exact wording matters; otherwise
  paraphrase. Always give the paragraph/page pin-cite when you have it.

**Legal Data Hunter is one input, not the whole of the law.** Its Pakistan corpus is incomplete and not
current — it missed a binding, directly-on-point Supreme Court judgment simply because the case post-dated
the dataset. So a search that turns up nothing, or nothing recent, does **not** mean no authority exists.
Never imply the search was exhaustive. State what you did and didn't find, note that the most recent or most
on-point Supreme Court authority may not be in the corpus, and — for anything where currency matters — say
the user should check the latest position (their own materials, the reporters, the official court sites) or
supply any judgment they already hold. This is the same honesty the coverage note demands, extended from
*which courts* the corpus covers to *how current* it is.

If the database is unavailable, say so plainly and give the analysis on the statutes (which are stable and
safe to state) while marking case law as needing verification.

## Verify every section against the official statute text

Section and article numbers are as dangerous as case citations: a wrong Order, Rule or Article in a pleading
is just as embarrassing as a wrong case. So before a statutory provision goes into a memo or is handed to the
drafting skill, check it against the official text, and quote the operative words when the point turns on them.

Sources, in order of preference:

1. **The texts bundled with this skill** in `references/statutes/` (see "Statutes bundled with this skill"
   below). Copies the user has added to the current project are equally good.
2. **Pakistan Code (Ministry of Law and Justice), the official federal source:**
   - Code of Civil Procedure, 1908:
     https://pakistancode.gov.pk/pdffiles/administrator6598dabbad120033d4d42d717dcf9755.pdf
     (page: https://pakistancode.gov.pk/english/UY2FqaJw1-apaUY2Fqa-apeb-sg-jjjjjjjjjjjjj)
   - Qanun-e-Shahadat Order, 1984:
     https://pakistancode.gov.pk/pdffiles/administrator01031a2c8cddc523d08a0df0ec37d7d0.pdf
     (page: https://pakistancode.gov.pk/english/UY2FqaJw1-apaUY2Fqa-apaUY2Npa5plaw%3D%3D-sg-jjjjjjjjjjjjj)
   - Any other federal Act: search https://pakistancode.gov.pk/english/ by title.
   Pakistan Code carries the federal text. The provinces, Sindh included, have made their own amendments to
   parts of the CPC, so for a Sindh matter also check whether a Sindh amendment applies and say so if one may.
3. **Legal Data Hunter** statute text, as described in `references/legal-data-hunter.md`.

If none of these can be reached in the session (the workspace often cannot fetch these sites directly; a
browser or the user's own copy may be needed), state the provision from knowledge but mark it **[VERIFY]**
and tell the user which source to check. Never present an unchecked section number as verified.

## Statutes bundled with this skill

`references/statutes/` holds searchable text of 22 laws downloaded in October 2026 from official sources
(Pakistan Code for federal laws; Sindh Code, Sindh Judicial Academy and Government of Sindh sites for Sindh laws):

- Procedure and evidence: `CPC_1908.txt`, `QSO_1984.txt`, `Limitation_Act_1908.txt`,
  `Sindh_Civil_Courts_Ordinance_1962.txt` (amended to Sindh Act VI of 2025), `Court_Fees_Act_1870.txt`,
  `Suits_Valuation_Act_1887.txt`.
- Property and contract: `Specific_Relief_Act_1877.txt`, `Transfer_of_Property_Act_1882.txt`,
  `Contract_Act_1872.txt`, `Registration_Act_1908.txt`, `Stamp_Act_1899.txt`, `Partition_Act_1893.txt`,
  `Land_Acquisition_Act_1894.txt`, `Illegal_Dispossession_Act_2005.txt`.
- Inheritance and identity: `Succession_Act_1925.txt`, `Muslim_Family_Laws_Ordinance_1961.txt`,
  `NADRA_Ordinance_2000.txt`.
- Land revenue (Sindh): use the `sindh-land-revenue-law` skill, which bundles the consolidated Sindh Land Revenue
  Act, 1967 (as amended to 2026) and the official amending texts. Also bundled here: the Sindh amendment Acts of
  1972, 2008, 2013, 2023 and 2026. The section "Sindh land revenue: verified position" below overrides
  `references/land-revenue-sindh.md` wherever they differ.

### Sindh land revenue: verified position (checked against official texts, Nov 2026)

| Point | Sindh law | Source |
|---|---|---|
| s.42 mutation | Sindh procedure (substituted 1980): report to Mukhtiarkar within 3 months (Sub-Registrar reports registered deeds); entry; notice; objections within **15 days**; open katcheri; certification by Mukhtiarkar; attestation | Ord. XI/1980 s.5 |
| ss.41, 43 | **Omitted** | Ord. XI/1980 ss.4, 6 |
| s.52 | Entry presumed true until contrary proved or new entry lawfully substituted | Ord. XI/1980 s.8 |
| s.53 | Declaratory suit by person aggrieved by entry as to a right **of which he is in possession** | Ord. XI/1980 s.9 |
| s.161 | New first appeal Asst. Collector Gr. II → Gr. I; Board only on point of law; no 2nd appeal where 1st appeal confirms; only Board may remand | Ord. XI/1980 s.12 |
| s.162 | 30 days (Collector), 60 (Commissioner), 90 (Board) | Act |
| **s.163 Review** | **Omitted** — no review before revenue officers | Ord. XI/1980 s.13 |
| **s.164 Revision** | **30 days** (not 90); revisional order cannot be questioned on application of affected party; only Board may remand | Ord. XI/1980 s.14 |
| s.167 | Limitation Act 1908 governs computation **and condonation** | Ord. XI/1980 s.15 |
| s.39 | (2)(e) omitted; new (3) definition of Record-of-Rights; (4) computerised record has same legal effect | Sindh Act XII/2026 s.3 |
| s.42-A(5) proviso | Attested **s.40 record prevails** over computerised record | Sindh Act XII/2026 s.4 |
| s.42-B | Refers to non-existent "sub-section (7)" — drafting error | Sindh Act XII/2026 s.4 |
| s.172 | Bar on civil courts; title questions (cl. xviii, s.141) and fraud in revenue sales (cl. xvi) outside the bar. Sub-s.(1) wording in consolidations appears defective — verify | Act |
| Board review | Board of Revenue may review its own orders, **90 days** | Sindh Board of Revenue Act 1957 s.8 |
| Full Board revision | Only where a Member reversed/modified an order on appeal; not against a Member's revisional order; 90 days | BoR Act s.7(2), 1959 Rules r.5 |
| Civil suits between private parties | Saved even though affected by a revenue-record entry | Sindh Revenue Jurisdiction Act 1876 s.5(b) |
| Suits against Government | Must first exhaust revenue appeals | Revenue Jurisdiction Act 1876 s.11 |

Unresolved [VERIFY] items: Sindh Act XVII of 1975 (amends the Revenue Jurisdiction Act — not seen); the law that
inserted s.42(14), s.55(f) and the computerised wording of s.177; the later amendments to ss.56-A and 70-A
(thresholds differ from 1976); the status of ss.57–69 after Sind Act IX of 1972. Chapter VII (assessment) has
little practical relevance after the abolition of land revenue in Sindh — check current status if it matters.

Grep the file for the section or Order heading, read the provision, and quote it. Pakistan Code carries
the federal text; check for a Sindh amendment where one may apply, and mark anything not checked **[VERIFY]**.

## Workflow

### 0. Start from what the user already has (do this first)

Before searching anything, find out which authorities and materials the user already holds — a judgment
they've been handed, a brief they've drafted, a case they've already mapped onto their facts, an opponent's
pleading citing cases. Litigants often already possess the single controlling authority, and it may be one
the database does not contain (see below). Ask briefly if it isn't already in the conversation.

Read each one and assess it critically — do **not** assume everything handed over belongs in the answer.
Judge every supplied authority on three axes:

- **Relevance.** Is it actually on the question at issue? A case can be uploaded that belongs to a different
  domain entirely (e.g. a land-reform or Martial-Law-Order repugnancy judgment handed into a gift-entry
  inheritance matter). Set the off-point ones aside with a one-line reason rather than padding the memo with
  them — cramming in irrelevant authority weakens the good authority around it.
- **Weight.** A binding, on-point judgment the user uploads outranks anything a search returns — treat it as
  the spine and let the database fill in *around* it, never the other way round. Never let a search miss bury
  a case the user could have told you about.
- **Direction.** A supplied case may *help* or may *cut against* the user. If it is adverse — often it is the
  opponent's best authority — say so plainly, put it in a clearly marked "adverse / to distinguish" section,
  and set out how to distinguish it on the facts. Anticipating the opponent's strongest case is worth more
  than a one-sided memo (this is the evenhandedness the Apply step also demands).

### 1. Frame the question

Restate the legal question in one or two lines and identify: the cause of action or defence, the province
and forum, and the specific facts that will decide it. If the user pasted a plaint, order, or the opponent's
pleading, extract the facts and the reliefs at issue from it rather than asking. Ask a clarifying question
only if a genuinely outcome-determining fact is missing.

### 2. Identify the governing statutes

Consult `references/core-statutes.md` to map the question to the right Act(s) and the specific
sections/Orders in play (e.g. specific performance → Specific Relief Act 1877 ss. 12, 22 + Contract Act
1872; oral gift → Muhammadan Law + Registration Act 1908 s. 17 + Qanun-e-Shahadat on proof; a defence
→ the relevant Order of the CPC 1908). State the statutory ingredients as a checklist, because the
application step tests the facts against each ingredient.

### 3. Find the on-point authorities

Use Legal Data Hunter (`references/legal-data-hunter.md`) to find judgments that construe those sections or
turn on the same issue. As a matter of *weight*, prefer: Supreme Court over High Court; larger benches over
smaller; later over earlier where they conflict; Sindh High Court over other High Courts for a Sindh matter.
As a matter of *what the database actually holds*, expect Supreme Court, Islamabad HC and Peshawar HC results
but no Sindh HC — so a Sindh matter will draw on binding SC plus persuasive HC authority, and you should say
so. For each authority, open the judgment with `get_document` and read it, then capture the **ratio** (the
rule the case actually decides), not just the outcome, plus the pin-cite. Do not table a case you have only
seen as a search snippet.

### 4. Apply the law to the facts

Walk each statutory ingredient and each controlling authority against the user's facts. Say which way each
one cuts, and be honest about the ones that cut against the user — anticipating the opponent's best authority
is worth more than a one-sided memo. Where a judgment is close but distinguishable, state the distinction
explicitly (a fact-by-fact mapping of the authority onto the case).

### 5. Produce the output

Two artefacts, in this order — a memo, then a table of authorities.

## Output format

### Part A — Research memo

Use this structure:

```
# Research Memo: [one-line subject]

**Question presented.** [The precise legal question, framed on the facts.]

**Short answer.** [2–4 sentences. The bottom line, with the degree of confidence and the key authority.]

**Governing law.** [The controlling statute(s) and section(s), stated as ingredients/tests.]

**Authorities.** [Narrative discussion of the leading judgments and what each establishes — see table below for the index.]

**Application.** [Ingredient-by-ingredient mapping of the law onto the facts, including the points that cut against the user and how to meet them.]

**Conclusion & litigation use.** [The bottom line plus concretely how to deploy it — which pleading, which paragraph, which relief, what evidence is needed to prove each element.]
```

### Part B — Table of authorities

A retrieved-and-read-only table. One row per judgment:

| # | Case | Citation | Court / Bench | Ratio (holding) | Relevance to this matter |
|---|------|----------|---------------|-----------------|--------------------------|

Cite each row by the reporter citation if the retrieved judgment carries one, otherwise by case number +
court + date + the official URL (see `references/citation-guide.md`). Any authority you could not retrieve
and read goes in a separate list titled **"To confirm against the reporter / official record"**, with a note
on why LDH couldn't produce it. Never merge that list into the table above.

Where an authority (whether the user supplied it or a search found it) cuts **against** the user, do not omit
it — add a clearly labelled **"Adverse authority (to distinguish)"** section giving the case, what it holds
against the user, and how to distinguish it on the facts. A memo that meets the opponent's best case is worth
far more than one that hides from it.

See `references/citation-guide.md` for the correct citation style (PLD, SCMR, CLC, YLR, MLD, PLJ, etc.).

## Delivery

Default to delivering the memo and table inline in the chat as Markdown, since the user usually wants to read
and react before anything is filed. Offer a Word version if the user wants something to circulate or attach —
and if they want it turned into an actual pleading, route the verified authorities into the
`sindh-civil-court-drafting` skill rather than reproducing the drafting logic here.

## Reference files

- `references/core-statutes.md` — map of the key Pakistani civil statutes and which questions each governs.
  Read this in step 2 to pin the right Act and sections.
- `references/citation-guide.md` — Pakistani law-report citation formats and how to cite a judgment correctly.
  Read this when building the table of authorities.
- `references/legal-data-hunter.md` — how to use the Legal Data Hunter database to find and verify authorities.
  Read this in step 3 (and whenever you need to confirm a citation the user supplies).
- `references/statutes/` — official texts of 22 laws (see the list above). The first place to check a
  section's exact wording.
- `references/pleading-checklists.md` — checklists for plaints, written statements, O.VII R.11, s.12(2),
  O.I R.10, O.VI R.17, O.XXXIX, O.XXIII, O.III and s.10, with the rule text quoted.
- `references/land-revenue-sindh.md` — Sindh land revenue: the Land Revenue Act text warning, key
  sections, revenue vocabulary, area conversion and Hanafi shares.