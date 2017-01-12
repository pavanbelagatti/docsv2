page_title: Google Container Registry (GCR) integration
page_description: Setting up Shippable account integrations for Google GCR
page_keywords: docker hub, amazon, AWS, ecs, gcr, ecr, google, shippable, quay, coreos, docker, registry, EC2 Container Service, Google, Docker Trusted registry, private, elastic compute cloud

# Google Container Registry (GCR)

##Adding the Account Integration

You will need to configure this integration to pull private images from or push images to Google Container Registry.

On the [Google Developers Console (GDC)](https://console.developers.google.com/):

1. In the top navigation bar, select the project you want to integrate with Shippable.
2. Click on the `Products and Services` menu on the top left and select 'Permissions'.
3. Select 'Service Accounts' tab and click on `Create service account`.
4. Add Service name and ID and then select `Furnish a new private key` and check `JSON`. Click on `Create`. A dialog box appears to save the keys.
5. Your new Public/Private key pair is generated and downloaded to your machine. Please store this carefully since you will not be able to retrieve this from your GDC account.

(For more on JSON keys and Service accounts, read [Google's docs](https://cloud.google.com/container-registry/docs/auth#using_a_json_key_file))


On your [Shippable dashboard](https://shippable.com):

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Click on the `Add Integration` button.
3. Choose `GCR` from the list of integration types.
4. For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `gcr_integration`
5. Enter your JSON key that you saved earlier
6. Assign this integration to the Subscription(s) you want to use it in. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
7. Click on `Save`

<img src="/ci/images/integrations/imageRegistries/gcr/addInt.png" alt="Google Container Registry integration" style="width:700px;"/>

The integration will now be available to all your continuous integration and deployment workflows. For a comprehensive tutorial on how to configure your workflows with GCR, read our [Using GCR tutorial](../../tutorials/ci/integrations/imageRegistries/usingGCR/)

---

##Deleting the Integration

To remove the GCR integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the GCR integration from the list of integrations. If you have many entries, use the `Filters` dropdown and select `GCR`. Alternatively, you can use the `Integration Name` field to provide the name of your GCR integration.
3. Your GCR integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any project, environment or subscription image dependent on this integration.
6. If there are dependencies, individually access the `Settings` tab for each project/environment/subscription image and delete the GCR integration.
7. Once all dependencies of the GCR integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the `Yes` button to delete the GCR Integration.

---
