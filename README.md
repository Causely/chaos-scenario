# chaos-scenario

The chaos-scenario repo contains scenarios which can create defects in your environment, based on chaos-mesh or litmus.

The list of the defects you can run are:
1. [CPU Noisy Neighbor](https://github.com/Causely/chaos-scenario/tree/main/defects/container/cpu_noisy_neighbor)
2. [CPU Starvation](https://github.com/Causely/chaos-scenario/tree/main/defects/container/cpu_starve)
3. [CPU Throttling](https://github.com/Causely/chaos-scenario/tree/main/defects/container/cpu_throttling)
4. [Memory Noisy Neighbor](https://github.com/Causely/chaos-scenario/tree/main/defects/container/mem_noisy_neighbor)
5. [IO Noisy Neighbor](https://github.com/Causely/chaos-scenario/tree/main/defects/container/io_noisy_neighbor)

## Litmus install
https://v1-docs.litmuschaos.io/docs/getstarted/

`kubectl apply -f https://litmuschaos.github.io/litmus/litmus-operator-v1.13.8.yaml`

`helm repo add litmuschaos https://litmuschaos.github.io/litmus-helm/`

`helm install chaos litmuschaos/litmus --namespace=litmus`


Install generic experiments

`kubectl apply -f https://hub.litmuschaos.io/api/chaos/1.13.8?file=charts/generic/experiments.yaml -n litmus`

Create the role binding for litmus experiments  

`kubectl create -f /litmus/litmus_rbac.yaml`

## Chaos-mesh install (optional)
Chaos-mesh can be installed via Helm, please make sure you specify the right container runtime during the install, see more details https://chaos-mesh.org/docs/production-installation-using-helm/#step-4-install-chaos-mesh-in-different-environments


## Sample app
The test application we are using for chaos attack is the Bookinfo app, https://istio.io/latest/docs/examples/bookinfo/. 

## Locust install
Locust is an open source load testing tool. In our chaos scenario, we use Locust to generate load against the bookinfo productpage endpoint.
If you have installed locust already, make sure you have the locust configmap configured with the right endpoint, please refer to scripts-cm.yaml in /locust.

If you have not installed locust, you can install it by:

`cd locust`

`kubectl create -f nodeport.yaml -f scripts-cm.yaml -f master-deployment.yaml -f service.yaml -f slave-deployment.yaml`
