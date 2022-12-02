# CPU Noisy Neighbor
A CPU noisy neighbor is defined as a container who's using a significant amount of CPU, causing node to congest. It will cause its neighbor to starve and experience significant performance degradation.

## Produce the defect
Make sure you have installed the litmus per README.md in the root directory. And run the following to create a chaosEngine for generating CPU stress on the ratings pod.

`kubectl create -f cpuChaos.yaml`

The above command will create a ChaosEngine resource object for cpu hog with the following parameters, and feel free to customize.

  **appinfo:** The information contains the scope of the experiment, appns, applable and appkind are configurable. In this example, we are stressing ratings pods for the bookinfo app.

  **TARGET_CONTAINER:** The container where chaos will be injected

  **TOTAL_CHAOS_DURATION:** The duration of the scenario. 100s is used here.

  **CONTAINER_RUNTIME and SOCKET_PATH:** Container runtime, default is docker. For Containerd, see https://docs.litmuschaos.io/docs/troubleshooting/

  **CPU_CORES:** number of CPU cores to be consumed
