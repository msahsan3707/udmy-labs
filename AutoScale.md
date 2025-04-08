<pre>
<h1> Auto Scale </h1>

[mahsan@r9 udmy-labs]$ oc get pods
NAME                      READY   STATUS    RESTARTS   AGE
apache-59fb8f8cbc-vfx5m   1/1     Running   1          25h
<b>[mahsan@r9 udmy-labs]$ oc autoscale deployment apache --min=2 --max=8 --cpu-percent=75 </b>
horizontalpodautoscaler.autoscaling/apache autoscaled
[mahsan@r9 udmy-labs]$ oc get hpa
NAME     REFERENCE           TARGETS              MINPODS   MAXPODS   REPLICAS   AGE
apache   Deployment/apache   cpu: <unknown>/75%   2         8         0          7s
[mahsan@r9 udmy-labs]$ oc get deployment
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
apache   2/2     2            2           25h
[mahsan@r9 udmy-labs]$ oc get pods
NAME                      READY   STATUS    RESTARTS   AGE
apache-59fb8f8cbc-l2kdv   1/1     Running   0          95s
apache-59fb8f8cbc-vfx5m   1/1     Running   1          25h
[mahsan@r9 udmy-labs]$


</pre>
