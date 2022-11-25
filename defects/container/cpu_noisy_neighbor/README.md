# CPU Noisy Neighbor
A CPU noisy neighbor is defined as a container who's using a significant amount of CPU, causing node to congest. It will cause its neighbor to starve and experience significant performance degradation.

## Produce the defect
Make sure you have installed the litmus per README.md in the root directory. And run the following to create a chaosEngine for generating CPU stress on the ratings pod.

`kubectl create -f cpuChaos.yaml`
