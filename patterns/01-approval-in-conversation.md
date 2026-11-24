# 01. Human approval inside the conversation, not in a flow

## Situation

An agent drafts an outbound email. A human has to approve it before it goes anywhere. The first design called a Power Automate flow from the agent, and the flow used an approval step: the reviewer gets an approval email with Approve and Reject buttons.

## What you see

The approval itself works. The reviewer clicks Approve and the flow run records it correctly. But the agent has already given up: after roughly two minutes it returns an HTTP 504 and closes the connection to the flow. The approval arrives, but nothing is listening anymore.

In testing this happened every time, even when the reviewer responded right away.

## Why it happens

The agent waits synchronously for the flow to return. That call has a timeout of a couple of minutes. A human approval takes as long as a human takes: finding the email, reading it, clicking. Those two timescales don't fit together, so in real use this fails almost every time.

⚠ The exact timeout value is what I observed, not a documented number. Check the current Microsoft documentation for agent flow limits before relying on it.

## What fixed it

Move the approval into the conversation itself. The person talking to the agent is usually the reviewer anyway.

1. The agent shows the full draft in the chat: recipient, subject and body.
2. It asks for two separate confirmations: one for the content, and one explicitly for the email address, which it marks as unverified.
3. Only after both confirmations does it call the action that creates the draft.

The agent just waits for the next message in the same conversation, so there is no service call with its own timeout. Removing the flow also removed one component to maintain.

Instruction excerpt (generalised):

```text
Before creating the draft, show the user the recipient, subject and full body.
Mark the recipient address as UNVERIFIED.
Ask for two separate confirmations:
1. Is the content approved?
2. Is the email address checked and correct?
Do not create the draft until both are explicitly confirmed in this conversation.
```

## When a flow is still the right choice

When the approver is someone other than the user talking to the agent, or when the approval has to be logged and audited outside the conversation. In that case, don't make the agent wait. Let the flow run asynchronously and have it report back through a separate channel, such as a Teams message or an update to a record.

## Catch it earlier

- Test approvals with a realistic delay. Wait five minutes before clicking, not five seconds.
- For every flow an agent calls, ask: can this take longer than a couple of minutes? If yes, it should not be a synchronous call.
