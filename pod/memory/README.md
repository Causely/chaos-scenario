# OOMKill
The OOMKill scenario can be recreated by
`kubectl create -f oom-review-pods.yaml`

The above command will create a StressChaos resource object with the following parameters, and feel free to customize.
workers: The number of threads that apply memory stress. 4 is used here.
size: The memory size to be occupied or a percentage of the total memory size. 100% is used here
duration: The duration of the scenario. 100s is used here.
namespaces and labelselectors: Specify the workloads the scenario will be applied on. In this example, we are stressing the review pods within the BookInfo App

### Result

