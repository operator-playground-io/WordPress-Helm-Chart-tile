<h1 align="center">WordPress</h1>

![Logo](_images/logo.png)


### Overview:

WordPress is one of the most versatile open source content management systems. It is a high-performance, and easy-to-manage web publishing platform you can use to build your websites, blogs, or apps. WordPress is scalable to many servers and can be integrated easily via REST, JSON, SOAP and other formats. It is built to offer powerful features and arrives with a whopping 15,000 plugins to extend your website functionalities. Power your place on the web with WordPress plugins and extensions that give you the advantage of galleries, forums, analytics, mailing lists, online store, and much more.


### Features of WordPress Helm Chart

Bitnami WordPress chart bootstraps a deployment on a cluster using the  package manager.
It also packages MariaDB deployment for the database requirements of the WordPress application.


In this tutorial, we’ll use Helm for setting up WordPress on top of a Kubernetes cluster, in order to create a highly-available website.
In addition to leveraging the intrinsic scalability and high availability aspects of Kubernetes, this setup will help keeping WordPress secure by providing simplified upgrade and rollback workflows via Helm.

### WordPress Architecture

In the below architectural diagram, a PHP application serves the incoming HTTP requests, while reads/writes are made to the file upload directory and queries are made against the backend MySQL database.

A high-level WordPress architectural flow for a multi-node installation of WordPress without Kubernetes or Portworx is shown below:

![](_images/wordpress-architecture.png)

### Objective of tutorial

In this tutorial,we are going to cover the following topics:

- Install Bitnami WordPress Helm Chart and verify its successful installation
- Verify the status of pods and services. 
- Describe configurable parameters for the WordPress Bitnami Helm Chart with their default values
- Access WordPress Console.
- Uninstall WordPress Helm Chart and release resources.





