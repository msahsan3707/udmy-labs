<pre>
<h1> Config Example 1</h1>
[mahsan@r9 ~]$ oc new-project apricot
Now using project "apricot" on server "https://api.crc.testing:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

[mahsan@r9 ~]$ oc create configmap url --from-literal=api_url=https://apiurl.example.com
configmap/url created

[mahsan@r9 ~]$ oc get  cm url -o yaml
apiVersion: v1
data:
  api_url: https://apiurl.example.com
kind: ConfigMap
metadata:
  creationTimestamp: "2025-04-06T18:14:46Z"
  name: url
  namespace: apricot
  resourceVersion: "50298"
  uid: 8960c235-8e92-4b98-a729-9557cc6c72b0
[mahsan@r9 ~]$
 
 [mahsan@r9 ~]$ oc run busybox --image=busybox --dry-run=client -o yaml > busybox-pod.yaml
[mahsan@r9 ~]$

[mahsan@r9 ~]$ oc create -f busybox-pod.yaml
Warning: would violate PodSecurity "restricted:latest": allowPrivilegeEscalation != false (container "busybox" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "busybox" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "busybox" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "busybox" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
pod/busybox created
[mahsan@r9 ~]$ oc get pods
NAME      READY   STATUS             RESTARTS     AGE
busybox   0/1     CrashLoopBackOff   1 (5s ago)   11s
[mahsan@r9 ~]$ oc logs busybox
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.217.4.1:443
HOSTNAME=busybox
SHLVL=1
HOME=/root
TERM=xterm
KUBERNETES_PORT_443_TCP_ADDR=10.217.4.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.217.4.1:443
KUBERNETES_SERVICE_HOST=10.217.4.1
PWD=/
NSS_SDB_USE_CACHE=no
api_url=https://apiurl.example.com

[mahsan@r9 ~]$ cat busybox-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - image: busybox
    name: busybox
    command: ["bin/sh", "-c" , "printenv"]
    env:
      - name: api_url
        valueFrom:
          configMapKeyRef:
            name: url
            key: api_url
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}




</pre>
