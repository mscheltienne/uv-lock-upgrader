# uv-lock-upgrader

This repository contains a workflow and GitHub app setup instruction for a dependency
bot running `uv lock --upgrade` periodically and opening a PR with the changes.

## Resources

Support for this maintenance operation in dependency bot will be gradually introduced,
tracking is available here:

- [Using uv with dependency bots](https://docs.astral.sh/uv/guides/integration/dependency-bots/#dependabot)
- [astral-sh/uv#2512](https://github.com/astral-sh/uv/issues/2512)
- [dependabot/dependabot-core#10478](https://github.com/dependabot/dependabot-core/issues/10478)
- [dependabot/dependabot-core#10847](https://github.com/dependabot/dependabot-core/issues/10847)
- [dependabot-core uv-related issues](https://github.com/dependabot/dependabot-core/issues?q=is%3Aissue%20state%3Aopen%20uv)

## Application setup

To use the `bot.yaml` workflow in this repository, you'll need to create a GitHub
application, either in your namespace (https://github.com/settings/apps) or in your
organization namespace (https://github.com/organizations/$ORG_NAME/settings/apps). On
the corresponding page:

- Select New GitHub App
- Set the GitHub App name to the name you want: note the name is unique across GitHub,
  thus you'll have to rename `uv-lock-upgrader` in this repository to something else.
- Set the description you want, for instance "This app is used in workflows which
  requires repository permissions to automatically upgrade the `uv.lock` file."
- Homepage URL: set it to your namespace URL or to your organization namespace URL, for
  instance https://github.com/mscheltienne
- Untick `Active` under Webhook
- Under permissions, expand `Repository permissions` and set:
  - Actions: Read
  - Contents: Write
  - Pull requests: Write
- Where can this GitHub App be installed: "Only on this account" is recommended.

Once the application created, you will be redirected to the appplication description and
settings page. Start by copying the `App ID` which will be required later. Then, scroll
down to `Display information` and set the avatar to the logo of your choice, for
instance to the `logo.png` file in this repository. Then scroll downto `Private keys`
and select `Generate a private key`, which should download a `.pem` file needed later.
Finally, at the top of the page on the left pane, select `Install App` and select where
to install the application.

## Secrets setup

The workflow requires 2 secrets:

- `UV_LOCK_UPGRADER_APP_ID`: the `App ID` that you noted earlier.
- `UV_LOCK_UPGRADER_APP_PRIVATE_KEY`: the content of the private key `.pem` file.

Please set those secrets in the repository in which you want to use the workflow, or at
the organization level if applicable.

## Workflow setup

You can now copy and use the `bot.yaml` workflow from this repository. Note that you
have to rename `uv-lock-upgrader` to whatever app name you used.
