## CPU stress
The scenario can be recreated by

`kubectl create -f bookinfo-cpu-attack.yaml`

The above command will create a StressChaos resource object with the following parameters, and feel free to customize.
workers: The number of threads that apply cpu stress, 10 is used here.
load: The percentage of CPU occupied, 100 means full load. 100 is used here.
namespaces and labelselectors: Specify the workloads the scenario will be applied on. In this example, we are stressing pods with tag version: v2

### Result
High CPU Utilization for the v2 pods

The latency actually did NOT get impacted.   

### Cleanup
`kubectl delete -f bookinfo-cpu-attack.yaml`
