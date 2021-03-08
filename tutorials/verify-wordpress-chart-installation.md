---
title: Verify WordPress Chart Installation
description: Learn how to verify that WordPress chart installed successfully.
---


Once the helm chart installation is complete, verify that all the pods and services are up and running.

### Check the Pod Status

- Execute the command below to check the status of pods and services.

```execute
kubectl get pods --namespace wordpress
```

You will see the output as below.

```
NAME                         READY   STATUS    RESTARTS   AGE
wordpress-56cb97d698-pvvm6   1/1     Running   0          100s
wordpress-mariadb-0          1/1     Running   0          100s
```

If the WordPress Pod is up and running, it means that WordPress is ready to use.



### Check the status of all the Kubernetes resources


- Run the following command to know status of all the deployed resources inside the namespace “wordpress”


```execute
kubectl get all --namespace wordpress
```

The output should display the deployment and service status as Running.

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

It takes a while for the Service Status to be RUNNING. Please wait till the time before moving forward.


### Conclusion

WordPress chart installed successfully.
