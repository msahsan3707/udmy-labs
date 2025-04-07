<pre>
<h1> ConfigMap2-OC-set-cmd </h1>

[mahsan@r9 udmy-labs]$ oc new-project peach
Now using project "peach" on server "https://api.crc.testing:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

[mahsan@r9 udmy-labs]$ echo "Apache Rocks" > index.html
[mahsan@r9 udmy-labs]$ oc create configmap content --from-file=index.html
configmap/content created
<b>[mahsan@r9 udmy-labs]$ oc new-app --image=bitnami/apache --name=apache</b>
--> Found container image 99cbd80 (25 hours old) from Docker Hub for "bitnami/apache"

    * An image stream tag will be created as "apache:latest" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "apache" created
    deployment.apps "apache" created
    service "apache" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/apache'
    Run 'oc status' to view your app.
[mahsan@r9 udmy-labs]$ oc get all
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
NAME                          READY   STATUS    RESTARTS   AGE
pod/apache-7bbf694668-f28rm   1/1     Running   0          3m30s

NAME             TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)             AGE
service/apache   ClusterIP   10.217.5.52   <none>        8080/TCP,8443/TCP   3m31s

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/apache   1/1     1            1           3m31s

NAME                                DESIRED   CURRENT   READY   AGE
replicaset.apps/apache-7549db4544   0         0         0       3m31s
replicaset.apps/apache-7bbf694668   1         1         1       3m30s

NAME                                    IMAGE REPOSITORY                                                       TAGS     UPDATED
imagestream.image.openshift.io/apache   default-route-openshift-image-registry.apps-crc.testing/peach/apache   latest   3 minutes ago
[mahsan@r9 udmy-labs]$

<b>[mahsan@r9 udmy-labs]$ oc set volumes deployment/apache --add --type=configmap --configmap-name=content --mount-path=/app </b>
info: Generated volume name: volume-jbgkk
deployment.apps/apache volume updated
[mahsan@r9 udmy-labs]$ oc get pods
NAME                      READY   STATUS    RESTARTS   AGE
apache-59fb8f8cbc-vfx5m   1/1     Running   0          9s
[mahsan@r9 udmy-labs]$
spec:
  containers:
  - image: bitnami/apache@sha256:a63e44cbc006f6f8382500c5c5a1f103e3f6274a4d4f7db1552e39a625eb5305
    imagePullPolicy: IfNotPresent
    name: apache
    ports:
    - containerPort: 8080
      protocol: TCP
    - containerPort: 8443
      protocol: TCP
    resources: {}
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop:
        - ALL
      runAsNonRoot: true
      runAsUser: 1000670000
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /app
      name: volume-jbgkk
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-4672j
      readOnly: true
  dnsPolicy: ClusterFirst


<b>[mahsan@r9 ~]$ oc expose svc apache --hostname=apache.apps-crc.testing </b>
route.route.openshift.io/apache exposed
[mahsan@r9 ~]$ oc get routes.route.openshift.io
NAME     HOST/PORT                 PATH   SERVICES   PORT       TERMINATION   WILDCARD
apache   apache.apps-crc.testing          apache     8080-tcp                 None
[mahsan@r9 ~]$ curl http://apache.apps-crc.testing
Apache Rocks
[mahsan@r9 ~]$
 
</pre>
