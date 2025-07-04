---
description: Convert your Docker Hub user account into an organization
title: Convert an account into an organization
keywords: docker hub, hub, organization, convert account, migrate account
weight: 35
aliases:
- /docker-hub/convert-account/
---

{{< summary-bar feature_name="Admin orgs" >}}

You can convert an existing user account to an organization. This is useful if you need multiple users to access your account and the repositories that it’s connected to. Converting it to an organization gives you better control over permissions for these users through [teams](manage-a-team.md) and [roles](roles-and-permissions.md).

When you convert a user account to an organization, the account is migrated to a Docker Team subscription.

> [!IMPORTANT]
>
> Once you convert your account to an organization, you can’t revert it to a user account.

## Prerequisites

Before you convert a user account to an organization, ensure that you meet the following requirements:

- The user account that you want to convert must not be a member of a company or any teams or organizations. You must remove the account from all teams, organizations, or the company.

    To do this:
    1. Navigate to **My Hub** and then select the organization you need to leave.
    1. Find your username in the **Members** tab.
    1. Select the **More options** menu and then select **Leave organization**.

    If the user account is the sole owner of any organization or company, assign another user the owner role and then remove yourself from the organization or company.

-  You must have a separate Docker ID ready to assign as the owner of the organization during conversion.

    If you want to convert your user account into an organization account and you don't have any other user accounts, you need to create a new user account to assign it as the owner of the new organization. With the owner role assigned, this user account has full administrative access to configure and manage the organization. You can assign more users the owner role after the conversion.

## Effects of converting an account into an organization

Consider the following effects of converting your account:

- This process removes the email address for the account, and organization owners will receive notification emails instead. You'll be able to reuse the removed email address for another account after converting.

- The current subscription will cancel and your new subscription will start.

- Repository namespaces and names won't change, but converting your account removes any repository collaborators. Once you convert the account, you'll need to add those users as team members.

- Existing automated builds will appear as if they were set up by the first owner added to the organization. See [Convert an account into an organization](#convert-an-account-into-an-organization) for steps on adding the first owner.

- The user account that you add as the first owner will have full administrative access to configure and manage the organization.

- To transfer a user's personal access tokens (PATs) to your converted organization,
you must designate the user as an organization owner. This will ensure any PATs associated with the user's account are transferred to the organization owner.

> [!TIP]
>
> To avoid potentially disrupting service of personal access tokens when converting an account or changing ownership, it is recommended to use [organization access tokens](/manuals/security/for-admins/access-tokens.md). Organization access tokens are
associated with an organization, not a single user account.

## Convert an account into an organization

Before you convert an account into an organization ensure you have:

- Removed your user account from any company or teams or organizations
- Created a new Docker ID before you convert an account

See the [Prerequisites](#prerequisites) section for details.

1. Sign in to [Docker Home](https://app.docker.com/login).
1. Select your avatar in the top-right corner and select **Account settings**.
1. In the **Settings** section, select **Convert**.
1. Review the warning displayed about converting a user account. This action cannot be undone and has considerable implications for your assets and the account.
1. Enter a **Username of new owner** to set an organization owner. This is the user account that will manage the organization, and the only way to access the organization settings after conversion. You cannot use the same Docker ID as the account you are trying to convert.
1. Select **Confirm**. The new owner receives a notification email. Use that owner account to sign in and manage the new organization.
