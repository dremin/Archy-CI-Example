# Archy CI Example

This repository is a basic example of how [Genesys Cloud Archy](https://developer.genesys.cloud/devapps/archy/) can be used with CI such as GitHub Actions. Archy flows within the `flows` directory will be automatically published to Genesys Cloud.

## Setup

### Credentials

Archy uses a client ID and secret to authenticate with Genesys Cloud. These can be created in Genesys Cloud by navigating to Admin > Integrations > OAuth.

To allow the CI to authenticate with Genesys Cloud, client credentials are stored as GitHub secrets. To create a secret, navigate within your repository to Settings > Secrets > New repository secret. The following secrets are used in this example:

| Secret               | Purpose                                                               |
| -------------------- | --------------------------------------------------------------------- |
| `DEV_CLIENT_ID`      | The Genesys Cloud OAuth client ID to use for the development org.     |
| `DEV_CLIENT_SECRET`  | The Genesys Cloud OAuth client secret to use for the development org. |
| `PROD_CLIENT_ID`     | The Genesys Cloud OAuth client ID to use for the production org.      |
| `PROD_CLIENT_SECRET` | The Genesys Cloud OAuth client secret to use for the production org.  |

### Region

Archy needs to know which Genesys Cloud region to use for each org. By default, the GitHub action in this repository uses `mypurecloud.com`. This configuration is set using environment variables within the workflow file:

| Variable        | Purpose                                      |
| --------------- | -------------------------------------------- |
| `location_dev`  | The base region URL for the development org. |
| `location_prod` | The base region URL for the production org.  |

## Promotion Workflow

This repository contains two branches: `main` and `develop`. The GitHub action is configured to publish flows to a different Genesys Cloud organization depending on the branch that is pushed. A basic code elevation process might look like this:

1. A developer works within their own branch.
2. The developer opens a pull request to merge into the `develop` branch.
3. After the PR is merged and pushed to `develop`, Archy will publish to the configured development Genesys Cloud org.
4. After functionality is verified in the dev org and ready to deploy to production, a pull request is opened to merge from the `develop` branch to the `main` branch.
5. After the PR is merged and pushed to `main`, Archy will publish to the configured production Genesys Cloud org.
6. Customers can now enjoy your newly published flows in production.
