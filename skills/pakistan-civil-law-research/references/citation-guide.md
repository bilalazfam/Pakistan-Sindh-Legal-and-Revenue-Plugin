# Pakistani Law-Report Citation Guide

How to cite a judgment so it reads correctly to a Pakistani court and can be located. Get the reporter
abbreviation, year, and page right — a citation the bench can't find is worthless, and a wrong one damages
credibility.

## Standard form

```
Party v. Party, <YEAR> <REPORTER> <FORUM> <PAGE>
```

- **Case name** in italics where formatting allows: *Noor Muhammad v. Ghulam Haider*.
- Follow with the neutral citation. Pakistani practice usually writes the year, the reporter abbreviation,
  the court, then the page — e.g. **PLD 1990 SC 99** or **2021 SCMR 1234**.
- Note that **PLD** conventionally places the reporter abbreviation *before* the year (**PLD 1990 SC 99**),
  while the review/digest reporters place the **year first** (**2021 SCMR 1234**, **2019 CLC 500**).
- Add a **pin-cite** to the paragraph or page relied on wherever possible (e.g. "at para 14" or "at 105").

## Common reporters

| Abbrev. | Reporter | Typical use |
|---------|----------|-------------|
| **PLD** | Pakistan Legal Decisions | Leading/reported judgments, esp. Supreme Court & High Courts; statutes |
| **SCMR** | Supreme Court Monthly Review | Supreme Court judgments |
| **CLC** | Civil Law Cases | Civil matters (High Courts) |
| **YLR** | Yearly Law Reporter | Civil & other matters (High Courts) |
| **MLD** | Monthly Law Digest | Civil & other matters |
| **PLJ** | Pakistan Law Journal | General |
| **PCr.LJ** | Pakistan Criminal Law Journal | Criminal (out of civil scope, but noted) |
| **CLD** | Corporate Law Decisions | Company/banking |
| **SBLR** | Sindh Bar Law Reporter | Sindh matters |
| **NLR** | National Law Reporter | General |

## Forum abbreviations

- **SC** — Supreme Court of Pakistan
- **SC (AJ&K)** — Supreme Court of Azad Jammu & Kashmir
- **FSC** — Federal Shariat Court
- **Kar** / **Karachi** — Sindh High Court, Karachi (also **Hyd** for the Hyderabad bench)
- **Lah** — Lahore High Court; **Pesh** — Peshawar; **Quetta** — Balochistan; **Isl** — Islamabad

## Worked examples

- Supreme Court, PLD reporter: *Qazalbash Waqf v. Chief Land Commissioner*, **PLD 1990 SC 99**.
- Supreme Court, SCMR reporter with pin-cite: *Noor Muhammad v. Ghulam Haider*, **20XX SCMR XXXX** at para 14.
  *(confirm the exact year/page via Legal Data Hunter before citing.)*
- High Court, CLC: *A v. B*, **2019 CLC 500 (Kar)**.

## Citing what Legal Data Hunter actually returns

The LDH corpus is built from court websites and datasets, not the reporter series, so cite from what the
retrieved document gives you:

- **If the judgment text carries a reporter citation** (common in Peshawar HC documents, e.g. a "2014 MLD
  635" or "2013 CLC 724" line at the top), use that reporter citation — it's the form a court expects.
- **If it doesn't** (typical of Islamabad HC and the Supreme Court dataset, which are raw judgment sheets),
  cite by **case number + court + date of decision**, e.g. *Shahida Perveen v. Saeed Ahmed*, Regular Second
  Appeal No. 9 of 2018 (Islamabad High Court, 29 Aug 2022), and give the official URL LDH returns.
- **Flag website-only judgments.** When you only have a court-website judgment with no reporter citation, say
  so, so the user knows to pull the reported citation before filing if one exists.
- Note that this corpus has **no Sindh High Court and no PLD/SCMR series** — Sindh HC and reporter-cited
  authorities have to be confirmed against the reporters or official sites outside LDH.

## Rules of good citation

- **Verify by reading, not by resolving.** Confirm a case says what you claim by pulling it with
  `get_document(..., include_full_text=True)` and reading it. `resolve_reference` is unreliable for Pakistani
  reporter citations and often returns nothing even for real, reported cases — a null result is not evidence
  the case is fake. Do not reconstruct a citation from memory.
- **Pin-cite the ratio.** Point to the paragraph that states the rule you rely on.
- **Note the bench** where it matters — a larger bench or a Full Bench carries more weight, and only the ratio
  (not obiter) binds.
- **Flag conflicts.** If two judgments pull opposite ways, say so and identify the later/larger/binding one.
- **Keep quotes short.** Paraphrase the holding; quote only the few operative words of the ratio when the
  exact language carries legal weight, always with the pin-cite.
