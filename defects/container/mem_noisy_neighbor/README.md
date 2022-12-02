# Memory Noisy Neighbor 
This experiment creates the scenario of container memory hog. It will cause memory congestion on the underlying node and potential OOM kill on the neighbor pods.

## Produce the defect
Make sure you have installed the litmus per README.md in the root directory. And run the following to create a chaosEngine for generating memory stress on the ratings pod.

`kubectl create -f memChaos.yaml`

The above command will create a ChaosEngine resource object for memory hog with the following parameters, and feel free to customize.

   **appinfo:** The information contains the scope of the experiment, appns, applable and appkind are configurable. In this example, we are stressing ratings pods for the bookinfo app.

   **MEMORY_CONSUMPTION:** The amount of memory injected in MB

   **TOTAL_CHAOS_DURATION:** The duration of the scenario. 100s is used here.

   **CONTAINER_RUNTIME and SOCKET_PATH:** Container runtime, default is docker. For Containerd, see https://docs.litmuschaos.io/docs/troubleshooting/