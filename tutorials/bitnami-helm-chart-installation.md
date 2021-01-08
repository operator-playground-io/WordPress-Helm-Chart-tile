---
title: WordPress Bitnami Helm Chart Installation Tutorial
description: This tutorial explains how we Installed WordPress Bitnami Helm Chart
---

### Install WordPress Bitnami Helm Chart

This tutorial is made just to describe what all steps are actually being performed behind the WordPress Chart "Install" button and is just for knowledge perspective.
Please do not execute these manually if you already installed the WordPress Helm Chart using "Install" button. 

We have followed following steps to install WordPress Bitnami Helm Chart :

Step 1: Create a namespace : "wordpress". 

```
kubectl create namespace wordpress
```

Step 2: Create hostpath for both MariaDV PersistentVolume and WordPress PersistentVolume with root user and give full permission to /bitnami.

Commands :

```
sudo mkdir -p /bitnami/mariadb/data && sudo mkdir -p /bitnami/wordpress/wp-content && sudo chmod -R 777 /bitnami
```


By default, the WordPress chart installs MariaDB on a separate pod inside the cluster and uses it as the WordPress database.So for these two pods: MariaDB pod and WordPress Pod, we need to define PersistentVolumes.
   
Step 3:  Define the PersistentVolume for mariadb-pv where the mariadb data to be stored. The hostPath tells the mysql directory is in /bitnami/mariadb location

```
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


Command to create mariadb PersistentVolume :

```
kubectl create -f  mariadbpv.yaml
```


Step 4: Define the PersistentVolume for wordpress-pv where the wordpress site data to be stored. The hostPath tells the mysql directory is in /data location

```
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


Command to create wordpress PersistentVolume :

```
kubectl create -f  wordpresspv.yaml
```



Step 5: Add ‘bitnami' to your repo list:

Command:

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

you should see the following output:

```
"bitnami" has been added to your repositories
```


Step 6: Setup User account along with Username and Password for WordPress.

As Wordpress is CMS, so we need to have a user account to access it.

Created below yaml file to Setup WordPress User account along with Username and Password:

```
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


Now we have completed all the pre-requisites for the installation. 

Step 7: Command for WordPress Helm Chart installation:

```
helm install wordpress bitnami/wordpress --values=wordpress-values.yaml --namespace wordpress 
```

output:

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




