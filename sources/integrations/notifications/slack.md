page_title: Integrating Slack on the Shippable platform
page_description: Configure Slack to send out Slack notifications for Continuous Integration/Continuous Delivery actions
page_keywords: Integration, Integrate, Slack, Notify, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, email, HipChat, IRC

#Slack

You can send Slack notifications for any status changes for CI, pipelines, and monitored Docker images.

To send these notifications to Slack channel(s), you will first need to add an integration to your Account Integrations.

---
<a name="addSlackToAccount"></a>
##Adding a Slack integration

There are two actions to set up the Slack integration through the UI. They are:

- Configure an incoming webhook on Slack
- Add the Slack integration to your subscription

#####Configure an incoming webhook on Slack

* Sign in to your Slack account and [go to this link to create an incoming webhook](https://my.slack.com/services/new/incoming-webhook/).
     - Provide credentials to your Slack account, if prompted.
* In the `Post Channel` section, use the dropdown and select a channel to create the webhook. It does not matter which channel you choose while creating the webhook. We will override it when you configure the integration on Shippable.
* Click the 'Add Incoming Webhook integration' button.
* Copy the text in the 'Webhook URL' section. It looks like this: `https://hooks.slack.com/services/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`.
* Click the `Save Settings` button.

#####Add the Slack integration to your Account
* Go to your **Account Settings** by clicking on the gear menu in the top navbar.
* Click on **Integrations** in the sidebar menu.
* Click on **Add Integration** and choose **Slack** from the list of available integrations
* Enter the following:
	* Add a friendly name for your integration
	* In the 'Webhook URL' field, paste the Slack webhook URL
* Assign this integration to the Subscription(s) you want to use it in. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
* Click on **Save**. You should now see the integration in your list of integrations.


---

##CI notifications

To use your Slack integration for your CI workflows, follow the steps below:

* First, add the [Slack integration to your Account](#addSlackToAccount)
* Select your Subscription from the top left menu dropdown on your dashboard. This should be the Subscription containing the project you want to send Slack notifications for.
* Go to the **Settings** tab and click on **Integrations** in the sidebar menu.
* If you find your integration in the list, you're good to go to the next step. If not, add the account integration to the Subscription by clicking on the `Add Integration` button and completing the required fields.

* Update the `shippable.yml` file for the project for which you want to send Slack notifications:

```
integrations:
  notifications:
    - integrationName: slack-integration
      type: slack
      recipients:
        - "#channelOne"
        - "#channelTwo"
      branches:
        only:
          - master
          - dev
      on_success: never
      on_failure: always
```
Use the descriptions of each field below to modify the `yml` and tailor it to your requirements.

- `integrationName` value is the name of the Slack integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with error as [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match).
- `type` is `slack`.
- `recipients` specifies the channels you want to send the notification to. Please note this is a required field for slack notifications to work.
     - Even if there is a single recipient, use the format `recipients: "#channelOne"`
- [optional] `branches` allows you to choose the branches you want to send notifications for. By default, notifications are sent for all branches. The `only` tag should be used when you want to send notifications to specific branches. You can also use the `except` tag to exclude specific branches.
- [optional] You can set the following options for the `on_success`, `on_failure` tags:
     - The value `change` for `on_success` or `on_failure` fields means you will receive notifications only when the build status changes to success or failure respectively.
     - `always` means that you will always receive a notification for that build status.
     - `never` means that you will never receive a notification for that build status.
     - By default, `on_success` is set to `change` and `on_failure` is set to `always` when Slack is configured in the yml and you have not specified these tags.
- [optional] You can set the following options for the `on_start`, `on_pull_request` tags:
     - Setting the value to `always` means that you will always receive a notification for build start/pull request.
     - Setting the value to `never` means that you will never receive a notification for that build start/pull request.
     - By default, `on_start` is set to `never` and `on_pull_request` is set to `always` when Slack is configured in the yml and you have not specified these tags.


Check our blog ["Notifying CI failure/success status on Email and Slack"](http://blog.shippable.com/notifying-ci-failure/success-status-on-email-slack) for multiple scenarios.


---

##Pipeline notifications

To use your Slack integration for your Pipeline workflows, follow the steps below:

* First, add the [Slack integration to your Account](#addSlackToAccount)
* Select your Subscription from the top left menu dropdown on your dashboard. This should be the Subscription containing the project you want to send Slack notifications for.
* Go to the **Settings** tab and click on **Integrations** in the sidebar menu.
* If you find your integration in the list, you're good to go to the next step. If not, add the account integration to the Subscription by clicking on the `Add Integration` button and completing the required fields.

Pipeline notifications are sent by defining the [notification resource](../../pipelines/resources/notification/) in your `shippable.resources.yml` and then using it in your `shippable.jobs.yml`. More on this is explained in our [Jobs documentation](../../pipelines/jobs/overview/#jobNotifications).

---

##Deleting Slack integrations

To remove the Slack integration, you'll need to remove this integration from all dependencies configured to use it.

###Deleting Slack integration from a Project
To remove the Slack integration from a project, simply remove the section shown below from the `shippable.yml` for that project.

```
integrations:
  notifications:
    - integrationName: slack-integration
      type: slack
      recipients:
        - "#channelOne"
        - "#channelTwo"
      branches:
        only:
          - master
          - dev
      on_success: never
      on_failure: always
```

###Deleting Slack integration from a Subscription
To delete a Slack integration from a subscription, the steps are:

1. Ensure you have logged in to [Shippable](https://app.shippable.com).
2. Select your Subscription from the dropdown burger bar menu on the top left.
3. Click the 'Settings' tab and go to the 'Integrations' section.
4. Review the list of integrations for your subscription.
5. Click the `Delete` button next to the Slack integration.

<img src="/ci/images/integrations/notifications/slack/deleteInt.png" alt="Delete the Slack integration from your Subscription" style="width:700px;"/>

###Deleting Slack Account Integration
To delete the Slack Account integration, all the dependencies, such as Subscriptions, using the Slack account must be deleted first.

Do note that this action is irreversible.

The steps to delete the Slack Account integration are:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the Slack integration from the list of integrations. If you have many entries, use the `Filters` dropdown and select `Slack`. Alternatively, you can use the `Integration Name` field to provide the name of your Slack integration.
3. Your Slack integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any project, environment or subscription image dependent on this integration.
6. If there are dependencies, individually access the `Settings` tab for each project/environment/subscription image and delete the Slack integration.
7. Once all dependencies of the Slack integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the `Yes` button to delete the Slack Integration.

--------
