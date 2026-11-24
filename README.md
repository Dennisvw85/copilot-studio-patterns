# Copilot Studio patterns

Lessons from building and rolling out Copilot Studio agents and Power Automate flows for real users.

Each pattern is something that worked in the maker's test pane and then failed, or nearly failed, once real users touched it. For each one: what you see, why it happens, what fixed it, and how to catch it earlier next time.

All examples are generalised. No customer names, tenant details or customer configuration.

## Patterns

| # | Pattern | Area |
|---|---|---|
| 01 | [Human approval inside the conversation, not in a flow](patterns/01-approval-in-conversation.md) | Copilot Studio, Power Automate |
| 02 | [Environment variables only resolve inside the solution](patterns/02-envvar-outside-solution.md) | Power Automate, ALM |

## Planned

- A retired model on one sub-agent breaks the whole multi-agent setup
- Connected agents don't share context, the orchestrator has to pass it on
- Works for the maker, fails for colleagues: connector consent in Teams
- Sharing an agent is not the same as publishing it to Teams

## About

Built by Dennis van Waas, consultant working on Copilot Studio, Power Platform and Microsoft Foundry.
