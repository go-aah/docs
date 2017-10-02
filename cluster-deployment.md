Title: Cluster Deployment
Desc: aah cluster deployment of Web application and API application.
Keywords: cluster deployment, cluster, deployment, web, api
---
# aah Cluster Deployment

This document provides aah cluster deployment references for Web and RESTful API.

  * [Web Application Cluster](#web-application-cluster)
  * [RESTful API Cluster](#restful-api-cluster)

## Web Application Cluster

`aah` web application cluster deployment is typically same as industry known way. Deploy as many nodes as required by your traffic, high availability, etc.

Typically application configuration is same in all nodes in the cluster except node specific config values. For example: `instance_name`. Following checklist is to provide key configuration reference:

  * **Instance Name:** Provide aah `instance_name` config value via Environment Variable or External Config file. `instance_name` value is important one; it helps you to distinguish application log from various nodes in the aah cluster.
      * Let's say your web application name is "User Portal". For example: `instance_name = "sfo-up-web-01"`, `instance_name = "sfo-up-web-02"`. Refer to [logging](logging.html#understanding-aah-logging).
  * **Session Management:** Ensure section `security.session { ... }` config values from `security.conf` is same in all nodes in the cluster to process user session. For example: `security.session.sign_key`, `security.session.enc_key`, etc.
  * **Anti-CSRF:** Ensure section `security.anti_csrf { ... }` config values from `security.conf` is same in all nodes in the cluster to prevent CSRF attacks properly. For example: `security.anti_csrf.sign_key`, `security.anti_csrf.enc_key`, etc.


## RESTful API Cluster

`aah` RESTful API cluster deployment is typically same as industry known way. Deploy as many nodes as required by your traffic, high availability, etc.

Typically application configuration is same in all nodes in the cluster except node specific config values. For example: `instance_name`.

  * **Instance Name:** Provide aah `instance_name` config value via Environment Variable or External Config file. `instance_name` value is important one; it helps you to distinguish application log from various nodes in the aah cluster.
      * Let's say your API service name is "User Subscription Service". For example: `instance_name = "sfo-uss-api-01"`, `instance_name = "sfo-uss-api-02"`. Refer to [logging](logging.html#understanding-aah-logging).
