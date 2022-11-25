# CPU Throttling
CPU Throttling is considered as the silent killer of Application response time. https://medium.com/omio-engineering/cpu-limits-and-aggressive-throttling-in-kubernetes-c5b20bd8a718.

## Load generator
We first leverage Locust to generate load against /productpage against bookinfo app. You can find locust installation instruction in the root README.md.

To start the load generator.

`kubectl port-forward svc/locust-master 8089:8089`

Go to the browser: http://localhost:8089/ and start a new load test. Below is the configuration as an example.

Number of users: 100
Spawn rate: 100
Host: http://productpage.bookinfo2.svc.cluster.local:9080


## Produce the Defect
Since K8s uses CFS's quota mechanism to implement the limit, CPU Throttling happens whenever the CPU limit is not configured properly. In order to produce the defect for the productpage container, we can lower the CPU limit of Productpage deployment. See the following snippet as an example.

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
            cpu: 100m
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

Once the container is restarted with the new limit, you can check the rate of container_cpu_cfs_throttled_periods_total counter in cadvisor, and confirm the CPU Throttling on the productpage container. 

Meanwhile, you can also visit the Locust UI to observe the impact of throttling by checking the response time in Locust chart.