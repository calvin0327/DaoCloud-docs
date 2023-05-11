# Upgrade the microservice engine online

Before you begin, understanding the deployment architecture of the microservice engine can help you better understand the subsequent upgrade process.

The microservice engine consists of two components:

- The `skoala` component is installed in the control plane cluster and is used to load the microservice engine module in the level 1 navigation bar of DCE 5.0
-  `skoala-init` Components are installed in the working cluster and are used to provide the core functions of the microservice engine, such as creating registries, gateway instances, and so on

!!! note

    - The two components must be upgraded at the same time when upgrading the microservice engine. Otherwise, version incompatibility may occur.
    - See [ release notes ](../intro/release-notes.md) for version updates of the microservice engine.

## Upgrade `skoala` components

Because the `skoala` component is installed in the control plane cluster, you need to perform the following operations in the control plane cluster.

1. Upgrade the dependent database

    !!!!!!!!! note "This step is Optional"

        - If the upgrade is from ** Microservice Engine version ** < 0.19.0 to 0.19.0, ** must ** Perform this step.
        - If you upgrade from 0.19.0 to a later ** Microservice Engine version **, check whether the corresponding [ release notes ](../intro/release-notes.md) mentions the database you need to upgrade to:
            
            - If you want to upgrade the database, perform this step.
            - If upgrading the database is not mentioned, skip this step.

    Run the `kubectl apply` command after modifying the configuration as described below in the `job.yaml` file.

    ```yaml
    sweet:
    type:  UPGRADE
      target: 0.17.0  # (1) 
      tag: v0.19.0   # (2) 
    ```

    1. The value is fixed and does not need to be modified
    2. The value is fixed and does not need to be modified

2. Run the following command to back up the original data

    ```
    helm -n skoala-system get values skoala > skoala.yaml
    ```

3. Add Helm repository for microservice engine

	  ```
    helm repo add skoala https://release.daocloud.io/chartrepo/skoala
    ```

4. Update Helm repository for microservice engine

    ```
    helm repo update
    ```

5. Run the `helm upgrade` command

    ```
    helm --kubeconfig /tmp/deploy-kube-config upgrade --install --create-namespace -n skoala-system skoala skoala/skoala --version=0.19.2 --set hive.image.tag=v0.19.2 --set sesame.image.tag=v0.19.2 -f skoala.yaml
    ```

    > Set the values of `version`, `hive.image.tag`, and `sesame.image.tag` to the version of the micro-service engine to be upgraded.

## Upgrade `skoala-init` components

Because the `skoala-init` component is installed in a working cluster, you need to do the following once in each working cluster.
<! -- If an upgrade is needed, it will be highlighted in release notes -->

1. Backup original parameters

    ```
    helm -n skoala-system get values skoala-init > skoala-init.yaml
    ```

2. Add Helm repository for microservice engine

    ```
    helm repo add skoala https://release.daocloud.io/chartrepo/skoala
    ```

3. Add Helm repository for microservice engine

    ```
    helm repo update
    ```

4. Run the `helm upgrade` command

    ```
    helm --kubeconfig /tmp/deploy-kube-config upgrade --install --create-namespace -n skoala-system skoala-init skoala/skoala-init --version=0.19.2 --set nacos-operator.image.tag=v0.19.2 --set skoala-agent.image.tag=v0.19.2 --set sentinel-operator.image.tag=v0.19.2 -f skoala-init.yaml
    ```
  
    !!! note

        - Set the values of `version`, `nacos-operator.image.tag`, `skoala-agent.image.tag`, and `sentinel-operator.image.tag` to the version of the micro-service engine that you want to upgrade.

5. You can manually delete other crd files that need to be updated and run them again as required.

    ```
    kubectl apply -f xxx.yaml
    ```