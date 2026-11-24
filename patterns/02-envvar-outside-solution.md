# 02. Environment variables only resolve inside the solution

## Situation

A Power Automate flow reads a value from an environment variable, for example a site URL or a list name, so the same flow can move from Dev to Test to Prod without editing it. The environment variable lives in a Dataverse solution. The flow was created outside that solution.

## What you see

While building, nothing looks wrong:

- the designer shows a valid dynamic content chip for the variable
- saving works
- the flow checker reports no errors
- after a refresh the value is still there

Then the first real run fails with:

```text
workflow parameter '<Name> (<prefix>_<name>)' is not found
```

## Why it happens

A flow outside the solution can't resolve the solution's environment variables at runtime. The designer doesn't check this, so the problem only shows up when the flow actually runs.

## What fixed it

1. Add the flow to the solution.
2. Remove the reference and pick the environment variable again from the dynamic content picker.

Step 2 matters. Just adding the flow to the solution was not enough, and a manually typed `parameters('...')` expression kept failing.

## The broader lesson

In Power Automate, "saved" is not proof of "works". For anything a flow references outside itself, such as environment variables, connection references and other solution components, a test run is the only real verification.

## Catch it earlier

- Create flows inside the solution from the start, never in My flows first.
- Do a test run after every change to references, not only after logic changes.
- Before a deployment, check that every flow that uses environment variables is in the same solution.
