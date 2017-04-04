# temp

Use Cases
---------

There are three main use cases for the toolset:

- In the first use case, the toolset has no control over the deployment/upgrade process and no control of the test process.
  A consumer, someone like an OpenStack developer, or a downstream project (OpenStack Ansible, Kolla, etc.), 
  use tools at will from the  toolset to perform validations on a cloud.They can choose to run as many tools from the toolset
  as they want and in whatever order they want. In this use case, the toolset is not the one controlling the process, so it is
  unaware of the upgrade process. Therefore if used this way to test upgrades, the consumer would be responsible for deploying
  the environment, running the validators and observers in the correct place in the upgrade process, performing the upgrade,
  and stopping the observers once the upgrade finishes.

- The second use case is about the toolset having partial control over the deployment/upgrade process but full control of the test process.
In this use case, the toolset uses a driver to communicate with the orchestrator (in this case a deployment tool like OpenStack Ansible, Kolla, etc.).
When testing an upgrade this way, the toolset would be responsible for running the deployment/upgrade process by signaling the orchestrator to perform these actions, and
would also be responsible for running the validators and observers in the appropriate place during the upgrade process. However it is important to note that in this use case
the toolset signals the orchestrator to perform actions in the OpenStack environment but has no control whatsoever and how things are done by the orchestrator, so the orchestrator
is responsible for the upgrade process itself.

- (**Use Case not in scope for this Spec**) The third use case is about having full control over the deployment/upgrade process and full control of the test process.
This use case is very similar to the second one, but in this case the toolset has its own orchestrator therefore is in full control of the deployment/upgrade process.
