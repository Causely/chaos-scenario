## Noisy Neighbor
In https://github.com/enlinxu/chaos-scenario/tree/main/pod/cpu, we simulated the CPU stress attack, and the conclusion is we don't observe the performance degradation even when the CPU of the pods are running hot.

BUT, what happened to my Grafana pod??

In the test environment, the grafana pod and the pod(review-v3) that experiences the CPU stress are on the same nodes

You can use the yaml here and make necessary change to pick the pod that runs on the same node as the grafana, to reproduce the scenario.

`kubectl create -f bookinfo-cpu-noisy-neighbor.yaml`

The above command will create a StressChaos resource object with the following parameters, and feel free to customize.

	**workers**: The number of threads that apply cpu stress, 10 is used here.

	**load**: The percentage of CPU occupied, 100 means full load. 100 is used here.

    **duration**: The duration of the scenario. 300s is used here.

	**namespaces and labelselectors**: Specify the workloads the scenario will be applied on. In this example, we are stressing pods with tag version: v3

### Result
High CPU Utilization for the v3 pods, which didn't impact the latency of the review-v3 service. BUT, the grafana pod becomes unavailable because of the noisy-neighbor review-v3 pod.



### Cleanup
`kubectl delete -f bookinfo-cpu-noisy-neighbor.yaml`
