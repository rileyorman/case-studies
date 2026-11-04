# SAIT Vendor Workbench

*For my capstone at SAIT I worked with SAIT Commercial Services on how campus food vendors pay rent. The leases had each grown up on their own terms over the years, and the question was how to compare them on the same footing. I built the team an application that keeps one audited record of sales and rent, runs the monthly routine, and lets them build a rent model and compare it side by side with each vendor's current lease, showing who would pay more and who would pay less. About 1,100 automated checks confirm every figure matches the client's own records to the cent. The app was handed over in September 2026, and the Director of Commercial Services called it "both technically sound and operationally practical."*

A client application built for SAIT Commercial Services in 2026 to administer campus food vendor rent. Delivered under a confidentiality agreement, so this write-up uses no vendor names and no rent figures. The code stays with the client.

## The problem

For my capstone at SAIT I worked with SAIT Commercial Services on how campus food vendors pay rent. The leases had each grown up on their own terms over the years, and the question was how to compare them on the same footing.

The brief was a consultancy: map how every location pays today, make the locations comparable, and give the client an instrument to define a standardized rent model themselves. The same lease parts everywhere, with the amounts set by the client in negotiation.

Fair was never the word, and we learned that early. Everyone at the table has a different idea of fair. One vendor will happily pay more if it prices a competitor out of the building. So the app does not decide what is fair. It makes every location comparable, shows who pays more and who pays less under each model, and leaves the negotiation to the people whose job it is.

## What I built

A Python and Streamlit application that runs on the client's own machine and sends nothing outside it.

- **One master record.** The client's source spreadsheets are read once, reconciled, and written to a single master file that every screen is built from. Nothing downstream is hand-edited. Originals and calculated figures are labelled as such on every screen, and a blank cell means no figure, never a guess.
- **The monthly routine.** Type in a month's sales and rent per storefront, or upload the vendor's spreadsheet. Uploads are checked before they land. They can add to the audited record but never overwrite it. A batch with a warning parks until a person clears it.
- **Corrections with an audit trail.** A fact that turns out to be wrong, a square footage or an agreement end date, can be corrected in the app with who, when and why on the record. A stored figure can be corrected the same way.
- **Lease renewals with a second sign off.** A new or renewed vendor is entered by one person and confirmed by a second sign-in; the enterer cannot confirm their own entry.
- **The Workbench.** Seven premade rent models, run against the current state, side by side: each model's total, who pays more, who pays less, the biggest move, and a multi-year phase-in. The client's own pattern is one of the seven. The team can also build a new model of their own by setting the dials, save any number of scenarios, and mark one as the model of record. A Vendor vs Peers page checks any one storefront against the others under the same model, so the whole portfolio reads consistently. The view never hides who pays more.
- **A one click month end pack** for the CFO. A memo cover with the model of record, the month in hand, what is missing, and the effect against today, then the figures laid out the way the finance team already keeps them, so month end is a read and a paste into their own records.
- **Operator tools** that remove the need for a developer: back up and restore, a write lock on the state files, a Help page with the twelve common questions, and an app doctor that checks the install.

## How I knew the numbers were right

This was the part I cared most about, because a rent model nobody trusts is a spreadsheet with a logo.

- Every rebuild runs thirteen steps and stops at the first failure. A separate audit re-reads the source spreadsheets and re-derives every figure in the master record, about 1,100 automated checks on every rebuild, with a coverage check so no number in the source can go unrecorded.
- The audit proves fidelity, that we transcribed the client's numbers faithfully. It does not claim validity. Where a figure looked wrong, the app raised a flagged question for the client. Anomalies were flagged, never fixed, and every open one lived in a tracker tab enforced by an audit check.
- The rent engine passed hand-recomputed checks. The revenue-neutral models were proven to reproduce today's total to the dollar.
- The app itself passed about 500 checks on a separate test copy of the data, plus about 140 attempts to break every input with bad entries before each release.
- Before handover I ran a rehearsal on made up data. A generator produced a messy year of invented vendor spreadsheets and new-vendor records, and the app had to take every one of them through the routine. Round one found six things. All six shipped before the client saw the app.
- An automated walk opens all eleven pages and scans them for vendor names, so a screenshot or a demo copy cannot carry a vendor name out of the building.

## What shipped

The app went to the client in two visits in September 2026: a live walkthrough with a findings sheet, then the install with printed operator guides and a handover checklist. What the team got: one place to enter the month, every change traceable to who, when and why, every storefront compared side by side under any rent model, and a month end pack that drops into the finance team's own records. The findings from the room were triaged the same evening and the fixes gated and shipped before the second visit. A security brief requested after handover became a hardening release.

## The people

The best part of the project was the people. Livia Kolbe, Director of Commercial Services, and her team treated me as a colleague from the first meeting. They gave me their time, answered the questions the app flagged, sat through the walkthrough and the install visit, and told me straight when something didn't work for them. The findings sheet from the first visit became the fix list for the second. The app is better for it, and I really enjoyed working with them.

After handover Livia wrote, in a public recommendation, that the result was "both technically sound and operationally practical." That is the line I was working for.

## What I learned

- **Say what the number is.** Original, calculated, uploaded, or blank. Labelling the provenance on every cell ended most of the arguments before they started.
- **Flag, never fix.** The temptation to tidy a source figure is strong and always wrong. A logged question with the client's answer attached is worth more than a clean-looking table.
- **The client's own routine is the product.** The capstone question was a standardized rent model. What the client needed on a Tuesday was a clerk's month: type, check, store, run the pack. Building that well is what got the model instrument used.
- **Fair is not a number.** Every vendor and every negotiator had a different one. The useful thing an app can do is make the structures the same and the comparison visible, then get out of the way.
- **Rehearse on made up data before anyone real touches it.** Six findings in round one would have been six embarrassments in the room.
- **Build it so a developer is not needed.** Every "can we do that without you" question from the client became a feature. That is the difference between a project and a tool.

## How it was built

With Claude Code as the pair, every change had to pass the rebuild, the test suites and a scripted walkthrough before it went into the copy handed to the client. The engineering rules that made this work are the same ones in [FinLens](https://github.com/rileyorman/finlens).
