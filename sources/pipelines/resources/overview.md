page_title: Unified Pipeline Resources
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

<br>
# Resources
Resources are the basic building blocks of your pipelines. They are inputs for and sometimes outputs from the executable units of your pipeline, aka [Jobs](../jobs/overview/).  

A key characteristic of resources is that they can be versioned and are immutable. A specific version of a resource is idempotent. i.e. it returns the same result every single time it is fetched. For example, git commit sha is always idempotent.

Shippable internally maintains its own version numbers for every resource so that different resources can be normalized internally. Docker images have tags as a way to version and git repos use commit shas. Both of these have a version number for every tag or sha making it easy to abstract it out.

There are several resources that are pre-defined as part of the platform and work as documented in the sections below:

- [gitRepo](gitRepo/): Source repository for your code
- [image](image/): Docker image definition
- [dockerOptions](dockerOptions/): Options for docker images
- [params](params/): Parameters for your apps/services/microservices
- [replicas](replicas/): Number of copies of the service to run
- [version](version/): Semantic versions
- [cluster](cluster/): Cluster that defines a container service
- [notification](notification/): Notifications for job success or failure
- [integration](integration/): Credentials for third party services
- [loadBalancer](loadBalancer/): AWS Classic and Application Load Balancers
- [time](time/): Trigger a job at a specific day and time

At this time, we do not support definition of custom resources. If you need a resource that is not listed above, send us an email at [support@shippable.com](mailto:support@shippable.com)

[Jobs](../jobs/overview/), [resources](../resources/overview/), and [triggers](../triggers/) together can be used to model any deployment pipeline, from the simple to highly complex.

---
## Adding resources
Resources are defined in a configuration file `shippable.resources.yml` present in a source control repository. Any repo can contain this file but only one of it can be used. If more than 1 resource files are present, the first one is used. This is done in order to reduce conflict due to the same resource being defined in multiple places.

To learn how to add this file and connect it to pipelines, [click here ](/tutorials/pipelines/howToAddSyncRepos/)

---
## Deleting resources

Since pipelines are all about dependencies and deployable units are flowing through these pipelines at all times, deleting a resource can significantly alter or irreversibly change the pipeline in unexpected ways. To avoid accidental deletion of resource(s) in ymls, we have made deletion of resources a 2 step process.

First, you need to soft-delete a resource by removing it from your `shippable.resources.yml` file. This removes it from the pipeline, but does not remove it from the database. You can see a list of soft-deleted resources at the bottom of the `Resources` tab. If soft-deleted resources are added back to the resources yml, the system will undelete them and you will retain version history for the undeleted resource.

To completely remove a resource from the system, you need to hard delete it through the UI. To do this:

* Go to your Subscription page and click on the `Pipelines` tab
* Click on the `Resources` pill
* You will find a list of soft deleted resources at the bottom of the page. To hard delete, just click the 	`Delete` button for the resource you want to delete

A resource must be soft deleted before it can be hard deleted.

---
## Viewing resources
You can view resources and version histories of resources by going to the Pipelines tab for your subscription and clicking on the `Resources` pill.

---
<a name="integration"></a>
## Integrations
Shippable is designed to separate out sensitive authentication information from your resources yml.

This is done to ensure there are no encryption/decryption or permissions issues when you move things around i.e. moving resource definitions from one repository to another, or if the person who created the pipeline is no longer the member of the team etc. Integrations are specified as a property in the YML definition for resources that connect to third party services.

To learn how to create integrations and use them in your ymls, [click here](../../integrations/overview/)
