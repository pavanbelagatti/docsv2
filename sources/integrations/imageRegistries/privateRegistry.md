page_title: Private Registry integration
page_description: Setting up Shippable account integrations for any private registries
page_keywords: docker hub, amazon, ecs, gcr, ecr, google, shippable, quay, coreos, docker, registry, EC2 Container Service, Google, Docker Trusted registry, private, elastic compute cloud

# Private Registry

##Adding the Account Integration

You will need to configure this integration in order to pull or push images from a private Image registry.

To set up this integration, you will need the URL of your private registry.

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
2. Click on the `Add Integration` button.
3. Choose `Private Docker Registry` from the list of integration types.
4. For 'Integration Name', use a distinctive name that's easy to associate to the integration and recall. Example: `my_private_registry`.
5. Enter the URL of your private registry and your credentials.
6. Assign this integration to the Subscription(s) you want to use it in. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
7. Click on `Save`.

The integration will now be available to all your continuous integration and deployment workflows.

<img src="/ci/images/integrations/imageRegistries/privateRegistry/addInt.png" alt="Private Registry integration" style="width:700px;"/>


Note: For fetching image tags we use token based authentication using Docker API V2.

This is used only in pipelines. So you need to have an auth-server setup for your private registry. For more information read the [official Docker's docs](https://docs.docker.com/registry/spec/auth/token/).

---

##Pull an image from a Private Docker Registry
You can pull any image you have access to, from Private Docker Registry and use that to spin up your CI build container.

To pull an image, you'll need to do the following:

1. Ensure your subscription has access to the Private Docker Registry integration.
2. Configure your `shippable.yml` to associate the Private Docker Registry integration for your project.

###Ensure your Subscription has access to the Private Docker Registry integration
To ensure your Subscription has access to the Private Docker Registry integration, do the following:

1. Log in to [Shippable](https://app.shippable.com).
2. Select your Subscription from the dropdown burger bar menu on the top left.
3. Click the 'Settings' tab and go to the 'Integrations' section.
4. If you find your integration in the list, you're good to go to the next step. If not, add the account integration to the Subscription by clicking on the `Add Integration` button and completing the required fields.

###Configure Private Docker Registry integration in the `shippable.yml`
To enable Private Docker Registry integration for your project, add the following to the `shippable.yml` file for that project.
```
pre_ci_boot:
    image_name: manishas/myImage
    image_tag: latest
    pull: true
    options: "-e HOME=/root"

integrations:
  hub:
    - integrationName: my_private_registry
      type: "private docker registry"
      branches:
        only:
          - master
          - dev
```
While the above is a sample code for your `shippable.yml`, use the descriptions of each field below to modify the `yml` and tailor it to your requirements.

- `image_name:` value is in the format docker-registry-username/docker-registry-image-repo.
- `image_tag:` value is the tag for the image that you want to pull.
- `pull:` set to `true` to pull the image from the specified Image Registry.
- In the `env` section, you can enter any environment variables you want to be set inside your CI container. Read more about [environment variables](/ci/advancedOptions/envVar/).
- In the `options` tag, enter any docker options you want to use in the docker run command. You also need to include the HOME environment variable as shown if it is not already set in your image.
- `integrationName` value is the name of the Private Docker Registry integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match). Moreover, this account should have permissions to pull the the build image specified in the `image_name` setting.
- `type` is `"private docker registry"`.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

For more information on pulling images, refer our documentation on [pulling an image](/ci/advancedOptions/images/).

---

##Build a Docker image which has a `FROM` that pulls an image from Private Docker Registry

If you want to build your Docker image as part of your workflow for each CI run and if your 'Dockerfile' has a `FROM` which pulls a private image from Private Docker Registry, then you will need to do the following steps:

1. Ensure your Subscription has access to the Private Docker Registry integration
2. Configure your `shippable.yml` to associate the Private Docker Registry integration for your project and add few options to ensure you are building the Docker image as part of CI.

###Ensure your Subscription has access to the Private Docker Registry integration
To ensure your Subscription has access to the Private Docker Registry integration, do the following:

1. Log in to [Shippable](https://app.shippable.com).
2. Select your Subscription from the dropdown burger bar menu on the top left.
3. Click the 'Settings' tab and go to the 'Integrations' section.
4. If you find your integration in the list, you're good to go to the next step. If not, add the account integration to the Subscription by clicking on the `Add Integration` button and completing the required fields.

###Configure Private Docker Registry integration in the `shippable.yml`

Add the following to your `shippable.yml` file:

```
build:
  pre_ci:
    - docker build -t myImage:tip .

  pre_ci_boot:
    image_name: myImage
    image_tag: tip
    pull: false
    options: "-e HOME=/root"

integrations:
  hub:
    - integrationName: my_private_registry
      type: "private docker registry"
      branches:
        only:
          - master
          - dev
```

- `image_name` value is the name of the image that was built in the `pre_ci` step.
- `image_tag` is the tag for the image that was built in the `pre_ci` step.
- set `pull` to `false` if you want to use the image you built during the pre_ci step instead of pulling from a docker registry.
- In the env section, you can enter any environment variables you want to be set inside your CI container.
- In the options tag, enter any docker options you want to use in the docker run command. You also need to include the HOME environment variable as shown if it is not already set in your 'Dockerfile'.
- `integrationName` value is the name of the Private Docker Registry integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match). Moreover, this account should have permissions to pull the the build image specified in the `image_name` setting.
- `type` is `"private docker registry"`.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

For more information on building images as part of the CI, refer our documentation on [building an image](/ci/advancedOptions/images/).

---

##Push an image to Private Docker Registry

You can push your image to Private Docker Registry in the `post_ci` or `push` sections of the `shippable.yml`. The main difference is that the `post_ci` section runs inside the build container and the `push` section runs outside the build container in the Shippable Agent.

To push an image to Private Docker Registry, do the following:

1. Ensure your Subscription has access to the Private Docker Registry integration
2. Configure your `shippable.yml` to associate the Private Docker Registry integration for your project and add few options to ensure you are pushing the Docker image in `post_ci` section or in the `push` section.

###Ensure your Subscription has access to the Private Docker Registry integration
To ensure your Subscription has access to the Private Docker Registry integration, do the following:

1. Log in to [Shippable](https://app.shippable.com).
2. Select your Subscription from the dropdown burger bar menu on the top left.
3. Click the 'Settings' tab and go to the 'Integrations' section.
4. If you find your integration in the list, you're good to go to the next step. If not, add the account integration to the Subscription by clicking on the `Add Integration` button and completing the required fields.


###Configure Private Docker Registry integration in the `shippable.yml`

To push the Docker image to Private Docker Registry in the `post_ci` section, add the following to your `shippable.yml` file:

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME manishas/sample-node:tag
    - docker push manishas/sample-node:tag

integrations:
  hub:
    - integrationName: my_private_registry
      type: "private docker registry"
      branches:
        only:
          - master
```

Similarly to push the Docker image to Private Docker Registry in the `push` section, add the following to your `shippable.yml` file:

```
build:
  post_ci:

  push:
    docker push manishas/sample-node:tag

integrations:
  hub:
    - integrationName: my_private_registry
      type: "private docker registry"
      branches:
        only:
          - master
```


- `integrationName` value is the name of the Private Docker Registry integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match). Moreover, this account should have permissions to pull the the build image specified in the `image_name` setting.
- `type` is `"private docker registry"`.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

For more information on pushing images as part of the CI, refer our documentation on [pushing an image](/ci/advancedOptions/images/).

---

##Deleting the Integration

To remove the Private Registry integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the Private Registry integration from the list of integrations. If you have many entries, use the `Filters` dropdown and select `Docker`. Alternatively, you can use the `Integration Name` field to provide the name of your Private Registry integration.
3. Your Private Registry integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any project, environment or subscription image dependent on this integration.
6. If there are dependencies, individually access the `Settings` tab for each project/environment/subscription image and delete the Private Registry integration.
7. Once all dependencies of the Private Registry integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the `Yes` button to delete the Private Registry Integration.

--------
