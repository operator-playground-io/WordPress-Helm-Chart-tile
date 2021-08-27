---
title: WordPress Bitnami Helm Chart Installation
description: Learn how to install WordPress Bitnami Helm Chart
---

### Introduction

This tutorial is intended to provide you knowledge about the complete process that is executed in the background when you press “Install” button. behind the WordPress Chart "Install" button and is just for knowledge perspective. Please do not execute the steps manually if you have already installed the WordPress Helm Chart using "Install" button.


### Install Bitnami WordPress Helm Chart

We have followed the below procedure to install WordPress (Bitnami) Helm Chart.

**Step 1: Create a namespace : "wordpress". **

```execute
kubectl create namespace wordpress
```

**Step 2: Create the hostpath for both MariaDB PersistentVolume and WordPress PersistentVolume with root user privilege and give the full permission to /bitnami folder location.**

The required command is as follows.

```execute
sudo mkdir -p /bitnami/mariadb/data && sudo mkdir -p /bitnami/wordpress/wp-content && sudo chmod -R 777 /bitnami
```


By default, the WordPress chart installs MariaDB on a separate pod inside the cluster and uses it as the WordPress database. So for these two pods: MariaDB pod and WordPress Pod, we need to define PersistentVolumes(PVs).
   
**Step 3:  Define the PersistentVolume(PV) for mariadb-pv where the mariadb data to be stored. The hostPath tells that mysql directory is in /bitnami/mariadb location.**

```execute
cat <<'EOF' > mariadbpv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mariadbpv
  labels:
    app: wordpress
spec:
  storageClassName: manual
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteOnce
  claimRef:
     namespace: wordpress
     name: data-wordpress-mariadb-0
  hostPath:
    path: "/bitnami/mariadb"
EOF
```


Following is the required command to create mariadb PersistentVolume

```execute
kubectl create -f  mariadbpv.yaml
```


**Step 4: Define the PersistentVolume (PV) for wordpress-pv where the Wordpress site data is to be stored. The hostPath tells that the mysql directory is in /data location.**

```execute
cat <<'EOF' > wordpresspv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: wordpresspv
  labels:
    app: wordpress
spec:
  storageClassName: manual
  capacity:
    storage: 50Gi
  accessModes:
    - ReadWriteOnce
  claimRef:
     namespace: wordpress
     name: wordpress
  hostPath:
    path: "/bitnami/wordpress"
EOF
```


- **Execute the command below to create Wordpress PersistentVolume.**

```execute
kubectl create -f  wordpresspv.yaml
```



**Step 5: Add ‘bitnami' to your repo list using the command below.**

Command:

```execute
helm repo add bitnami https://charts.bitnami.com/bitnami
```

You should see the following output:

```
"bitnami" has been added to your repositories
```


**Step 6: Setup the user account along with Username and Password for WordPress access.**

Note: Since Wordpress is a CMS, so we need to have a user account to access it.

- **Create the below yaml file to setup WordPress user account along with Username and Password.**

```execute
cat <<'EOF' > wordpress-values.yaml
wordpressUsername: jhooq
wordpressPassword: jhooq
mariadb.rootUser.password: secretpassword,
mariadb.db.password: dbpassword,
allowEmptyPassword: false
wordpressEmail: contact@jhooq.com
wordpressFirstName: Sammy
wordpressLastName: Rocks
wordpressBlogName: Jhooq.com
service: 
  type: NodePort
EOF
```


Till this stage, we have completed all the pre-requisites for installing WordPress helm chart.

Now, execute the below command to install the chart.

```execute
helm install wordpress bitnami/wordpress --values=wordpress-values.yaml --namespace wordpress 
```

This should produce the output as below.

```
NAME: wordpress
LAST DEPLOYED: Tue Jan  5 21:53:30 2021
NAMESPACE: wordpress
STATUS: deployed
REVISION: 1
NOTES:
** Please be patient while the chart is being deployed **

Your WordPress site can be accessed through the following DNS name from within your cluster:

    wordpress.wordpress.svc.cluster.local (port 80)

To access your WordPress site from outside the cluster follow the steps below:

1. Get the WordPress URL by running these commands:

   export NODE_PORT=$(kubectl get --namespace wordpress -o jsonpath="{.spec.ports[0].nodePort}" services wordpress)
   export NODE_IP=$(kubectl get nodes --namespace wordpress -o jsonpath="{.items[0].status.addresses[0].address}")
   echo "WordPress URL: http://$NODE_IP:$NODE_PORT/"
   echo "WordPress Admin URL: http://$NODE_IP:$NODE_PORT/admin"
 2. Open a browser and access WordPress using the obtained URL.

3. Login with the following credentials below to see your blog:

  echo Username: jhooq
  echo Password: $(kubectl get secret --namespace wordpress wordpress -o jsonpath="{.data.wordpress-password}" | base64 --decode)
```

You have successfully installed the WordPress Helm Chart which you can use further to access a WordPress site.

### Conclusion

You have successfully installed the WordPress Helm Chart which you can use further to access a WordPress site.
