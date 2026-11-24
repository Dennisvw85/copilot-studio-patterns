# Copilot Studio patterns

Lessons from building and rolling out Copilot Studio agents and Power Automate flows for real users.

Each pattern is something that worked in the maker's test pane and then failed, or nearly failed, once real users touched it. For each one: what you see, why it happens, what fixed it, and how to catch it earlier next time.

All examples are generalised. No customer names, tenant details or customer configuration.

## Patterns

| # | Pattern | Area |
|---|---|---|
| 01 | [Human approval inside the conversation, not in a flow](patterns/01-approval-in-conversation.md) | Copilot Studio, Power Automate |
| 02 | [Environment variables only resolve inside the solution](patterns/02-envvar-outside-solution.md) | Power Automate, ALM |
| 03 | [A retired model on one sub-agent breaks the whole setup](patterns/03-retired-model-per-agent.md) | Copilot Studio, multi-agent |
| 04 | [Connected agents don't share context, the orchestrator has to pass it on](patterns/04-connected-agents-handoff.md) | Copilot Studio, multi-agent |
| 05 | [Works for the maker, fails for colleagues: connector authentication](patterns/05-connector-auth-maker-vs-user.md) | Copilot Studio, connectors |
| 06 | [Sharing an agent is not the same as publishing it to Teams](patterns/06-share-vs-publish-teams.md) | Copilot Studio, rollout |

## A checklist before sharing an agent

Most of these patterns come down to the same thing: the maker's own test is not proof that it works for anyone else.

- [ ] Every agent in the setup is on a current model ([03](patterns/03-retired-model-per-agent.md))
- [ ] Every handoff between agents is spelled out in the orchestrator ([04](patterns/04-connected-agents-handoff.md))
- [ ] Every connector tool has a deliberate User or Maker authentication choice ([05](patterns/05-connector-auth-maker-vs-user.md))
- [ ] All flows and environment variables are in the solution and have had a real run ([02](patterns/02-envvar-outside-solution.md))
- [ ] No synchronous call waits on a human ([01](patterns/01-approval-in-conversation.md))
- [ ] The channel is published and the direct link is tested ([06](patterns/06-share-vs-publish-teams.md))
- [ ] Tested end to end with an account that is not the maker

## About

Built by Dennis van Waas, consultant working on Copilot Studio, Power Platform and Microsoft Foundry.
