# Install and configure Traefik

## Introduction

The WebLogic Kubernetes Operator supports three load balancers or ingress controllers: Traefik, NGINX, and Apache. Samples are provided in the [documentation](https://github.com/oracle/weblogic-kubernetes-operator/blob/v2.5.0/kubernetes/samples/charts/README.md).

This lab demonstrates how to install the [Traefik](https://traefik.io/)  ingress controller to provide load balancing for WebLogic Server clusters.

Estimated Lab Time: 10 minutes

## **STEP 1**: Install the Traefik operator with a Helm chart

1. Change to your operator local Git repository folder.
    ```bash
    <copy>cd ~/weblogic-kubernetes-operator/</copy>
    ```

2. Create a namespace for Traefik:
    ```bash
    <copy>kubectl create namespace traefik</copy>
    ```

3. Set up Helm with the location of the Traefik Helm chart:
    ```bash
    <copy>helm repo add traefik https://helm.traefik.io/traefik --force-update</copy>
    ```

4. Install the Traefik operator in the `traefik` namespace with the provided sample values:

    ```bash
    <copy>helm install traefik-operator \
    traefik/traefik \
    --namespace traefik \
    --values kubernetes/samples/charts/traefik/values.yaml  \
    --set "kubernetes.namespaces={traefik}" \
    --set "serviceType=LoadBalancer"</copy>
    ```

    The output should be similar to the following:
    ```bash
    NAME: traefik-operator
    LAST DEPLOYED: Thu Feb 27 06:23:32 2025
    NAMESPACE: traefik
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
    traefik-operator with docker.io/traefik:v3.3.3 has been deployed successfully on traefik namespace !
    ```

3. The Traefik installation is basically done. Verify the Traefik (load balancer) services:
    ```bash
    <copy>kubectl get service -n traefik</copy>
    ```
    The output should be similar to the following:
    ```bash
    NAME               TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    traefik-operator   LoadBalancer   10.96.22.119.252   141.148.64.40   80:30305/TCP,443:30443/TCP   44s
    ```

    > Please note the EXTERNAL-IP of the traefik-operator service. This is the public IP address of the load balancer that you will use to access the WebLogic Server Administration Console and the sample application.

4. To print only the public IP address, execute this command:
    ```bash
    <copy>kubectl describe svc traefik-operator --namespace traefik | grep Ingress | awk '{print $3}'</copy>
    ```
    The output should be similar to the following:
    ```bash
    141.148.64.40
    ```

5. Verify the `helm` charts:
    ```bash
    <copy>helm list -n traefik</copy>
    ```
    The output should be similar to the following:
    ```bash
    NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
    traefik-operator        traefik         1               2025-02-27 06:23:32.884561115 +0000 UTC  deployed        traefik-34.4.0  v3.3.3      
    ```

    You may now **proceed to the next lab**.

## Acknowledgements
* **Author** -  Ankit Pandey
* **Contributors** - Sid Joshi, Maciej Gruszka 
* **Last Updated By/Date** - Ankit Pandey, March 2025
