---
title: Verify WordPress Chart Installation
description: This tutorial explains how to verify that WordPress chart installed successfully
---


Once the helm chart installation done we need to verify all the pods and services are up and running.

Execute below command to check status of pods and services: 

### Check the pod status


```execute
kubectl get pods --namespace wordpress
```

You will see similar to this output:

```
NAME                         READY   STATUS    RESTARTS   AGE
wordpress-56cb97d698-pvvm6   1/1     Running   0          100s
wordpress-mariadb-0          1/1     Running   0          100s
```

Once the wordpress POD is up and running , your wordpress is ready to use



### Check all the Kubernetes resources status

You can run the following command to know status of all the deployed resources inside the namespace wordpress


```execute
kubectl get all --namespace wordpress
```

All the deployment and service status should be Running.

```
NAME                             READY   STATUS    RESTARTS   AGE
pod/wordpress-56cb97d698-pvvm6   1/1     Running   0          2m16s
pod/wordpress-mariadb-0          1/1     Running   0          2m16s

NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/wordpress           NodePort    10.99.150.199    <none>        80:31450/TCP,443:31851/TCP   2m16s
service/wordpress-mariadb   ClusterIP   10.105.148.213   <none>        3306/TCP                     2m16s

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/wordpress   1/1     1            1           2m16s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/wordpress-56cb97d698   1         1         1       2m16s

NAME                                 READY   AGE
statefulset.apps/wordpress-mariadb   1/1     2m16s
```
