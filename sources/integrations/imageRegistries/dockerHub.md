page_title: Image registries integrations
page_description: Setting up Shippable account integrations for Docker Hub Image registries
page_keywords: docker hub, amazon, ecs, gcr, google, shippable, quay, coreos, docker, registry, EC2 Container Service, Google, Docker Trusted registry, private

# Docker Hub integration

You will need Docker Hub integration if you want to do the following -

- Pull a private image  
- Build a Docker image which has a `FROM` that pulls a private image
- Push an image
- Use an [image resource](../../pipelines/resources/image/) as part of your CD [pipeline](../../pipelines/overview/)

##Add an account integration

* Ensure you have logged in to [Shippable](https://app.shippable.com).
* Go to your **Account Settings** by clicking on the gear menu in the top navbar.
* Click on **Integrations** in the sidebar menu.
* Click on **Add Integration**.
* Enter the following:
	* From the list of integration types, choose **Docker**
	* Add a friendly name for your integration
	* Enter your Docker Hub credentials
* Assign this integration to the Subscription(s) you want to use it in. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
* Click on **Save**. You should now see the integration in your list of integrations.

To learn how to configure your CI and CD workflows to use Docker Hub images, check out our [Docker Hub Tutorial](../../tutorials/ci/integrations/imageRegistries/usingDockerHub/).

---

##Deleting your Integration
To remove the Docker Hub integration, you'll need to remove this integration from all dependencies configured to use it.

###Deleting Docker Hub integration from a Project
To remove the Docker Hub integration from a project, simply remove the section shown below from the `shippable.yml` for that project.

```
integrations:
  hub:
    - integrationName: docker-hub-integration
      type: docker
      branches:
        only:
          - master
```

###Deleting Docker Hub integration from a Subscription
To delete a Docker Hub integration from a subscription, the steps are:

* On your Subscription page, click the **Settings** tab.
* Click on **Integrations** in the left sidebar menu.
* Click the `Delete` button next to the Docker Hub integration you want to delete.

<img src="/ci/images/integrations/imageRegistries/dockerHub/deleteInt.png" alt="Delete the Docker Hub integration from your Subscription" style="width:700px;"/>

###Deleting Docker Account Integration
To delete the Docker Account integration, all the dependencies, such as Subscriptions, using the Docker account must be deleted first. Do note that this action is irreversible. The steps to delete the Docker Account integration are:

* Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
* Select the Docker Hub integration from the list of integrations. If you have many entries, use the `Filters` dropdown and select `Docker`. Alternatively, you can use the `Integration Name` field to provide the name of your Docker Hub integration.
* Your Docker Hub integration shows up in the list.
* Click on the `Delete` button.
* A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any Subscription dependent on this integration.
* If there are dependencies, individually access the `Settings` tab for each Subscription and delete the Docker Hub integration.
* Once all dependencies of the Docker Hub integration have been removed, Step 5 will show the message: `No dependency`.
* Click the `Yes` button to delete the Docker Hub Integration.

--------
