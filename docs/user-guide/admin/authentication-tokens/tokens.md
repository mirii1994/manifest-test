---
sidebar_position: 1
title: Types of Tokens
---


Logz.io uses tokens to manage data shipping for logs, metrics, and traces; permissions for dashboard sharing links; and for API authorizations.

You will need to be an account admin to create, delete, or access your tokens.

### Which token should I use?

The type of token you use depends on what you're trying to do.
Read on to see your options.

## Tokens to send data to your account

To send data into your account you should use: **Log shipping token**,  **Metrics shipping token**, or **Tracing shipping token**.

The shipping token tells Logz.io which account to send your data to.
Every account has its own tokens.

You can click any **Token** to copy it with one-click.

* Learn more about managing your [Log shipping tokens](https://docs.logz.io/user-guide/tokens/log-shipping-tokens/)
* Learn more about your [Metrics shipping token](https://docs.logz.io/user-guide/accounts/finding-your-metrics-account-token/)
* Learn more about your [Distributed Tracing shipping token](https://docs.logz.io/user-guide/accounts/finding-your-tracing-account-token/)

## Tokens to share dashboards and more

To share dashboards and other elements with others, use: **Shared token**.

Shared tokens allow you to share visualizations and dashboards with anyone, even if they don't have a login to your account.

To limit the data available to shared tokens, attach filters.
Keep your data secure by attaching a filter to every token and deleting tokens you no longer need.

To manage your shared tokens, select [**<i class="li li-gear"></i> > Tools > Manage tokens**](https://app.logz.io/#/dashboard/settings/manage-tokens/shared) in the top menu and select the **Shared tokens** tab.

* For more information on [managing shared tokens](https://docs.logz.io/user-guide/tokens/shared-tokens.html)

## Tokens to develop an integration

If you're interested in developing an integration, you should use **API token**.

Use API tokens to authenticate integrations with your Logz.io account.
API tokens are available to Enterprise and Pro plan subscribers, as well as during an account's trial period.

To manage your API tokens, select [**<i class="li li-gear"></i>Settings > Tools > Manage tokens**](https://app.logz.io/#/dashboard/settings/manage-tokens/api) in the top menu and select the **API tokens** tab.

* For more information on [managing API tokens](https://docs.logz.io/user-guide/tokens/api-tokens.html)
* If you want to build your own integration, visit the [Logz.io API Developer Guide](https://docs.logz.io/api/)

## About token permissions

Tokens are account-specific. This means their permissions are scoped to the account they are created in.
If you change your account permissions, tokens respect the updated permissions.

For example, if you create an API token in your main account, it can be used to search the data indexed in the main account and any of the sub accounts by default.

If you change the account permissions so that the sub account is not searchable from the main account, the main account's API token can no longer be used to search the sub account's data.
