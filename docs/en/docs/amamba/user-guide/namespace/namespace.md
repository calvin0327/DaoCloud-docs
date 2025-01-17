# Namespace management

A namespace is an abstraction used in Kubernetes to isolate resources.

## prerequisites

- The current workspace already has cluster resources, refer to [Binding Resources](../../../ghippo/user-guide/workspace/quota.md).
- The current user is authorized to the `Workspace Admin` role, for details, refer to [Workspace Best Practice](../../../ghippo/user-guide/workspace/ws-best-practice.md).

## create namespace

!!! note

    Only `Workspace Admin` is supported to create namespaces and set namespace quotas, other roles do not support this operation.

1. Click `Namespace` in the left navigation bar to enter the namespace list, and click `Create` in the upper right corner.

    <!--![]()screenshots-->

2. On the `Create Namespace` page, configure the basic information of the namespace.

    <!--![]()screenshots-->

    - name: set the name of the namespace

    - Cluster: Select a cluster from all the clusters bound under the current workspace

    - label: set the label of the namespace

3. After the basic information is set, you need to configure the resource quota of the namespace.

    !!! note

        - If the current cluster has any resource quotas set on the workspace, this resource quota must be set for the namespace when the namespace is created.
        - The resource quota set for the namespace cannot exceed the resource usage limit set by the current cluster in the workspace.
        - An empty request or limit field for a resource quota means that no quota is set for the current namespace.

    The resource quotas currently supported in the workspace include: CPU request, CPU limit, memory request, memory limit, total storage request, and storage volume declaration. Among them, the limit value of CPU and memory resources must be greater than the requested value.

    <!--![]()screenshots-->

4. Click `OK` to complete the resource creation. The screen prompts that the creation is successful, and returns to the namespace list page.

    <!--![]()screenshots-->

5. Click `︙` on the right side of the list, and you can choose `Quota Management`, `Edit Label`, `Delete` and other operations in the pop-up menu.

    !!! warning

        If a namespace is deleted, all resources under the namespace will be deleted, so please proceed with caution.

## Namespace Quotas

In addition to the CPU request, CPU limit, memory request, memory limit, total storage request, and storage volume declaration resource quota required in creating a namespace, other resource quotas can also be set in namespace quota management, such as containers under the namespace Resources such as groups, stateless workloads, stateful workloads, common tasks, and scheduled tasks.

1. On the namespace list page, select a namespace and click `Quota Management`.

    <!--![]()screenshots-->

2. In the pop-up `Resource Quota` dialog box, you can see the resource quota information of the current namespace. Click `Add` under `Application Resources`, select a resource and set a quota, for details, please refer to [Kubernetes Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/) .

    <!--![]()screenshots-->

3. Click `OK` to complete the quota setting.