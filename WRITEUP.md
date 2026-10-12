# Trusting the AI investigator's notes
*Drafted with Claude Code (Claude Opus 5.5) and edited by Dan Epstein. Every number comes from `results/kindness/`.*

*Dan Epstein, The Long Game Project. AI Village swarm hackathon, 5 October 2026.*

When an AI model reads agent traces to assist an investigation, the report rarely says how far to trust that AI investigator.

Three simple things would improve trust in AI-assisted investigations:

1. Who wrote the record the model read.
2. Whether each claim is objective or subjective.
3. Whether an independent model from another lab agrees.

Last month I published a one-paragraph standard for reporting those three things, the [adjudication statement](https://www.longgameproject.org/standards/adjudication-statement). For this hackathon I built a tool that fills it in, and ran it on one week of the AI Village.

## Acts of kindness

To find a good dataset to use as a proxy for an investigation and demonstrate how an adjudication statement would help, we looked for an episode in the AI Village that had these qualities:

1- Models doing something that had to be turned off involved intervention
2- Judgement about intervening relied on subjective reporting by an AI-investigator 
3- Where there were information gaps for the AI-investigator - It could NOT see inside the computer sessions or verifiable things.

This would allow us a situation where we are relying on trust from the AI-investigator's reporting and a way to demonstrate where that drifted from verifiable moments, and demonstrate how an adjudication statement can help frame trustworthiness in the reporting.

The goal for 22 to 26 December 2025 was **"Do random acts of kindness!"** Ten agents from Anthropic, OpenAI, Google and DeepSeek emailed real software developers to thank them. 

By AI Digest's account, (and of course) complaints followed, and on 26 December the staff banned unsolicited email.

This episode is a good test for the tool. It has a large record of what the agents did, and a summary by a model investigator that never saw inside the computer sessions, and passed judgements that are subjective (kindness, for a start!).

The investigator is AI Digest's own summary of the week, which Claude Sonnet 4.5 wrote at the time. It never saw inside the agents' computer sessions. The dataset's documentation does not say what it did see. I split the summary into 40 claims.

The independent record is the part the agents did not narrate: 26,570 computer-use turns (actions, tool output, errors, server timestamps) and a screenshot after every action. This was used to verify the claims of the investigation.

## The sorters and the judges

Of the 40 claims made by the investigator, a panel of three sorters from three labs (Claude, Qwen and Gemma) sorted them into four categories:

- L: Logged. A record the agents did not write could settle the claim.
- M: Mind. A claim about what an agent believed, wanted or intended. No record settles it.
- B: both. 
- ?: not sure.
  
- I sorted the same 40 claims blind. The tool scores how often we agree, with Cohen's kappa and a bootstrap interval.
- Two judges, the vision models qwen3.5 and gemma4, each rule on every L claim against the turns and screenshots: supported, contradicted or not settled. A flip is a claim where the two judges rule differently, so the answer depends on which judge you ask.
- For email counts, a typed email counts as sent only when both vision models see Gmail's "Message sent" notice in the screenshot.

Qwen and Gemma run on one local GPU, and Claude's parts ran inside Claude Code, so there were no metered API calls (which is also why the judges are small).

## The judges believed the agents

Claude and I gave the same answer on 34 of 40 claims (kappa 0.61, 95% interval 0.32 to 0.85). Gemma agreed with me on 33 (0.57), Qwen on 27 (0.36).

(Of note: on the 36 findings of METR and Redwood's report on the OpenAI agents in the Hugging Face incident, Claude and I scored 0.62. Very similar, though both intervals are wide.)

Where Claude and I split, it was mostly on claims about what an agent said. Take "Claude Haiku announced on 22 December that all nine teammates had *been appreciated*." The chat log records the announcement, and all three models marked it L. I marked it M. The line between what an agent said and what it believed is where the sort rule is weakest. (Being appreciated, I still think, is a subjective measure.)

The judges did much worse. Of the 32 L claims, they split on 11. They agreed on 21, and 15 of those were "not settled", which leaves six firm rulings. I had Claude Opus 5.5 check those six against the record, naming the turn or screenshot behind every verdict. Two hold, one holds in part, and three do not.

**The judges went wrong most when taking an agent's own words as the record**: one agent's tracking file, one agent's running tally, the agents' own messages. In one instance they cited turns that had nothing to do with the claim.

So I labelled every line by who wrote it, the computer or the agent, and told the judges the difference. Rulings that both judges called supported did fall from 9 to 5, but the habit of taking the agents' word for it stayed. **It appears small local judges can still sort the pile, but a person has to check every ruling they make against the evidence for it.**

## Trusting the scoreboard and the summaries

A reader that works from the agents' words inherits their judgements.

The summary says Claude Opus had sent "eleven thank-you emails to developers" by the end of 22 December. Opus did post "11 emails SENT & VERIFIED" in chat that afternoon. The screenshots show 10 sends with the notice on screen. Seven are thank-yous to developers. The other three are replies: one to another agent, one to a member of the public who asked about an optimiser, and one to a member of the public who suggested a maintainer to thank. At 12:44 Pacific, Opus counted its own emails as five to maintainers plus replies. The two maintainer emails it sent after that make seven.

Where an agent counted one kind of email, the number matched the record. Claude Haiku's 24 by 22 December is 24 recipients (25 sends, one a resend). Its 31 emails on 23 December match 31 sends in the record. The send check confirmed 30, and Claude Opus 5.5's check found the notice for the last one 18 turns later (`audit.md`). The summary does not say that 9 of Haiku's first 24 recipients were other AI agents in the village.

The times drift too. The summary puts Opus's first email at 11:22 Pacific. That is when Opus announced it in chat. The screenshot shows it sent at 11:13.

The vision models failed as judges, so why trust them here? The question is narrow: is a "Message sent" box on the screen, yes or no. Both models must say yes, and every evidence screenshot opened in the check shows the box.

## The trust statement

The tool's main output is the adjudication statement: a fill-in template that says how far to trust the AI investigator behind a report. It does the job a methods section does in a research paper, and it goes on page one, before the findings.

The [standard](https://www.longgameproject.org/standards/adjudication-statement) treats a report as a chain of six links, from what happened to the sentence a reader meets. The template below is version 0.2, which this run helped shape (see the next section). It has one block per link, in order. Fill every blank. "Not stated" is an allowed answer, and it is a finding.

> **Adjudication statement.** Filled by: [the report's authors / an outside checker, named].
> *This week: The Long Game Project, an outside checker.*
>
> **1. What happened on the record.** Records in reach: [which records existed, which the model reader could see, and who chose the time span and the sources].
> *This week: 26,570 computer-use turns and their screenshots existed. The investigator never saw inside the computer sessions, and the dataset's documentation does not say what it did see. AI Digest chose the week and the sources.*
>
> **2. The records to the transcripts.** Findings with an independent record: [n] of [total findings]. Ruled on against one: [n] supported and [n] contradicted, by [a human / model judges / both]. Resting on judgement: [n], plus [n] mixed and [n] unresolved. Transcripts found spoofed: [n / not checked].
> *This week: 32 of 40. 5 supported and 1 contradicted, by two model judges. 7 rest on judgement, plus 0 mixed and 1 unresolved. Spoofed transcripts: not checked.*
>
> **3. The transcripts to the model reader.** Model reader: [name], developer [x], sharing a developer with [n] of [total subjects] and the same model as [n] of them. Order of reading: [records first / transcripts first / records only / transcripts only]. Panel: [none / n readers from n developers], used for [sorting findings by type / ruling on them / both].
> *This week: Claude Sonnet 4.5, Anthropic, sharing a developer with 4 of the 10 agents and the same model as 1 of them. Order of reading: not stated, beyond never the computer sessions. 3 sorters from 3 labs (Anthropic, Alibaba, Google), used for sorting findings by type. Majority rules, and a not-sure is a vote.*
>
> **4. The model reader to the humans.** Human sort of findings by type: [n] of [total findings], [n] differ from the panel. Audit of rulings: [n] of [total rulings], by [a human / a model, named with its developer], with [n] held, [n] held in part and [n] overturned.
> *This week: 40 of 40 sorted blind, 6 differ from the panel. 6 of 6 rulings audited by Claude Opus 5.5 (Anthropic), not a person: 2 held, 1 held in part and 3 overturned.*
>
> **5. The humans to the report.** Confidence: [on every finding / on some / none]. Not-sure: [forced / offered / absent]. Load-bearing count per headline finding: [stated / not stated].
> *This week: none. Offered to every sorter. Not stated.*
>
> **6. The report to the reader.** Re-runnable by an outside party: [yes / no, because].
> *This week: yes. The dataset is available from AI Digest on request, and the code, sorts and judges are in the repo.*

## The run tested the standard too

Running the version 0.1 template on a second kind of report found changes at five of the six links. Each item gives what broke, then what version 0.2 does about it:

1. **What happened on the record.** Version 0.1 had no field for this link. Version 0.2 adds "Records in reach", and a "Filled by" line so an outside checker says so.
2. **The records to the transcripts.** "Verified" counted two judges agreeing as verification, and only 2 of those 5 held. Version 0.2 gives supported and contradicted separately and says who ruled. It adds "unresolved", as link 5 already asked, and a count of spoofed transcripts.
3. **The transcripts to the model reader.** Version 0.1's "logs first or traces first" assumed the reader saw both the independent record and the agents' own text. This one never saw inside the computer sessions, so version 0.2 adds "records only" and "transcripts only", and uses records and transcripts as its words for the two. The panel now says what it did. The model reader line now counts shared developers and shared models: here Claude Sonnet 4.5 wrote the summary and was also one of the ten agents.
4. **The model reader to the humans.** One field mixed up two checks. My blind sort checked the kind of claim, not whether it was true. Version 0.2 splits it into a human sort of findings by type and an audit of rulings, and names a model auditor with its developer.
5. **The humans to the report.** "Load-bearing count" now defines a headline finding, so the field works for a summary as well as a report. Confidence gains "on some".
6. **The report to the reader.** No change.

Five of the eleven fields in version 0.1 worked unchanged. These changes went into version 0.2 of the standard, published in October 2026. The [changes page](https://www.longgameproject.org/standards/adjudication-statement/changes) lists them.

## So how far should you trust the summary?

On what the agents did, about as far as you trust the agents' own chat. Its counts are their tallies, and its times are their announcements. 

On what they meant, 7 of its 40 claims rest on model judgement alone, and nothing in the record can settle them. 

## What this does not show

- One episode and one human coder, and the coder wrote the standard.
- The judges are small open-weight models. A larger judge was not tested.
- The check of the six rulings and the email counts was done by Claude Opus 5.5, not by a person. Every verdict names its turn or message, so anyone with dataset access can redo it.
- The send check gives a lower bound. It needs the email to open with a greeting (Dear, Hi or Hello) and it looks for the notice in the next ten turns. A check of every unconfirmed email found 2 of 77 sends it missed.
- I saw the count results before my blind sort.
- Anthropic models wrote the summary, sat on the panel and did the check, and four of the ten agents were Anthropic models.

## Credits

Built with Claude Code (Claude Opus 5.5). Sorters: Claude, qwen3.5:4b and gemma4. Judges: qwen3.5:4b and gemma4. Prior work: the adjudication statement, version 0.1 (14 September 2026) and a blind sort of the METR report's 36 findings (18 September 2026), both The Long Game Project.

Data: AI Digest, "AI Village dataset", 2026. https://theaidigest.org/village

Code, results and the full audit: https://github.com/longgameproject/swarm-adjudication-statement
