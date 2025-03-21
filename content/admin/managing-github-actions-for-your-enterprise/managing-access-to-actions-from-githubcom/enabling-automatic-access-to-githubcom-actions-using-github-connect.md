---
title: Enabling automatic access to GitHub.com actions using GitHub Connect
intro: 'To allow {% data variables.product.prodname_actions %} in your enterprise to use actions from {% data variables.product.prodname_dotcom_the_website %}, you can connect your enterprise instance to {% data variables.product.prodname_ghe_cloud %}.'
permissions: 'Enterprise owners can enable access to public {% data variables.product.prodname_dotcom_the_website %} actions.'
redirect_from:
  - /enterprise/admin/github-actions/enabling-automatic-access-to-githubcom-actions-using-github-connect
  - /admin/github-actions/enabling-automatic-access-to-githubcom-actions-using-github-connect
  - /admin/github-actions/managing-access-to-actions-from-githubcom/enabling-automatic-access-to-githubcom-actions-using-github-connect
versions:
  ghes: '*'
type: how_to
topics:
  - Actions
  - Enterprise
  - GitHub Connect
shortTitle: Use GitHub Connect for actions
---

{% data reusables.actions.enterprise-github-hosted-runners %}

## About automatic access to {% data variables.product.prodname_dotcom_the_website %} actions

By default, {% data variables.product.prodname_actions %} workflows on {% data variables.product.prodname_ghe_server %} cannot use actions directly from {% data variables.product.prodname_dotcom_the_website %} or [{% data variables.product.prodname_marketplace %}](https://github.com/marketplace?type=actions). To make public actions from {% data variables.product.prodname_dotcom_the_website %} available on your enterprise instance, you can use {% data variables.product.prodname_github_connect %} to integrate {% data variables.product.prodname_ghe_server %} with {% data variables.product.prodname_ghe_cloud %}.

{% data reusables.actions.self-hosted-runner-networking-to-dotcom %}

Alternatively, if you want stricter control over which actions are allowed in your enterprise, you can manually download and sync public actions onto your enterprise instance using the `actions-sync` tool. For more information, see [AUTOTITLE](/admin/github-actions/managing-access-to-actions-from-githubcom/manually-syncing-actions-from-githubcom).

## About resolution for actions using {% data variables.product.prodname_github_connect %}

{% data reusables.actions.github-connect-resolution %}

If a user has already created an organization and repository in your enterprise that matches an organization and repository name on {% data variables.product.prodname_dotcom_the_website %}, the repository on your enterprise will be used instead of the {% data variables.product.prodname_dotcom_the_website %} repository. For more information, see [Automatic retirement of namespaces for actions accessed on {% data variables.product.prodname_dotcom_the_website %}](#automatic-retirement-of-namespaces-for-actions-accessed-on-githubcom).

## Enabling automatic access to public {% data variables.product.prodname_dotcom_the_website %} actions

Before enabling access to public actions from {% data variables.product.prodname_dotcom_the_website %} for your enterprise, you must:
* Configure {% data variables.location.product_location %} to use {% data variables.product.prodname_actions %}. For more information, see [AUTOTITLE](/admin/github-actions/getting-started-with-github-actions-for-your-enterprise/getting-started-with-github-actions-for-github-enterprise-server).
* Enable {% data variables.product.prodname_github_connect %}. For more information, see [AUTOTITLE](/admin/configuration/configuring-github-connect/managing-github-connect).

{% data reusables.enterprise-accounts.access-enterprise %}
{% data reusables.enterprise-accounts.github-connect-tab %}
1. Under "Users can utilize actions from GitHub.com in workflow runs", use the drop-down menu and select **Enabled**.
1. {% data reusables.actions.enterprise-limit-actions-use %}

## Automatic retirement of namespaces for actions accessed on {% data variables.product.prodname_dotcom_the_website %}

When you enable {% data variables.product.prodname_github_connect %}, users see no change in behavior for existing workflows because {% data variables.product.prodname_actions %} searches {% data variables.location.product_location %} for each action before falling back to {% data variables.product.prodname_dotcom_the_website %}. This ensures that any custom versions of actions your enterprise has created are used in preference to their counterparts on {% data variables.product.prodname_dotcom_the_website %}.

{% data reusables.actions.namespace-retirement-ghecom %}

After using an action from {% data variables.product.prodname_dotcom_the_website %}, if you want to create an action in {% data variables.location.product_location %} with the same name, first you need to make the namespace for that organization and repository available.

{% data reusables.enterprise_site_admin_settings.access-settings %}
1. In the left sidebar, under **Site admin** click **Retired namespaces**.
1. To the right of the namespace that you want use in {% data variables.location.product_location %}, click **Unretire**.
1. Go to the relevant organization and create a new repository.

   > [!TIP]
   > When you unretire a namespace, always create the new repository with that name as soon as possible. If a workflow calls the associated action on {% data variables.product.prodname_dotcom_the_website %} before you create the local repository, the namespace will be retired again. For actions used in workflows that run frequently, you may find that a namespace is retired again before you have time to create the local repository. In this case, you can temporarily disable the relevant workflows until you have created the new repository.
