page_title: Shippable CI/CD Overview
page_description: Explanation of the CI/CD OVERVIEW
page_keywords: ci/cd dashboard, subscription settings, CI/CD, shippable CI/CD, documentation, shippable, config, yml

# Continuous Integration overview

Shippable's Continuous Integration feature helps you automate unit testing, packaging, and deployment for any change in your source control repository.

Continuous Integration is an essential first part of a continuous delivery workflow and helps software teams ensure that their changes are built and tested with the latest version of the entire codebase. As a result, most bugs are found almost immediately after the code change is committed, leading to better quality since each bug can be easily isolated to a specific code change and fixed promptly.   

For more on Continuous Integration and why you should include it as part of your workflow, read Martin Fowler's article on the [Benefits of Continuous Integration](http://martinfowler.com/articles/continuousIntegration.html#BenefitsOfContinuousIntegration)

For a complete integrated platform that automated the end to end Continuous Deployment workflow, check out [Shippable's CD pipelines](../pipelines/overview/).

---  

##Terminology

###Account

You do not need to explicitly create an account on Shippable to start using it. However, since we allow you to connect multiple source control providers and clouds to Shippable, the term 'account' is used to emcompass all of these identities. So for example, 'sync' at an account level means syncing your information across all source control providers and connected third party services.

###Subscription
A subscription on Shippable corresponds to an organization or personal account in your source control provider. So if you sign in to Shippable with GitHub credentials and your username is abcfoo and you're a member of orgs org1foo and org2foo, you will have 3 subscriptions on Shippable.

Billing is handled per subscription.

###Projects
A project on Shippable corresponds to a repository on your source control provider. As with subscriptions, project permissions are also synced with your source control provider.

###Minions
Minions are the build machines that are spun up to run your builds on Shippable Hosted. They are also called build machines or build containers at some places in the documentation.

---

##Signing in to Shippable

You do not need to explicitly create an account on the Hosted version of Shippable to start using it. You can sign in using your GitHub or Bitbucket credentials. We use OAuth authentication so you will need to authorize Shippable the first time you sign in. We sync all organizations from your source control, so if you click on the <i class="fa fa-bars" aria-hidden="true"></i> menu at the top left of your screen, you will see a list of your organizations (aka subscriptions on Shippable). You can then click into any organization to [enable projects](../navigatingUI/ci/subscriptions/ci/).


---
<a name="ciWorkflow"></a>
##CI workflow

The picture below shows a very basic CI workflow. Shippable receives an incoming webhook from your source control and spins up a build machine. A Shippable agent comes up on the build machine and starts the build container, inside which your CI commands are executed. At the end of the build process, you can push to any endpoint. This endpoint can be an artifact repository like Artifactory or Docker Hub, or can be a PaaS/IaaS/Container Service endpoint.

<img src="../images/ciWorkflow.png" alt="Continuous Integration workflow" style="width:1000px;"/>

Your CI workflow stops after deployment to an endpoint. If you want to define your end to end application delivery pipelines, check out our [Unified Pipelines section](../pipelines/overview/).

---

##Configuration
Your CI workflow is configured with a yml based file called **shippable.yml** which should be committed to the root of the repository you want to build. This is mandatory for all enabled projects and tells us what the build should do. For the yml structure and how to configure it, check out our [Build Configuration page](shippableyml/).

---
##CI Triggers

When a repository is enabled on Shippable, we enable webhooks on that repository and start listening to commit and pull request events.

Shippable automatically builds and tests your repositories when the following triggers are received-

* commit webhook
* webhook for a pull request opened for an enabled repository
* webhook for a git tag push event (GitHub only, turned off by default)
* webhook for a release event (GitHub only, turned off by default)

You can also initiate manual builds through the UI, by clicking on the Build button for any project or branch, irrespective of a webhook event.

To learn how to switch some of these triggers off, read about the configuration in [Subscription settings](../navigatingUI/subscriptions/settings/).

---

##Build flow

When a build is triggered, it is executed in the sequence below -

* First, we provision a build machine for you. Build machines have 2 cores, 3.75 GB RAM each. Provisioning can take anytime between 10 seconds to 4 mins, depending on whether your subscription already has a build machine running from an earlier build.
* Once the machine is available, the Shippable agent starts running on that machine.
* Commands in the `pre_ci` section are then executed on your build machine by the Shippable Agent.
* The next step is booting your build container. This will use our default Docker images if nothing is configured in the `pre_ci_boot` section of your yml. If that section is configured, it overrides the default image and boots up the build container specified.
* Next, we set the environment in the build container and clone your repository that is to be built.
* All commands in the `ci` section are executed in sequence inside the build container.
* Commands in the `post_ci` section are executed inside the build container.
* If the `ci` and `post_ci` sections were successful, commands in the `on_success` section are executed.
* If the `ci` and/or `post_ci` sections failed, commands in the `on_failure` section are execured.
* If notifications are configured in the yml, we will send out notifications about build results through the configured channel. Email notifications are on by default.

---

##Enabling GitHub private repositories

To enable GitHub integration for public repositories:  

- Log in to [Shippable](https://app.shippable.com) using your GitHub credentials.
- Click on `Authorize application` button to authorize Shippable to access your public repositories on GitHub (This is a one-time step). Provide your GitHub password, if prompted.
- Your subscription using your GitHub account is ready to use. In your 'Subscription', in the 'CI' tab, click the 'Enable Project' section to view all your public repositories in GitHub. Proceed to [enabling a project](/navigatingUI/subscriptions/ci/#enable-project).
- If you don't see your projects in the above step, click on the Account settings (gear icon on the top right hand navigation bar). In the 'Accounts' section click the `Sync` button.
- For private repositories, you need to one-time authorize Shippable to access them. You can do this by following the outlined steps:
- Ensure you have logged in to [Shippable](https://app.shippable.com) using your GitHub credentials.
- Click on the Account settings (gear icon on the top right hand navigation bar).
- In the 'Accounts' section and under 'Git Identities', click 'Enable' under 'GitHub'.
- Click `Authorize application` in the next page to enable access to private repositories.

<img src="/ci/images/integrations/scm/github/enablePvtRepoMv.gif" alt="Enable access to GitHub Private Repositories" style="width:700px;"/>
