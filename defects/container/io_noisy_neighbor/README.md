# IO Noisy Neighbor 
This experiment creates the scenario of container IO Stress. It will cause CPU IO Wait increase on the underlying node and potential IO Throttling on the neighbor pods.

## Produce the defect
Make sure you have installed the litmus per README.md in the root directory. And run the following to create a chaosEngine for generating IO stress on the ratings pod.

`kubectl create -f io_stress.yaml`

The above command will create a ChaosEngine resource object for IO stress with the following parameters, and feel free to customize.

   **appinfo:** The information contains the scope of the experiment, appns, applable and appkind are configurable. In this example, we are stressing ratings pods for the bookinfo app.

   **FILESYSTEM_UTILIZATION_PERCENTAGE:** The percentage of free space of file system, need to be stressed, see https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-io-stress/#filesystem-utilization-percentage

   **TOTAL_CHAOS_DURATION:** The duration of the scenario. 100s is used here.

   **CONTAINER_RUNTIME and SOCKET_PATH:** Container runtime, default is docker. For Containerd, see https://docs.litmuschaos.io/docs/troubleshooting/