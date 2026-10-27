# SAIT Vendor Workbench

A client application built for SAIT Commercial Services in 2026 to administer campus food-vendor rent. Delivered under a confidentiality agreement, so this write-up uses no vendor names and no rent figures. The code stays with the client.

## The problem

Sixteen campus food vendors paid rent under sixteen different arrangements: flat rent, a percentage of sales, operating-cost recoveries, common-area charges, tiers, and combinations of these, written into leases over many years. Nobody could say in one sentence what a vendor paid for its space or whether two vendors with similar sales paid anything like the same amount. The monthly routine ran on spreadsheets that arrived from the vendors in different shapes, and every question about what a location should pay turned into a day of reconciliation.

The brief was a consultancy: map how every location pays today, make the locations comparable, and give the client an instrument to define a standardized rent model themselves. Standardized in structure, not in amount. The same lease parts everywhere, with the amounts set by the client in negotiation.

Fair was never the word, and we learned that early. Everyone at the table has a different idea of fair. One vendor will happily pay more if it prices a competitor out of the building. So the app does not decide what is fair. It makes every location comparable, shows who pays more and who pays less under each model, and leaves the negotiation to the people whose job it is.

## What I built

A Python and Streamlit application that runs on the client's own machine with no external calls.

- **A canonical record.** The client's source workbooks are read once, reconciled, and written to a single JSON file that every view regenerates from. Nothing downstream is hand-edited. Originals and calculated figures are labelled as such on every screen, and a blank cell means no figure, never a guess.
- **The monthly routine.** Type in a month's sales and rent per storefront, or upload the vendor's spreadsheet. Uploads pass a validation boundary that can extend the audited record but never override it. A batch with a warning parks until a person clears it.
- **Corrections with an audit trail.** The two facts this engagement actually had to correct, a square footage and an agreement end date, can be corrected in the app with who, when and why on the record. A stored figure can be corrected the same way.
- **Lease renewals with maker-checker.** A new or renewed vendor is entered by one person and confirmed by a second sign-in; the enterer cannot confirm their own entry.
- **The Workbench.** Seven premade rent models, run against the current state, side by side: each model's total, who pays more, who pays less, the biggest move, and a multi-year phase-in. The client's own pattern is one of the seven. The view never hides who pays more.
- **A one-click month-end pack** for the CFO. A memo cover with the model of record, the month in hand, what is missing, and the effect against today, then the figures laid out the way the finance team already keeps them, so month-end is a read and a paste into their own records, not a rebuild from vendor spreadsheets.
- **Operator tools** that remove the need for a developer: back up and restore, a write lock on the state files, a Help page with the twelve common questions, and an app doctor that checks the install.

## How I knew the numbers were right

This was the part I cared most about, because a rent model nobody trusts is a spreadsheet with a logo.

- The pipeline rebuilds everything in thirteen steps and stops at the first failure. An independent audit script re-reads the source workbooks and re-derives every figure in the canonical record: about 1,100 automated checks on every rebuild, with a coverage pass so no source number can hide uncaptured.
- The audit proves fidelity, that we transcribed the client's numbers faithfully. It does not claim validity. Where a figure looked wrong, the app raised a flagged question for the client. Anomalies were flagged, never fixed, and every open one lived in a tracker tab enforced by an audit check.
- The rent engine passed hand-recomputed checks. The revenue-neutral models were proven to reproduce today's total to the dollar.
- The app layer ran about 500 checks in an isolated data directory, and an abuse pass of about 140 attacks on every input before each release.
- Before handover I ran a dogfood drill: a generator produced a messy synthetic year of vendor spreadsheets and new-vendor records, and the app had to take every one of them through the routine. Round one found six things. All six shipped before the client saw the app.
- A headless walk renders all eleven pages and runs an anonymization leak scan, so a screenshot or a demo copy cannot carry a vendor name out of the building.

## What shipped

The app went to the client in two visits in September 2026: a live walkthrough with a findings sheet, then the install with printed operator guides and a handover checklist. The findings from the room were triaged the same evening and the fixes gated and shipped before the second visit. The client's technical services group is now weighing bringing the app inside the institution's own environment, which it does not normally do for a department-sourced tool, and asked for a security brief, which became a hardening release.

## What I learned

- **Say what the number is.** Original, calculated, uploaded, or blank. Labelling the provenance on every cell ended most of the arguments before they started.
- **Flag, never fix.** The temptation to tidy a source figure is strong and always wrong. A logged question with the client's answer attached is worth more than a clean-looking table.
- **The client's own routine is the product.** The capstone question was a standardized rent model. What the client needed on a Tuesday was a clerk's month: type, check, store, run the pack. Building that well is what got the model instrument used.
- **Fair is not a number.** Every vendor and every negotiator had a different one. The useful thing an app can do is make the structures the same and the comparison visible, then get out of the way.
- **Dogfood on synthetic data before anyone real touches it.** Six findings in round one would have been six embarrassments in the room.
- **Build it so a developer is not needed.** Every "can we do that without you" question from the client became a feature. That is the difference between a project and a tool.

## How it was built

With Claude Code as the pair, every change gated by the pipeline rebuild, the test suites and a scripted walkthrough before it reached the transfer copy. The engineering rules that made this work are the same ones in [FinLens](https://github.com/rileyorman/finlens), and the short version is in [how I work with AI](how-i-work-with-ai.md).
