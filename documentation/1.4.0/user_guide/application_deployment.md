---
layout: post
title:  Application deployment
root: ../../
categories: DOCUMENTATION-1.4.0
parent: [user_guide, application_management]
node_name: application_deployment
weight: 100
---

{% summary %}{% endsummary %}

To deploy an application you need to setup the context, you can do this in the `Applications > Deployments` sub-menu.  
The deployment setup is done by going through a number of hierarchical steps.
Each step perform a validation of the deployment topology, and errors details are displayed. Note that you can not go to the next step  when the current one is still not valid.

# Location selection
 The first thing to do is to select, among the displayed location, the one on which you would like to deploy.
 The proposed locations are determined by matching every existing location against the topology, done by a matcher plugin.  

[![Configure your deployment](../../images/user_guide/application/deployment/user_guide_deployment_setup.png)](../../images/user_guide/application/deployment/user_guide_deployment_setup.png)

 For now, note that if no matching plugin is configured by the administrator, a default matcher is used, checking the following:

- The orchestrator managing a location can support all the artifacts contains in your topology (nodes and relationships implementations scripts)
- The current user have sufficient rights to deploy on the location

# Node substitution :

Next step is to substitute some abstract nodes from your topology with resources provided by the selected location.  
In the meantime, you can edit some properties if you need to.

[![Node substitution](../../images/user_guide/application/deployment/user_guide_deployment_setup_substitution.png)](../../images/user_guide/application/deployment//user_guide_deployment_setup_substitution.png)


# Inputs: properties and artifacts

Here you set values to inputs and provider deployments properties. Also, you can upload artifacts set as input in the topology.

Here you need to select the value for your `inputs`. If they are some missing configuration in your topology, a `todo list` will be displayed.

[![Deployment inputs](../../images/user_guide/application/deployment/user_guide_deployment_setup_inputs.png)](../../images/user_guide/application/deployment/user_guide_deployment_setup_inputs.png)

Once all those steps are valid, the *deploy* step is unlocked, and if your topology is valid and ready for deployment, you can hit the deploy button to proceed.  
You can now see what is happening on the [runtime view](#/documentation/1.4.0/user_guide/application_runtime.html).

# Update

Once an application has been successfully deployed, you can upgrade it. Upgrading a deployment means adding/removing/changing nodes and/or relationships in a deployed topology.

This can be done :

- in an incremental development mode: your application has been deployed, you add / remove some nodes in your topology, then you can update the deployment in order to deploy your changes.
- between versions: you have already deployed a V1 of your application in production. You have worked on a V2 and have successfully tested it. You want to push the delta in production environment, you can use the upgrade feature to deploy the V2 in your production environment (instead of undeploying V1 then deploying V2).

Since this feature strongly depends on underlying orchestrator, you should refer to the dedicated documentation portion of the orchestrator you are using to know more about this feature ([here](#/documentation/1.4.0/orchestrators/cloudify3_driver/deployment_update.html) for Cloudify orchestrator).
