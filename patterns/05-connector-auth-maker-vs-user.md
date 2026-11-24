# 05. Works for the maker, fails for colleagues: connector authentication

## Situation

An agent uses connector tools, for example SharePoint to read a list and Outlook to create a draft. The maker tests it in the test pane and everything works. Then the agent is shared with colleagues and they use it in Teams or Microsoft 365 Copilot.

## What you see

For colleagues, the agent stops at the first tool that uses a connector. Sometimes it says it needs permission to access a source. In one case it told the user to click Allow or Deny on a card that never appeared. That text wasn't in any instructions: the model made it up after the tool call failed.

For the maker, the same steps work without any issue.

## Why it happens

Each connector tool has an authentication setting with two options:

- **User**: the tool runs as the person talking to the agent, with that person's own connection.
- **Maker**: the tool runs with the maker's connection, for everyone.

With User authentication, each user needs their own working connection. The maker already has one, so the problem never shows up in the maker's own tests.

Things I ruled out along the way:

- **Sharing the sub-agents with users.** In my setup the orchestrator was shared with users and the sub-agents were not. The handoffs between sub-agents worked anyway. The failure only started at the first sub-agent with a connector tool.
- **Sharing the maker's connection.** Not possible. The connection's share dialog says it can only be shared with service principals.
- **SharePoint permissions.** The users were members of the site.

## What fixed it

Choose the authentication mode per tool, based on whose identity actually matters.

- **Shared data that everyone reads the same way**, like a team SharePoint list or a document library: use Maker authentication, publish again, and test with another account. This fixed the same problem in a second project, where a colleague couldn't reach a SharePoint source until it was switched to the maker's credentials.
- **Actions that belong to the user**, like creating a draft in their own mailbox: keep User authentication. Then each user has to create their own connection once. In Teams this also depends on single sign-on being set up for the agent, which needs an app registration with admin consent. ⚠ I didn't get to fully confirm that route end to end.

Note: if the agent is part of a managed solution, you can't change the tool configuration in the target environment. Change it in Dev and deploy again.

## Catch it earlier

- Always test with a second account that is not the maker, before sharing.
- For every connector tool, decide on purpose: should this run as the user or as the maker?
- If the agent will be used in Teams with User authentication, sort out single sign-on before go-live, not after.
