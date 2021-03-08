---
title: Access WordPress admin console and website
description: Learn how to access WordPress admin console and website post chart installation.
---


### Access WordPress Console


To obtain the application URL, wait until the pods are running.


Your WordPress site can be accessed through the following DNS name from within your cluster:

```
 wordpress.wordpress.svc.cluster.local (port 80)
```

To access your WordPress site and dashboard from outside the cluster follow the steps below:


**Step 1: Get the WordPress URL by running the following commands:**


```execute
 export NODE_PORT=$(kubectl get --namespace wordpress -o jsonpath="{.spec.ports[0].nodePort}" services wordpress)
 export NODE_IP=$(kubectl get nodes --namespace wordpress -o jsonpath="{.items[0].status.addresses[0].address}")
```


**Step 2: Open your browser and access the WordPress site via retrieved URL by executing the command below.**

```execute
echo "WordPress URL: http://$NODE_IP:$NODE_PORT/"
```
You should see your WordPress site with the custom theme that you included in your image already activated. 

Please see the below snapshot to see how it looks like :

![](_images/wordpress-site.PNG)

**Step 3: Access your WordPress admin console by executing the command as below.**

```execute
echo "WordPress Admin URL: http://$NODE_IP:$NODE_PORT/admin"
```

**Step 4: Open the WordPress Admin URL in your browser. You will see the following login page.**

![](_images/login-console-final.PNG)

**Step 5: Run the command below and log in to WordPress admin console using the obtained credentials.**

```execute
echo Username: jhooq
echo Password: $(kubectl get secret --namespace wordpress wordpress -o jsonpath="{.data.wordpress-password}" | base64 --decode)
```

![](_images/console-admin-final.PNG)

On successful login, you should see the dashboard as depicted below.

![](_images/dashboard-wordpress.PNG)

**Step 6: Navigate to the "Plugins" section.**

Here, you will be able to see all the installed and activated plugins. See the sample below.

![](_images/plugins.PNG)


### Create Your First Post

Once you can access the dashboard, you can now add and publish a new post through the following steps.

**Step 1: Select the “Posts -> Add New” menu option to create a new post.**

  ![](_images/posts.png)

**Step 2: Add your new WordPress post.**

**Step 3: Enter a title and add the content to the post space.**

You can use the formatting tools at the top of the content area to format your post and add hyperlinks or images.

**Step 4: Choose the format and category for your post. (This step is optional)**

**Step 5: Publish the post to the site using the “Publish” button.**
   
   ![](_images/publish.png)


After completing the above steps, you should be able to see the new post on the blog ’s page of your site.



