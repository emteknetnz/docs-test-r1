# docs-test-r1

This is a repo holding test docs

It's used with https://github.com/emteknetnz/docs-test-w1

The workflow file `docs-trigger.yml` is used to trigger the workflow in the other repo. Because it's triggering a workflow in another repo, it needs a token scoped to run workflows in a different repository.

There are a few ways to get this token:
A) Use a personal access token (PAT) with the `repo` scope (legacy token) or `actions` scope with `write` permission (newer token). Downside is this needs to be added to every repository that needs to trigger the workflow, and it's bound to a user account.
B) Use a GitHub App hosted on a personal account. This is the solution trialed here. This is very similar to using a machine user, without the need to login in to GitHub seperately to create the token. The downside with this approach is that like the PAT approach, the private-key needs to be added to every repository that needs to trigger the workflow.
C) Use a GitHub App hosted on an organisation account. This is the recommended way, it allows the app to be added to all repositories in the organisation, and the private key only needs to be added once at the organisation level.

## Creating a GitHub App for a personal account - solution B):
- Create a new GitHub App https://github.com/settings/apps
- Untick any ticked checkboxes
- Generate a private key for the app
- Under https://github.com/settings/apps/<your-new-app>/permissions under "Repository permissions" set `Actions` to `Read & write`
- Install the app on your personal account - https://github.com/settings/apps/<your-new-app>/installations
- Add the private key to any reponsitories with docs (i.e. docs-test-r1) as a respisotry secret `APP_PRIVATE_KEY`
- Also add the app id to repositories with docs, this time as a repository variable rather than a secret, as `APP_ID`

## Other Notes:

We don't use a client secret in the app, instead we use a private key, as noted in https://github.com/orgs/community/discussions/28581#discussioncomment-8260094
- Private key logs activity as the app/machine user e.g. "My useful bot"
- Client secret logs all activity as the user that triggered the action e.g. "SomeRandomGitHubUser156"
