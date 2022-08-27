## Network Delay
The scenario can be recreated by

`kubectl create -f bookinfo-delay.yaml`

The above command will create a NetworkChaos resource object with the following parameters, and feel free to customize.
latency: Network latency, 1s is used here.
correlation: Indicates the correlation between the current latency and the previous one. Range of value: [0, 100]. 100 is used here
namespaces and labelselectors: Specify the workloads the scenario will be applied on. In this example, we are creating delay for all pods with version: v2
### Result
The latency for v2 is extremely big.

### Cleanup
`kubectl delete -f bookinfo-delay.yaml`