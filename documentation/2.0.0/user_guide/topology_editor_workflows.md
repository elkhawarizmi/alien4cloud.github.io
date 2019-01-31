---
layout: post
title: Workflows
root: ../../
categories: DOCUMENTATION-2.0.0
parent: [user_guide, topology_editor]
node_name: topology_editor_workflows
weight: 180
---

{% summary %}{% endsummary %}

A workflow defines sequences of steps that act upon topology's nodes and relationships in order to achieve a defined goal.

Each topology embed several workflows:

- **standard** workflow (**install**, **uninstall**, **start**, **stop**) : when you are designing a topology, a4c maintains the two standard workflows (install and uninstall) following the TOSCA normative lifecycle, the two others (start and stop) workflows are Alien4Cloud specific to ease application maintenance. You can customize them in order to change the way steps are orchestrated.
- **custom** workflows: you can create as many custom workflows as you want.

We can also talk about **deduced** workflows: these workflows are deduced from standard workflows. For example, when you scale up a host, the host node installation sub-workflow is deduced from **install** workflow (by isolating all steps concerning this particular host and ignoring all other hosts and links from/to steps outside this host).

# Workflow steps

So a workflow is a set of steps that are eventually linked. Actually it's a directed acyclic graph.

A step can have predecessors and successors. Rules are :

- If A is followed by B, then A will be executed before B.
- if A is followed by B and C, then B and C will be executed in parallel after A (fork).
- if C is preceded by A and B, then C will be executed only after A and B are terminated (join).
- if a step has no predecessors, it will be linked from the workflow entry point (start).
- if a step has no successors, it will be linked to the workflow end point (end).

# Workflow activity

A step is associated with an activity. Currently, an activity can be:

- **set state** activity: this activity is used to change the state of a node.
- **call operation** activity: used to call an operation on a node interface.
- **delegate workflow** activity: this is used by a4c to specify that a particular node lifecycle management should be handled by the orchestrator (consider it as a black box).
- **inline workflow** activity: Include other workflow in the current workflow.

{% info %}
<h5>Delegate activity and abstract nodes</h5>
When you add an abstract node to a topology, a4c will add a delegate workflow activity, until you replace the node by a concrete implementation.
If the node is not replaced before the deployment, it must be substituted at deployment matching stage. The lifecycle for this node will then be managed by the orchestrator.
{% endinfo %}

{%warning%}
<h5>Node relationship operations</h5>
When you add a node in a topology, a4c adds all the necessary steps in the standard workflows : all the operations of `tosca.interfaces.node.lifecycle.Standard` interface are added in the correct order.

When you add a relationship between two nodes, the steps concerning those two nodes and those of the relationship are organized regarding the standard lifecycle rules described in TOSCA.
{%endwarning%}

{%warning%}
<h5>Importance of state change activity</h5>
As we can see in the image below, each operation call is surrounded with state changes.
[![*Topology version](../../images/user_guide/application/wf-edition3-state-change.png)](../../images/user_guide/application/wf-edition3-state-change.png){:target="_blank"}
Here, the **create** operation is preceded by a state change to '**creating**' and followed by a state change to '**created**'. This is defined by TOSCA in the standard lifecycle.

This is very important to surround each standard interface operation call by these state changes and even add these state changes even if the operation is not added in the workflow.
These state changes are mainly used as bound when some relationship are added in the topology.
{%endwarning%}

# Workflow edition

Editing a complex workflow can become a mess if you have a lot of nodes and relationships in your topology. We have tried to build a intuitive editor to help you to customize your workflows.

Basic usage rules are:

- the first time you click on a step, it's spinned (and becomes blue). The spinned step is the one on which you will be able to make some actions. All possible actions on the spinned step are listed in the panel at the right of the screen.

[![*Topology version](../../images/2.0.0/user_guide/applications/wf-edition1.png)](../../images/user_guide/application/wf-edition1.png){:target="_blank"}

In the image above, we have selected the step named 'create' and we are about to insert an operation call.

- when a step is spinned, when you click again on other steps, you will add them to the selection (yellow background). Then you will be able to make actions between the spinned step and the selected steps. In the image below, we are adding a link between the tomcat.started and the java.add_target steps.

[![*Topology version](../../images/2.0.0/user_guide/applications/wf-edition2.png)](../../images/user_guide/application/wf-edition2.png){:target="_blank"}

When you edit a workflow, some validations are checked and some errors can be raised:

- cycles are not accepted.

In the image above, a cycle is detected and an error is raised.

[![*Topology version](../../images/2.0.0/user_guide/applications/wf-edition3-error-cycle.png)](../../images/user_guide/application/wf-edition3-error-cycle.png){:target="_blank"}

Some actions are not allowed:

- you can not remove/add state change activity steps in standard workflows.
- you can not remove delegate activity steps in standard workflows.
- you can not add any activity for an abstract node.

# Workflow limitations

{%warning%}
<h5>Topology composition</h5>
Custom workflows are not compatible with topology composition: when you use topology composition (when you add a node of a type that is a result of a template exposition), all your customizations will be lost (the standard workflow will be regenerated).
{%endwarning%}