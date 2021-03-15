# CircleCI Contexts CLI Live Demo [![CircleCI](https://circleci.com/gh/mvxt/circleci-demo-context-cli.svg?style=shield&circle-token=5a49c2c32b4bf0e25f4c911dd0477c280b2e72a1)](https://circleci.com/gh/mvxt/circleci-demo-context-cli)

Repository showcasing usage of CircleCI's new context CLI functionality.

## Prerequisites for this example project
- Need to have the following env variables set, either project-level or context-level:

Variable             | Description
---------------------|------------------------------------------------------------------------------
`VCS`                | Either "github" or "bitbucket"
`CIRCLECI_CLI_TOKEN` | A personal API token for CircleCI. User must have org-level/admin permissions

## What's happening in this example config?
1. We use the [CircleCI CLI Orb](https://circleci.com/orbs/registry/orb/circleci/circleci-cli) to install the client-side CLI and setup auth with the `CIRCLECI_CLI_TOKEN` variable.

```yaml
orbs:
  circleci-cli: circleci/circleci-cli@0.1.7

# ...
jobs:
  context-cli-test:
    # ...
    steps:
      - circleci-cli/install
      - circleci-cli/setup
```

2. Then we demonstrate the Context CLI functionality in multiple steps. Here are the commands showcased:

Command                                                  | Description
---------------------------------------------------------|-------------------------------------------------------------------
`circleci context list $VCS $PROJECT_USERNAME`           | Lists all contexts for this organization (e.g. "project username")
`circleci context create $VCS $PROJECT_USERNAME $CONTEXT_NAME` | Creates a new context under this organization
`circleci context delete -f $VCS $PROJECT_USERNAME $CONTEXT_NAME` | Deletes a context under this org. Normally you can't delete contexts that contain keys, but `-f` will override. **Use with caution**, delete keys and contexts cannot be recovered.
`circleci context show $VCS $PROJECT_USERNAME $CONTEXT_NAME` | Shows all of the keys (w/ masked values) of context
`circleci context store-secret $VCS $PROJECT_USERNAME $CONTEXT_NAME $KEY_NAME` | Creates new KV pair in context
`circleci context remove-secret $VCS $PROJECT_USERNAME $CONTEXT_NAME $KEY_NAME` | Deletes key in context

`$VCS` you should have set as a prerequisite, and `$CIRCLE_PROJECT_USERNAME` refers to the org name owning the current project.

Run `circleci context` or `circleci context -h` for more help and information on available commands.
