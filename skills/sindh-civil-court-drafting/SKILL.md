---
name: sindh-civil-court-drafting
description: Draft and format pleadings and court documents for civil courts in Sindh, Pakistan under the Code of Civil Procedure 1908 — plaints, written statements, interlocutory applications, replies/counter-affidavits, affidavits, and legal notices. Use this skill WHENEVER the user asks to draft, prepare, format, reply to, or review any Sindh (or Pakistani) civil court document, pleading, application, affidavit, or "written statement"/"WS"/"parawise reply", even if they don't name the skill. Also use it when the user pastes an opposing plaint or application and asks for a reply, or asks for the cause title, verification, prayer, or affidavit for a matter before a Senior Civil Judge / Civil Judge / District Judge in Sindh.
---

# Sindh Civil Court Drafting

Produce court-ready pleadings for civil courts in Sindh under the **Code of Civil Procedure, 1908 (CPC)** as applied in Pakistan. The output must look like a document that can be typed, verified, affirmed before an Oath Commissioner, and filed with the correct court fee.

This skill governs **format, structure and drafting register**. It does not supply substantive law — the facts, reliefs and legal grounds come from the user.

## When to use which template

Read the matching reference file **before drafting**. Do not draft from memory of the structure.

| User wants | Read |
|---|---|
| A fresh suit / plaint (Order VII CPC) | `references/plaint.md` |
| A written statement / parawise reply (Order VIII CPC) | `references/written-statement.md` |
| An interlocutory application + supporting affidavit (Order 39, Order 7 R.11, Order 6 R.17, s.151, etc.) | `references/application-and-affidavit.md` |
| A reply / counter-affidavit to an application | `references/application-and-affidavit.md` |
| Cause title, verification, prayer, affidavit, signature block conventions | `references/formatting-conventions.md` |

Most drafts need `formatting-conventions.md` **plus** the document-type file.

## Core workflow

1. **Identify the document type and the court.** If the court designation, suit number, or party details are missing, insert a clearly marked placeholder like `[COURT: e.g. VIIth Senior Civil Judge, Hyderabad]` — never guess a cause number or party name.
2. **Read the relevant reference file(s)** for the exact skeleton.
3. **If the user pasted an opposing plaint/application**, reply **paragraph by paragraph** — one numbered reply for each numbered paragraph of the original (see written-statement.md). Keep denials specific, not evasive (Order VIII Rr. 3–5 CPC): a general denial is treated as an admission.
4. **Assemble in this order**: cause title → parties → nature of document → body (numbered paras) → prayer → verification → place/date → signature block → supporting affidavit (for applications).
5. **Flag, don't invent.** Any fact, figure, date, or citation the user hasn't supplied becomes a bracketed placeholder for the user to fill.
6. **Offer the Word file.** After the draft, produce it as a `.docx` when asked (this pairs with the built-in docx skill).

## Non-negotiable guardrails

- **Never fabricate case law.** Do not insert PLD / SCMR / CLC / YLR / MLD citations from memory. If a legal authority is needed, either use one the user supplied or write `[CITATION TO BE CONFIRMED: proposition = ...]` so the user verifies it against the actual report before filing. A fabricated citation in a filed pleading is a filing risk.
- **Never invent facts, amounts, survey numbers, dates, or party particulars.** Use placeholders.
- **Preserve the user's theory of the case.** In a written statement aligned with a co-party's position, do not soften or contradict that alignment.
- **Do not give the register a casual tone.** Pleadings are formal, third person, numbered. "The plaintiff submits…", "It is denied that…", "The defendant No. 6 respectfully states…".
- **Court fee / valuation.** Where valuation affects court fee (declaration, recovery, specific performance), add a valuation-for-court-fee note or a placeholder; do not state a fee figure unless the user gives one.

## Check every section, Order and Article against the bundled statutes

Before any provision goes into a plaint, written statement, application or affidavit, grep its text in
`references/statutes/`, read it, cite the exact Order, Rule, sub-rule, Section or Article, and quote the
operative words when the argument turns on them. Mark anything depending on a Sindh amendment or a law not
bundled **[VERIFY]**.

**Use the checklists.** `references/pleading-checklists.md` holds the checklist for each document type: the
Supreme Court's 2026 pleading checklist, written statement (O.VIII), O.VII R.11, s.12(2), O.I R.10,
O.VI R.17, O.XXXIX, O.XXIII, O.III R.4 and s.10. Run the relevant checklist before delivering the draft.
Note in particular that the federal CPC has **no counter-claim rule**: a counter-claim must be framed as a
complete cross-suit with valuation and court fee.

For land and inheritance matters read `references/land-revenue-sindh.md` first.

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
- Land revenue (Sindh): use the `sindh-land-revenue-law` skill for the Sindh Land Revenue Act, 1967 as amended to
  2026. Also bundled here: the Sindh amendment Acts of 1972, 2008, 2013, 2023 and 2026. In Sindh, s.164 revision is
  **30 days** and s.163 review is omitted (Sind Ordinance XI of 1980).

Grep the file for the section or Order heading, read the provision, and quote it. Pakistan Code carries
the federal text; check for a Sindh amendment where one may apply, and mark anything not checked **[VERIFY]**.

## Style rules

- Numbered paragraphs throughout; sub-paras as (a), (b), (c).
- Parties referred to by role and number: "the plaintiff", "the defendant No. 6", not by first name after the cause title.
- One allegation per paragraph in a plaint; one reply per paragraph in a WS.
- Prayer uses lettered reliefs (a), (b), (c) ending with a general/equitable relief clause and costs.
- Every filed pleading ends with a **verification** (pleadings) or is **supported by affidavit** (applications), sworn/affirmed at the place and on the date stated.

## Calibrating to the user's own format

This is a v1 scaffold built on general Sindh/CPC convention. If the user has a previously filed plaint, WS, or application, ask them to share it once and update the templates to match that court's exact cause-title wording, margins, verification phrasing, and signature block. Their filed format always overrides this scaffold.
