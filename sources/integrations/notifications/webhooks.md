page_title: Event Triggers based on user specified Webhook Integration
page_description: Setting up Event Triggers based on user specified Webhook integrations on Shippable
page_keywords: eb, amazon, aws, shippable, google, openshift

#Event Triggers based on user specified Webhooks
Shippable supports triggers on user-specified webhook URLs or other enabled projects.

By default, configured triggers are hit only if a commit build succeeds.

You can configure the integration in the `shippable.yml` according to your scenarios.

This can be done by following the steps below:

1. Adding the Account Integration (One time only per account)
2. Configure the Event Trigger Integration in the `shippable.yml` file for your project

## Adding an Account Integration

You can add the integration for a Project or for a Generic Webhook. The steps to do so are:

###Adding the Account Integration for a Project
1. Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
2. Click on the `Add Integration` button.
3. For 'Integration type', choose `Event Triggers` from the list of dropdown choices.
4. For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `Event-Trigger-Integration`.
5. Under 'What would you like to trigger?' select `Project` from the dropdown choices.
6. [Optional] Select the Project that you want to associate the integration. In the screen shot below, `sample_nodejs` has been selected.
7. In the 'Authorization' field, you'll need to specify the [Shippable API Token](/NavigatingUI/accountSettings/api/).
8. Assign this integration to the Subscription(s) you want to use it in. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
9. Click on `Save`.

<img src="/ci/images/integrations/deploy/webhooks/addTriggerProject.png" alt="Event Trigger integration" style="width:700px;"/>

The integration will now be available to all your Continuous Integration and Pipelines settings within the Shippable portal.

###Adding the Account Integration for a Generic Webhook

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
2. Click on the `Add Integration` button.
3. For 'Integration type', choose `Event Triggers` from the list of dropdown choices.
4. For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `Event-Trigger-Integration`.
5. Under 'What would you like to trigger?' select `Generic Webhook` from the dropdown choices.
6. Specify a webhook URL you want to trigger in the 'WebhookURL' field.
7. In the Authorization field, you need to specify the HTTP Authorization Header required to hit the specified webhook URL. If no authorization is required, this field can be left blank.
8. Assign this integration to the Subscription(s) you want to use it in. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
9. Click on `Save`.

<img src="/ci/images/integrations/deploy/webhooks/addTriggerWebhook.png" alt="Event Trigger integration" style="width:700px;"/>

The integration will now be available to all your Continuous Integration and Pipelines settings within the Shippable portal.

---

##Configure the Integration

You will need the following format to add the integration to your shippable.yml:

```
integrations:
  notifications:
    - integrationName: my_webhook_integration
      type: webhook
      payload:
        - isPRBuild=$IS_PULL_REQUEST
        - branchName=$BRANCH
        - message=Shippable Run $BUILD_NUMBER ($BUILD_URL) succeeded for $COMPARE_URL
        - FOO=BAR
        - FIZZ=BUZZ
      branches:
        only:
          - master
          - dev
      on_success: always
      on_failure: never
      on_pull_request: never
      on_start: never
```

- `integrationName` value is the name of the Webhook integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match).
- `type` is `webhook`.
- [optional] `payload` You can specify `key=value` pairs where the `value` is a string. The string can contain Shippable [Environment Variables](/ci/advancedOptions/environmentVariables/). These variables will be populated by corresponding values from a run for this project.
- If the `webhook` integration is set up **to trigger an enabled project**: the `payload` will be injected into the next run as a set of global environment variables ([Injecting Global Env Variables](/ci/advancedOptions/environmentVariables/#injecting-global-env-variables)).
- If the `webhook` integration is set up **to trigger an external webhook**: the `payload` will be converted to a JSON with `key: value` attributes which will be sent as the body when the specified webhook URL is triggered.
- [optional] `branches` allows you to choose the branches you want to trigger the configured webhook for. By default, the configured webhook is triggered for all branches. The `only` tag should be used when you want to trigger webhooks for specific branches. You can also use the `except` tag to exclude specific branches.
- [optional] You can set the following options for the `on_success`, `on_failure` tags :
    - `change` for `on_success` or `on_failure` means you will receive notifications only when the build status changes to success or failure respectively
    - `always` means that you will always receive a notification for that build status
    - `never` means that you will never receive a notification for that build status
    - **By default, `on_success` is set to `always` and `on_failure` is set to `never`** if the integration is configured in the yml but you do not specify these tags.
- [optional] You can set the following options for the `on_start`, `on_pull_request` tags :
    - `always` means that you will always receive a notification for build start/pull request
    - `never` means that you will never receive a notification for that build start/pull request
    - **By default, `on_start` is set to `never` and `on_pull_request` is also set to `never`** if `webhook` is configured in the yml but you do not specify these tags.

Check out our blogs  on practical examples of [Triggering a custom webhook after CI](http://blog.shippable.com/triggering-a-custom-webhook-after-continuous-integration) and [Triggering a parameterized build after CI](http://blog.shippable.com/triggering-a-parameterized-build-after-continuous-integration) to learn more about Event Triggers.

---

##Deleting integration from a project

To remove an Event Trigger integration from a project, remove the following section from the `shippable.yml` file:

```
integrations:
  notifications:
    - integrationName: my_webhook_integration
      type: webhook
      payload:
        - isPRBuild=$IS_PULL_REQUEST
        - branchName=$BRANCH
        - message=Shippable Run $BUILD_NUMBER ($BUILD_URL) succeeded for $COMPARE_URL
        - FOO=BAR
        - FIZZ=BUZZ
      branches:
        only:
          - master
          - dev
      on_success: always
      on_failure: never
      on_pull_request: never
      on_start: never
```

The integration is now deleted for the project.

---

##Deleting integration from your Subscription

1. Ensure you have logged in to [Shippable](https://app.shippable.com).
2. Select your Subscription from the dropdown burger bar menu on the top left.
3. Click the 'Settings' tab and go to the 'Integrations' section.
4. Your Event Trigger integration shows up in the list.
5. Click on the `Delete` button.
6. A window pops up confirming that you want to delete the integration.
7. Click the `Yes` button to delete the Event Trigger Account Integration for the subscription.

The integration is now deleted for all your Continuous Integration and Pipelines settings in the Subscription.

---

##Deleting Account Integration

To remove the Event Trigger Account integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the Event Trigger Account integration from the list of integrations. If you have many entries, use the `Filters` dropdown and select `webhook`. Alternatively, you can use the `Integration Name` field to provide the name of your Event Trigger integration.
3. Your Event Trigger integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any project, environment or subscription image dependent on this integration.
6. If there are dependencies, individually access the `Settings` tab for each project/environment/subscription image and delete the Event Trigger integration.
7. Once all dependencies of the Event Trigger integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the `Yes` button to delete the Event Trigger Account Integration.

--------
