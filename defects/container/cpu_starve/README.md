# CPU Starvation
In K8s, the CPU request is implemented using cpu control group's cpu.shares. See https://mechpen.github.io/posts/2020-04-27-cfs-group/index.html and https://mechpen.github.io/posts/2020-07-25-k8s-cpu/index.html for more details.

In this scenario, the container gets starved during CFS scheduling due to lack of cpu shares (lack of cpu requests in K8s).

## Load generator
We first leverage Locust to generate load against /productpage against bookinfo app. You can find locust installation instruction in the root README.md.

To start the load generator.

`kubectl port-forward svc/locust-master 8089:8089`

Go to the browser: http://localhost:8089/ and start a new load test. Below is the configuration as an example.

Number of users: 100
Spawn rate: 100
Host: http://productpage.bookinfo2.svc.cluster.local:9080


## Produce the Defect
In order to produce the defect for the productpage container, we can lower the CPU request of Productpage deployment. See the following snippet as an example.

```yaml
spec:
      containers:
      - image: docker.io/istio/examples-bookinfo-productpage-v1:1.16.4
        imagePullPolicy: IfNotPresent
        name: productpage
        ports:
        - containerPort: 9080
          protocol: TCP
        resources:
          limits:
            cpu: 100m
            memory: 200Mi
          requests:
            cpu: 50m
            memory: 30Mi
        securityContext:
          runAsUser: 1000
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /tmp
          name: tmp
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      serviceAccount: bookinfo-productpage
      serviceAccountName: bookinfo-productpage
      terminationGracePeriodSeconds: 30
      volumes:
      - emptyDir: {}
        name: tmp
```

Once the container is restarted with the new request, you can check the rate of cpu time spent on a runqueue (ssh to a worker node productpage pod is running on and check /proc/<pid>/schedstat)as explained in https://docs.kernel.org/scheduler/sched-stats.html#proc-pid-schedstat. 

Meanwhile, you can also visit the Locust UI to observe the impact of starvation by checking the response time in Locust chart.
