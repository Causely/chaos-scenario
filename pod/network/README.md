## Network Delay
The scenario can be recreated by

`kubectl create -f bookinfo-delay.yaml`

The above command will create a NetworkChaos resource object with the following parameters, and feel free to customize.
latency: Network latency, 1s is used here.
correlation: Indicates the correlation between the current latency and the previous one. Range of value: [0, 100]. 100 is used here
namespaces and labelselectors: Specify the workloads the scenario will be applied on. In this example, we are creating delay for all pods with version: v2
### Result
The latency for v2 is extremely big.

![Screen Shot 2022-08-26 at 10 16 30 PM](https://user-images.githubusercontent.com/4391815/187010521-06067d74-b860-40e3-ade4-c29433bb572b.png)

![Screen Shot 2022-08-26 at 10 17 10 PM](https://user-images.githubusercontent.com/4391815/187010541-9f8605f9-4c34-4ad4-ad76-3e008db72f60.png)


### Cleanup
`kubectl delete -f bookinfo-delay.yaml`
