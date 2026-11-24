# 04. Connected agents don't share context, the orchestrator has to pass it on

## Situation

An orchestrator calls sub-agents in sequence. Sub-agent A generates content, in this case several full email texts. Sub-agent B needs that content to do its job.

## What you see

Sub-agent B behaves as if it never got the content. It asks for it again or gets stuck.

## What did not fix it

The first attempt was to describe the data better on the sub-agents themselves: expand the output description of sub-agent A and the input description of sub-agent B so both mention the email texts explicitly. After publishing and testing, the symptom was exactly the same.

## What fixed it

One line in the orchestrator's instructions:

```text
When calling the email sender, pass the subject lines and the full email texts
produced by the email generator, together with the score, literally in the call.
```

After publishing, sub-agent B had everything it needed and went straight on.

## Why it happens

Data moves between connected agents through the call the orchestrator makes. Sub-agents don't see each other's output. If the orchestrator doesn't include something when it calls a sub-agent, that sub-agent doesn't have it, however clearly its own input is described.

Short values, like a single score, often seem to arrive anyway. They probably fit in the summary the orchestrator passes along, while several full email texts don't. ⚠ That's my explanation of what I observed, not something I've confirmed in documentation.

## Catch it earlier

- When a sub-agent is missing data, look at the orchestrator's instructions first, not at the sub-agents.
- For every handoff, say explicitly in the orchestrator's instructions what it has to pass on.
- Test with realistic content sizes. A handoff that works with one short value can fail with three full documents.
