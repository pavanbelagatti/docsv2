page_title: What is supported?
page_description: Supported languages, services & platforms
page_keywords: containers, languages, services, platforms, lxc, docker, Continuous Integration, Continuous Deployment, CI/CD, testing, automation

# What is an integration?

Integrations are used to connect your Shippable CI or CD workflows to third party platforms and services. We believe in separating authentication credentials from your yml definitions for better security and privacy. All integrations are stored in our <a href="https://www.vaultproject.io/" target="_blank">Vault store</a> for maximum security.

Using an integration is a two step process:

<img src="../images/accountIntegrationLifecycle.png" alt="Google Container Registry integration" style="width:800px;"/>

#### Adding an integration to your account
You can get to your account integrations by clicking on the gear icon in the top navbar and clicking on `Integrations` in the left sidebar menu. Click on `Add integration` to add a new integration. Instructions for each type of integration are provided in the [section below](#listIntegrations). Please note that while creating the integration, you will need to specify which Subscriptions have access to it. This is a very important step and if you miss it, you will not be able to use it in your ymls.

#### Using the integration in your yml
Once an integration is enabled for a subscription, you can use it in any project in that subscription with a few lines of yml configuration. Please note that the integration name should be the one configured in your integration.

For more details on how to use each type of integration, click on the integration you need from the list below.

---
<a name="listIntegrations"></a>
##List of available integrations

### Source control providers
You need a source control integration if:

- You want to build repositories on Bitbucket Server, Gitlab, or GitHub Enterprise using Shippable Hosted
- You set up continuous deployment using [Pipelines](../../pipelines/gettingStarted/#sync)

We currently support the following source control providers:

-  [GitHub](scm/github/)
-  [GitHub Enterprise](scm/githubEnterprise/)
-  [Bitbucket](scm/bitbucket/)
-  [Bitbucket Server](scm/bitbucketServer/)
-  [GitLab](scm/gitlab/)

---

### Notification Providers

You need a notification integration if you want to send notifications for your [CI](../../ci/overview/) or [Pipeline](../../pipelines/overview/) workflows.

- [Slack](notifications/slack/)
- [IRC](notifications/irc/)
- [Email](notifications/email/)
- [HipChat](notifications/hipchat/)
- [Event triggers](notifications/webhooks/)

---

### Docker registries
You will need Docker registry integration if you want to do the following -

- Pull a private image  
- Build a Docker image which has a `FROM` that pulls a private image
- Push an image
- Use an [image resource](../../pipelines/resources/image/) as part of your CD [pipeline](../../pipelines/overview/)

We support the following Docker registries:

- [Docker Hub](imageRegistries/dockerHub/)
- [Amazon EC2 Container Registry (ECR)](imageRegistries/ecr/)
- [Google Container Registry](imageRegistries/gcr/)
- [Quay](imageRegistries/quay/)
- [Docker Trusted Registry](imageRegistries/dockerTrustedRegistry/)
- [Any private registry](imageRegistries/privateRegistry/)

---

### Container Services

You need a Container Service integration if you want to push your application to a container service as part of your CD workflow using [Pipelines](../pipelines/overview/).

We support the following Container Services:

- [Amazon EC2 Container Service (ECS)](containerServices/ecs/)
- [Google Container Engine (GKE)](containerServices/gke/)
- [Docker Cloud](containerServices/dockerCloud/)
- [Docker Datacenter](containerServices/dockerDatacenter/)

---

### Deploy

You can push to Amazon elastic beanstalk as part of your CI workflow by adding the integration:

- [Amazon Elastic Beanstalk](deploy/eb/)

---
### Keys

You need a key integration if you want to integrate with a third party service that is not natively supported with Shippable. You should use these integrations so that you don't have to expose your keys in your CI yml configuration:

- [PEM](keys/pem/)
- [SSH](keys/ssh/)

---
