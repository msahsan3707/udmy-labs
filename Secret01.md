<pre>
<h1> Secret </h1>
[mahsan@r9 ~]$ oc new-project atlantics
Now using project "atlantics" on server "https://api.crc.testing:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.43 -- /agnhost serve-hostname

<b>[mahsan@r9 ~]$ oc create secret generic mysql-secret --from-literal=MYSQL_DATABASE=ex280 --from-literal=MYSQL_USER=ex280 --from-literal=MYSQL_PASSWORD=ex280 </b>
secret/mysql-secret created


<b>[mahsan@r9 ~]$ oc new-app --name=mysql --image=registry.redhat.io/rhel8/mysql-80:latest </b>
--> Found container image 24bf26f (3 weeks old) from registry.redhat.io for "registry.redhat.io/rhel8/mysql-80:latest"

    MySQL 8.0
    ---------
    MySQL is a multi-user, multi-threaded SQL database server. The container image provides a containerized packaging of the MySQL mysqld daemon and client application. The mysqld server daemon accepts connections from clients and provides access to content from MySQL databases on behalf of the clients.

    Tags: database, mysql, mysql80, mysql-80

    * An image stream tag will be created as "mysql:latest" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "mysql" created
    deployment.apps "mysql" created
    service "mysql" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/mysql'
    Run 'oc status' to view your app.
[mahsan@r9 ~]$

[mahsan@r9 ~]$ oc get pods
NAME                     READY   STATUS              RESTARTS   AGE
mysql-5bfc857bb5-r7zzb   0/1     ContainerCreating   0          38s
[mahsan@r9 ~]$ oc get pods
NAME                     READY   STATUS             RESTARTS     AGE
mysql-5bfc857bb5-r7zzb   0/1     CrashLoopBackOff   1 (2s ago)   42s
[mahsan@r9 ~]$ oc logs mysql-5bfc857bb5-r7zzb
=> sourcing 20-validate-variables.sh ...
You must either specify the following environment variables:
  MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
  MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
  MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
  MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
Optional Settings:
  MYSQL_LOWER_CASE_TABLE_NAMES (default: 0)
  MYSQL_LOG_QUERIES_ENABLED (default: 0)
  MYSQL_MAX_CONNECTIONS (default: 151)
  MYSQL_FT_MIN_WORD_LEN (default: 4)
  MYSQL_FT_MAX_WORD_LEN (default: 20)
  MYSQL_AIO (default: 1)
  MYSQL_KEY_BUFFER_SIZE (default: 32M or 10% of available memory)
  MYSQL_MAX_ALLOWED_PACKET (default: 200M)
  MYSQL_TABLE_OPEN_CACHE (default: 400)
  MYSQL_SORT_BUFFER_SIZE (default: 256K)
  MYSQL_READ_BUFFER_SIZE (default: 8M or 5% of available memory)
  MYSQL_INNODB_BUFFER_POOL_SIZE (default: 32M or 50% of available memory)
  MYSQL_INNODB_LOG_FILE_SIZE (default: 8M or 15% of available memory)
  MYSQL_INNODB_LOG_BUFFER_SIZE (default: 8M or 15% of available memory)

For more information, see https://github.com/sclorg/mysql-container
[mahsan@r9 ~]$

<b>[mahsan@r9 ~]$ oc set env deployment/mysql --from=secret/mysql-secret </b>
deployment.apps/mysql updated
[mahsan@r9 ~]$ oc get pods
NAME                     READY   STATUS    RESTARTS   AGE
mysql-58589567d7-2jwlv   1/1     Running   0          10s
[mahsan@r9 ~]$

Controlled By:  ReplicaSet/mysql-58589567d7
Containers:
  mysql:
    Container ID:   cri-o://89c49204d585641f05add95500621cfbac39d4cb2bb5dc9aaa46735cef29e342
    Image:          registry.redhat.io/rhel8/mysql-80@sha256:3c7b2ea314bee059fa306c486fdc38b17811ef4e9fb0afea860e20ea13cdb03e
    Image ID:       registry.redhat.io/rhel8/mysql-80@sha256:3c7b2ea314bee059fa306c486fdc38b17811ef4e9fb0afea860e20ea13cdb03e
    Port:           3306/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 08 Apr 2025 14:50:50 -0700
    Ready:          True
    Restart Count:  0
    Environment:
      MYSQL_USER:      <set to the key 'MYSQL_USER' in secret 'mysql-secret'>      Optional: false
      MYSQL_DATABASE:  <set to the key 'MYSQL_DATABASE' in secret 'mysql-secret'>  Optional: false
      MYSQL_PASSWORD:  <set to the key 'MYSQL_PASSWORD' in secret 'mysql-secret'>  Optional: false
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-xqvmm (ro)
Conditions:

template:
      metadata:
        annotations:
          openshift.io/generated-by: OpenShiftNewApp
        creationTimestamp: null
        labels:
          deployment: mysql
      spec:
        containers:
        - env:
          - name: MYSQL_USER
            valueFrom:
              secretKeyRef:
                key: MYSQL_USER
                name: mysql-secret
          - name: MYSQL_DATABASE
            valueFrom:
              secretKeyRef:
                key: MYSQL_DATABASE
                name: mysql-secret
          - name: MYSQL_PASSWORD
            valueFrom:
              secretKeyRef:
                key: MYSQL_PASSWORD
                name: mysql-secret
          image: registry.redhat.io/rhel8/mysql-80@sha256:3c7b2ea314bee059fa306c486fdc38b17811ef4e9fb0afea860e20ea13cdb03e
          imagePullPolicy: IfNotPresent
          name: mysql
          ports:
          - containerPort: 3306
            protocol: TCP
          resources: {}
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext: {}
        terminationGracePeriodSeconds: 30
  status:
    availableReplicas: 1
    conditions:


</pre>
