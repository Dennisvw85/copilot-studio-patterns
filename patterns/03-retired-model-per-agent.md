# 03. A retired model on one sub-agent breaks the whole setup

## Situation

A multi-agent setup: one orchestrator agent that calls several sub-agents, each with its own instructions and tools. Some time after building it, Copilot Studio shows a "Retired model" label in the model panel of the orchestrator. You switch the orchestrator to a current model, publish, and move on.

## What you see

It's easy to read the label as cosmetic, as if Copilot Studio will quietly fall back to a working model. It doesn't. As long as the label is there, the agent fails with a hard error in the test pane, along the lines of:

```text
You don't have access to the model (<model name>). Please ask your administrator
to add you to the security group that grants access to this external model provider,
then try again.
```

The error mentions access and security groups, which points you in the wrong direction. The actual cause is the retired model.

In my case the orchestrator was fixed, but every sub-agent was still on the retired model. The setup was about to be shared with colleagues, and it would have failed for them the first time it handed off to a sub-agent.

## Why it happens

The model setting is per agent. Fixing it on the orchestrator says nothing about the sub-agents.

## What fixed it

For every agent in the setup, not only the orchestrator:

1. Open the model panel.
2. Pick a current model.
3. Publish.
4. Test in the test pane and confirm the error is gone.

## Catch it earlier

- Treat a "Retired model" label as a blocker, never as a warning.
- Keep a list of all agents in a multi-agent setup and check each one before sharing or deploying.
- Run one end-to-end test that goes through every sub-agent, not just a quick test of the orchestrator.
