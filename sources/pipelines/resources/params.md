page_title: Unified Pipeline Resources
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# params

A `params` resource type is used to add a list of environment parameters that are
appended to an application or microservice. This resource is used as an input to [manifest jobs](../jobs/manifest/) or [deploy jobs](../jobs/deploy/).

You can create this resource by adding it to `shippable.resources.yml`
```
resources:
  - name: <string>                          	#required
    type: params                              	#required
    version:
      params:                                 
        key1: "value1"                   		#required atleast 1
        key2: "value2"                     	#optional
        secure: <encrypted value>  			#optional
```

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view and the list of resources in the Pipelines `Resources` tab. It is also used to refer to this resource in the jobs yml. If you have spaces in your name, you'll need to surround the value with quotes, however, as a best practice we recommend not including spaces in your names.

* `type` is always set to 'params'.

* The version -> params section includes the key value pairs that are set as environment variables when the application or service starts in the target environment.
	* Include at least one key value pair under params
	* You can use secure variables to encrypt any key value pairs that contain sensitive information you don't want to include as plain text. To encrypt one or more key value pairs, [follow the instructions in the Subscription Settings guide](../../navigatingUI/subscriptions/settings.md#encrypt). Copy the encrypted value and include it in your resource file as shown in the snippet above.

A new version of this resource is created every time anything in the version section changes.  


##Overriding params
`params` can also be used to override settings that were set in an upstream stage of the pipeline.

For example, if you want to use different environment parameters (say database settings) in Test and Production environments, you can do so by overriding the resource.

<img src="../../images/resources/overrideParams.png" alt="Overriding docker options" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

In the picture above, `deploy-test` takes `params-1` as an input. After testing, a release is created with the `release` job. This triggers production deployment with the `deploy-prod` job, which takes `params-2` as an input. For this production deployment, we will use a superset of settings from `params-1` and `params-2`, with values for any common settings being chosen from `params-2`.
