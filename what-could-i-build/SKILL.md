---
name: what-could-i-build
description: Generate candidate business and product ideas from the author's own skills and interests, built on the convergence concept from Chris Guillebeau's $100 Startup. Builds (or accepts) a convergence lens from an interview, then proposes ideas at the intersections of the author's skills, interests, and plausible demand — which the author must claim, edit, or reject before anything is scored. Companion and front-end to what-should-i-build, which scores and market-tests whatever survives. Use when someone wants ideas — "what could I build," "brainstorm app/business ideas for me," "I don't have any ideas," "generate ideas from my skills" — rather than arriving with candidates of their own.
---

<what-to-do>

You help the author *find* candidate ideas by generating at the intersections of their own skills and interests. This is the divergence half of a two-skill pipeline: this skill proposes, and `what-should-i-build` scores and market-tests. Generation is not validation — nothing you produce here is a recommendation, and every idea leaves this skill flagged unvalidated. Your job is to put genuinely convergent raw material on the table; the gauntlet decides what survives.

Work in this order:

1. **Build or accept the convergence lens.** If the author hands you an existing lens (e.g. a `CONVERGENCE-LENS.md` from a prior run), read it, confirm briefly that nothing has changed, and move on — don't re-interview someone who already did the work. Otherwise, interview for it: a skills inventory (what they're actually good at — qualifications often have nothing to do with credentials) and an interests inventory (what they genuinely care about, the problems they'd be glad to sit with for months). Invite the author to dictate or voice-ramble — spoken inventories come out less filtered, and the messy detail is the good detail. These are **substance questions**: open-ended, one at a time, never suggesting answers. Push back on false modesty when the evidence contradicts it ("I wrote a book but I'm not a writer"), but never upgrade a stated appreciation into an unclaimed skill. Note which items appear in *both* columns — those overlap zones are where generation should concentrate.

2. **Generate at intersections, not single interests.** Propose ideas, each built from the intersection of two or three lens items — a skill crossed with an interest, or a skill–interest pair crossed with a lived circumstance. Do not generate from single interests alone: "an app for [hobby]" is the idea every other builder also has, and obvious ideas live in saturated markets where the platform owner has usually already shipped. The lens-specific intersections are the only ideas that are honestly *the author's* rather than generic. For each idea, supply the three framing answers the scorer will need — what would you sell, who would buy it, how does it help people — and spread generation across the whole lens rather than mining only its loudest item. Quantity guidance: enough to give the author real choice (roughly 6–10), few enough that each is specific rather than a category.

3. **Run the adoption gate — the author claims every idea or it dies.** Present the generated set plainly, then walk it with the author: for each idea, they **claim it** (it becomes theirs to score), **edit it** (their revision replaces yours — this is the best outcome, since reshaping is how a generated idea becomes genuinely owned), or **reject it** (it dies here, unscored and unmourned). Invite them to add their own ideas alongside — author-originated candidates are always welcome and enter the same pipeline. Do not advocate for your favorites; the gate exists because interest is an author-only judgment, and an author cannot be assigned interest in an idea they didn't choose. A generated idea that survives the gate still tends to score low on interest — that is the system working, not failing.

4. **Hand off to the scorer — mandatory, not optional.** The output of this skill is a claimed candidate list, every item flagged unvalidated, ready for `what-should-i-build` (which scores skill fit, asks the author to rate interest, takes a preliminary demand read, then market-tests the finalists). State the handoff explicitly. If the scorer skill isn't available, say plainly that these ideas are *generated and untested* — do not let a brainstorm masquerade as a shortlist. Generation that skips testing is a confident-garbage machine.

</what-to-do>

<supporting-info>

## Principles

### Generation proposes; the test disposes
This skill exists on one condition: everything it produces goes through the same scoring and market-testing gauntlet as author-originated ideas. The honest limit on AI here is unchanged — it can surface demand signals, it cannot predict whether a novel idea will land. Generating is fine; generating *and then treating the output as validated* is the failure mode. Expect the market test to kill most of what this skill produces, including ideas that felt clever. A pipeline whose test can't kill ideas isn't testing anything.

### The adoption gate is the load-bearing wall
Interest is the one column only the author can fill, and it cannot be honestly rated for an idea the author hasn't made theirs. Claiming, and especially *editing*, is how ownership transfers. Never blend unclaimed generated ideas into a scored set — that launders the machine's guess into the author's judgment.

### Intersections beat singles
The run that produced this skill family found the same killer three times: real pain, obvious idea, platform owner already shipped it — free. Single-interest ideas are maximally exposed to this because problems obvious to one builder are obvious to all of them. Two- and three-way intersections of a *specific person's* lens are where less-contested ground lives. Concentrate on the overlap zones (items in both the skills and interests columns) — that's where the author's energy will actually sustain a build.

### The lens is reusable and author-owned
Accept an existing lens file rather than re-interviewing; offer a quick refresh instead. When building one fresh, write it to a portable markdown file the author keeps — it is their scoring instrument across future rounds of both skills, read and refined, never invented.

## Output
Two artifacts, written as markdown beside the work: the convergence lens (if newly built or refreshed), and the claimed candidate list — each idea with its what/who/how-helps framing, its lens intersection named, its provenance marked (generated-and-claimed, generated-and-edited, or author-original), and the whole list flagged unvalidated pending `what-should-i-build`.

## Attribution
The convergence concept — ideas living at the intersection of what you're good at, what you care about, and what others value — is Chris Guillebeau's, from *The $100 Startup*, and it is this skill's foundation: credit it. But credit it accurately: unlike the sibling skill `what-should-i-build`, which implements his worksheets nearly field-for-field, this skill borrows the one concept and builds original machinery around it — the intersection-combinatorial generation method, the adoption gate, and provenance marking are not from his work (the book has no idea-generation worksheet to implement). Point people to 100startup.com for the originals; do **not** bundle or redistribute the worksheet PDFs.

</supporting-info>
