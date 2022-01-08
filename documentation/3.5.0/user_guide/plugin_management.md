---
layout: post
title:  Plugin(s) management
root: ../../
categories: DOCUMENTATION-3.5.0
parent: [user_guide, admin]
node_name: plugin_management
weight: 100
---

{% summary %}{% endsummary %}

Plugins allows to provide some additional functionalities to Alien 4 Cloud. Users can create different type of plugins (or plugins with multiple features):

* __Orchestrator plugins__ allows to provide additional orchestrators support to alien 4 cloud to use some other technologies to deploy TOSCA or Specific topologies.
* __Location matching plugins__ allows to override the basic location matching logic provided within Alien 4 Cloud. Only a single Location Matching plugin can be defined in Alien 4 Cloud currently. If more than one location matching plugin is enabled in Alien then one will be picked up randomly.
* __Node matching plugins__ allows to override the basic TOSCA node matching logic within Alien 4 Cloud.
* __Generic extension plugins__ can provide additional UI screens and REST Services allowing any kind of extension to alien 4 cloud and even to override some of the alien UI components (it is not possible to override native rest services).

{% info %}
If you want to create new plugins for Alien 4 Cloud please refer to the [developer guide](#/developer_guide/index.html).
{% endinfo %}

# Installing plugin in Alien 4 Cloud

{%inittab%}
{% tabcontent Drag and Drop enabled %}

*Drag* you archive file > *Drop* it on the **dash dotted** area

![Upload an archive file with D&D](../../images/3.4.0/user_guide/admin/plugin_management.png)
{%endtabcontent%}
{% tabcontent Drag and Drop disabled %}

Click on *[Upload plugin]* > *Select* your archive (The file is automatically uploaded)

![Upload an archive file without D&D](../../images/user_guide/upload-plugin-wihout-drag-and-drop.png)
{%endtabcontent%}
{%endinittab%}

{% warning %}
After installing, removing, disabling or enabling a plugin that provides UI components user must refresh it's browser page in order to reload plugin's javascript code that may have changed.
This is especially true when removing or disabling a plugin as the rest services used by the plugin's UI won't be available anymore eventually causing unexpected 500 errors.
{% endwarning %}

# Plugin configuration

Some plugins may requires specific configuration that is global to the plugin. In case a plugin can be configured you will see the following icon : ![configure plugin](../../images/3.4.0/user_guide/admin/plugin_global_configuration.png){: .inline}

## Advanced plugins configurations

The configuration detailed in the previous section is global for the plugin. Some plugins may requires some specific configurations that you can find at other places in the application. Your should refer to the plugin specific documentation to know more about it.

For example, PaaS providers plugins actually are able to manage multiple instances of orchestrators, the specific configuration for each instance is managed at the cloud level.

# Plugin update

Due to historical management of orchestrator plugins alien4cloud allowed, before 1.3.1, multiple versions of the same plugin to be concurrently loaded and enabled.

Starting from version 1.3.1 this behavior is not allowed anymore and a single version of a given plugin can exists at a given point of time. This avoid a lot of potential conflicts especially on the UI side.
In previous version of alien4cloud a migration tool was provided to ensure plugin version update, starting from 1.3.1 plugins will be automatically updated when the alien4cloud server restarts based on plugins existing in the initialization folder.

Update process:
 - Stop your alien4cloud server
 - Remove old plugin(s) archive(s) from the /init/plugins folder and add new one(s)
 - Restart alien4cloud

Alien 4 cloud on startup will automatically update the plugins versions inside alien4cloud and load the new plugin version.

{% warning %}
<h5>Model update</h5>
This auto-update does not perform any model update. If your plugin model has changed you should either provide a migration tool to update data or have a built-in migration process upon plugin initialization.
{% endwarning %}

{% warning %}
<h5>Hot update</h5>
We don't support hot-plugin updates currently. This is a choice we made as unloading a plugin may cause interruption of some active processing from the plugin (including ongoing deployment/un-deployment).
This behavior will however be improved in next versions and plugins will be responsible of their shutdown management before a plugin is disabled.
{% endwarning %}


# Plugins list

This table lists the currenty supported and used plugins :

{: .table .table-striped }
| Plugin| Description|  Since version| 
|:---------|:------------|:---------|
| [alien-maven-repository-plugin](https://github.com/alien4cloud/alien4cloud-premium-repository-plugins/) | Maven Artifact Resolver Plugin| 1.3.0 | 
| [alien-git-repository-plugin](https://github.com/alien4cloud/alien4cloud-premium-repository-plugins/) | Git Artifact Resolver Plugin| 1.3.0 | 
| [alien-http-repository-plugin](https://github.com/alien4cloud/alien4cloud-repository-plugins) | HTTP Artifact Resolver Plugin| 1.3.0 | 
| [alien4cloud-premium-workspace](https://github.com/alien4cloud/alien4cloud-premium-repository-plugins) | Alien 4 Cloud Premium Workspaces| 1.3.0. |
| [alien-vault-plugin](https://github.com/alien4cloud/alien4cloud-vault-plugin) | Integration to HashiCorp Vault|  2.0.0 | 
| [alien4cloud-workflow-scheduler-plugin](https://github.com/alien4cloud/alien4cloud-workflow-scheduler) | Scheduler topology processing plugin using cron executions on Orchestrator| 2.1.0 | 
| [alien4cloud-yorc-provider](https://github.com/alien4cloud/alien4cloud-yorc-provider) | Yorc Orchestrator Provider, manages interactions with a running Yorc instance.| 2.2.0 |
| [alien4cloud-kubernetes-plugin](https://github.com/alien4cloud/alien4cloud-kubernetes-plugin) | Allows transformation of a TOSCA generic topology into a specific kubernetes topology. It contains support for topology and policies modification.| 2.2.0 | 
| [alien4cloud-k8s-spark-jobs](https://github.com/alien4cloud/alien4cloud-k8s-spark-jobs)| Features Spark job modelization for running into Kubernetes cluster | 3.0.0|
| [alien4cloud-kafka-listener](https://github.com/alien4cloud/alien4cloud-kafka-listener)| Subscribes to Kafka Messages. Features application workflow launches, service creation, git repositories pull| 3.0.0 |
| [alien4cloud-k8s-webhook](https://github.com/alien4cloud/alien4cloud-k8s-webhook) | Allows Kubernetes resources to enriched or validated thanks using the webhook mechanism| 3.1.0 |
| [alien4cloud-rms-scheduler-plugin](https://github.com/alien4cloud/alien4cloud-rms-scheduler-plugin) | A rule based scheduler embedding drools.| 3.1.0 |

These 2 sample plugins illustrate how plugins can be used and implemented  :

{: .table .table-striped }
| Plugin| Description|  Since version| 
|:---------|:------------|:---------|
| [sample-topology-validator-plugin](https://github.com/alien4cloud/sample-topology-validator-plugin) | A sample plugin that checks topology nodes name (minimum length and pattern) | 2.2.0 | 
| [alien4cloud-plugin-sample](https://github.com/alien4cloud/alien4cloud-plugin-sample) | Set of modules that illustrate how to write plugin with backend and UI, wizard addon, topology modifier:| 1.3.0 | 