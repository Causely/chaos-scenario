## OOMKill
The OOMKill scenario can be recreated by

`kubectl create -f oom-review-pods.yaml`

The above command will create a StressChaos resource object with the following parameters, and feel free to customize.
workers: The number of threads that apply memory stress. 4 is used here.
size: The memory size to be occupied or a percentage of the total memory size. 100% is used here
duration: The duration of the scenario. 100s is used here.
namespaces and labelselectors: Specify the workloads the scenario will be applied on. In this example, we are stressing the review pods within the BookInfo App

### Result
Pods get killed after the StressChaos above is created
![Screen Shot 2022-08-26 at 10 49 38 AM](https://user-images.githubusercontent.com/4391815/186933272-30d91ce3-741e-4aa1-ae8a-2aba292569ce.png)

API response time increased significantly or not available

![Screen Shot 2022-08-26 at 10 49 08 AM](https://user-images.githubusercontent.com/4391815/186933944-3a524f42-2161-4171-8412-78c351fa5dce.png)
![Screen Shot 2022-08-26 at 10 51 40 AM](https://user-images.githubusercontent.com/4391815/186933961-9548cfa0-947f-4ef5-8df5-4ab48ed1fe07.png)

### Cleanup
`kubectl delete -f oom-review-pods.yaml`
