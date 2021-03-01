---
title: Access WordPress site and WordPress Admin Console
description: Learn how to access WordPress site and WordPress Admin Console once chart installed successfully
---


### Access WordPress Console


To obtain the application URL, wait until the pods are running.


Your WordPress site can be accessed through the following DNS name from within your cluster:

```
 wordpress.wordpress.svc.cluster.local (port 80)
```

To access your WordPress site and Dashboard from outside the cluster follow the steps below:


1. Get the WordPress URL by running below commands:


```execute
 export NODE_PORT=$(kubectl get --namespace wordpress -o jsonpath="{.spec.ports[0].nodePort}" services wordpress)
 export NODE_IP=$(kubectl get nodes --namespace wordpress -o jsonpath="{.items[0].status.addresses[0].address}")
```


2. Open a browser and access WordPress Site using the obtained URL by executing below command:

```execute
echo "WordPress URL: http://$NODE_IP:$NODE_PORT/"
```
You should see your WordPress site with the custom theme that you have included in your image already activated. 

Please see the below snapshot how it looks like :

![](_images/wordpress-site.PNG)

3. Access WordPress Admin Console obtained by executing below command:

```execute
echo "WordPress Admin URL: http://$NODE_IP:$NODE_PORT/admin"
```
Open WordPress Admin Url in browser. You will see following login page :

![](_images/login-console-final.PNG)

- Log in to WordPress admin login console with the credentials obtained from running below commands:

```execute
echo Username: jhooq
echo Password: $(kubectl get secret --namespace wordpress wordpress -o jsonpath="{.data.wordpress-password}" | base64 --decode)
```

![](_images/console-admin-final.PNG)

On successful login you will be see the following dashboard:

![](_images/dashboard-wordpress.PNG)

Navigate to the "Plugins" section.You can see all the plugins installed and activated as in below snapshot:

![](_images/plugins.PNG)


### Create Your First Post

You can now add a new post using the following steps:

1. Select the “Posts -> Add New” menu option to create a new post.

  ![](_images/posts.png)

2. Add new WordPress post

3. Enter a title and content for the post. You can use the formatting tools at the top of the content area to format your post and add hyperlinks or images.

4. Optionally, choose the format and category for your post.

5. Publish it immediately using the “Publish” button.
   
   ![](_images/publish.png)


And now, when you visit your blog’s front page, you should see your new post.



