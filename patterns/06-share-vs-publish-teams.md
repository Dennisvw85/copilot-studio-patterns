# 06. Sharing an agent is not the same as publishing it to Teams

## Situation

An agent is ready for colleagues. The maker shares it with them and tells them to find it in Teams.

## What you see

Colleagues can't find it. Searching for the agent's name in the Teams apps search returns "We didn't find any matches", even for people on the share list.

## Why it happens

There are two separate steps, and both are needed:

1. **Share** decides who is allowed to use the agent. It doesn't make the agent visible anywhere.
2. **Publish to the Teams and Microsoft 365 Copilot channel** makes it available in Teams. This has to be switched on in the channel settings and published separately.

Even with both done, a custom agent doesn't show up in the general Teams apps search. That's expected behaviour, not a delay you have to wait out.

## What fixed it

1. Turn on the Teams and Microsoft 365 Copilot channel and publish.
2. Send users the direct link, not "search for it in Teams". The channel settings have a button to open the agent in Teams. That link works for everyone on the share list.

Optional, depending on the audience:

- **"Show to my teammates and shared users"** in the channel's availability options lists the agent in the Teams app store under Built with Power Platform for the people it's shared with.
- **Showing it to the whole organisation** needs admin approval.
- **Microsoft 365 Copilot** has its own setting to make the agent available there. After publishing, it can take a while before the agent appears.

⚠ Channel names and menu labels in Copilot Studio change regularly. Check the current UI and Microsoft documentation.

## Catch it earlier

- Treat "shared", "published to the channel" and "can actually be used" as three separate checks.
- Write the rollout message with the direct link in it, and test that link with an account that is not the maker.
- Combine this with [05](05-connector-auth-maker-vs-user.md): an agent that is visible can still fail at its first connector tool.
