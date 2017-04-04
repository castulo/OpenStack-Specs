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


Requirements
------------

**Functional**

1. The tool must be agnostic to the OpenStack environment and to the deployment tool used, performing actions consistently across different environments
2. It must validate that services are actually at the correct release version at any given time
3. It must validate that all services are functional at any given time (e.g. before, during and after an upgrade)
4. It must provide a way of creating and validating persistent resources, like VMs or volumes, at any given time
5. It must be capable of measuring if there is API downtime during a specified period of time for any of the 
`supported services`_ listed below
6. It must be capable to detect if all of the `supported services`_ listed below are fully available continuously 
during a specified period of time
7. It must have a centralized store for logs of tests and data collected
8. It must attempt to clean up after itself, if resources were created for testing or monitoring purposes they must be 
removed after they are no longer needed
9. It must be pluggable in services meaning that when new services are ready to implement an upgrade strategy (for example 
zero downtime or zero impact), they can be easily added to the scope of the tool
10. It must provide a common public interface that others can use to consume the toolset
11. It must provide a common public interface that the toolset can use to communicate with deployment tools so certain 
steps of the deployment or the upgrade can be triggered
12. In case of cascade errors the tool must stop actions like creating resources that would lead to an even more unstable
environment, attempt to clean up and report back
13. It must run tests using non-admin OpenStack user(s)
14. It should be capable of measuring the performance of the `supported services`_ listed below during a specified period of time
15. It should auto discover cloud services and configure the tool accordingly
16. It should verify that all requests made during an upgrade are honored at some point successfully, validating that they
are not just added to a queue but are actually processed
17. It should provide the capability to add tests via a plugin system
18. It should use existing test discovery and plugin mechanisms to allow maximum reuse of test resources from existing 
OpenStack test tools
19. It should be capable of performing tests on one of the `supported services`_ at a time
20. It could include a GUI where results can be easily interpreted and could include trends
21. It could be capable of verifying if the data plane is accessible during a specified period of time
