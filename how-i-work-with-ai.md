# How I work with AI

I build finance software with Claude Code as the pair. This page says what that means in practice, because "built with AI" can mean anything from a prompt pasted into a chat to a working method. The commit history of [FinLens](https://github.com/rileyorman/finlens) shows the method in detail. This is the short version.

## The split

I decide what gets built, in what order, and what a number means. The model writes most of the code, proposes designs, and reviews. Nothing ships on the model's say-so. Every change passes the same gate whoever wrote it: the test suite, a lint check, and for anything that touches the screen, a scripted walkthrough of the app.

The judgment calls stay with me and get written down. In FinLens that is an adjudication casebook: every time a filing's line could mean two things, the decision, the evidence and the date go in the book, and the next filing conforms to it instead of re-deciding. The model can propose an entry. It cannot ratify one.

## The rules the model works under

They live in a file at the root of the repository (`CLAUDE.md`) that the model reads at the start of every session. The ones that matter most:

- **Working code with evidence beats a documentation argument.** When the model proposed "correcting" a prompt-cache setting to match the published docs, the correction broke caching. The original setting had an empirical test behind it. Now the rule is written: show the test before you change what works.
- **Every number on screen has a receipt or a badge.** A figure either traces to a source line in the filing, or it is visibly marked as unvalidated market data. There is no third state. This came from a real incident: a page computed a margin of safety from the wrong company's market cap because a bare ticker resolved to a different listing. The filing data was perfect and the page was wrong.
- **Normalize at the boundary.** External data comes in once, in one shape. Consumers never see source quirks. The alternative, patching each screen as the bugs appear, produced the same bug four times before the rule existed.
- **Flag, never fix.** The integrity engine checks the numbers and warns. It never changes one.
- **No procedure may depend on a person remembering it.** If a rule exists only in someone's head or in chat history, encoding it is the next task. Machine-enforced first, then a slash command that walks the steps, then a documented check. Human memory is not an acceptable fourth option.

## What the loop looks like

1. I describe the change and the evidence I expect to see at the end, usually a test, a log line, or a screen.
2. The model proposes a design. For anything structural it writes a design brief first and I ratify or redirect it. The briefs are dated and kept.
3. It builds, runs the gate, and reports what it checked, including the sibling code paths it did not change and why. "Checked, not affected" is an acceptable answer. Silence is not.
4. I read the diff and the evidence. If the evidence is a claim rather than an artefact, it goes back.
5. It commits with a message that says what and why. The history is the audit trail.

A second model reviews the week's work on a schedule and writes a dated audit. Disagreements between models are useful: a second extractor reading the same filing independently is how FinLens finds the lines a human needs to look at.

## Where this has gone wrong

- The model is confident when it is wrong, and it is wrong in plausible ways. Every rule above exists because of a specific incident, and the incidents are recorded in the repository with the commit that fixed them.
- Rewriting a working thing to be "cleaner" is the most expensive habit to break. The fix was a rule, not a conversation.
- It is easy to let the model's speed set the pace. The dogfood drills, the walkthroughs and the audit cadence are there to slow the loop down to the speed at which I can still read everything that ships.

## What it is good for

Building the thing that would not otherwise get built. The [SAIT Vendor Workbench](sait-vendor-workbench.md) is a client application with about 1,100 automated checks, delivered by one person in a term. FinLens is a working analysis platform with 750 tests. Neither exists without the pair. Neither is trustworthy without the method.

Riley Orman, Calgary. [GitHub](https://github.com/rileyorman) · [LinkedIn](https://www.linkedin.com/in/riley-orman-mba)
