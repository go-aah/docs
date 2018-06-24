Title: Cluster Deployment
Desc: aah cluster deployment of Web applications and API applications.
Keywords: cluster deployment, cluster, deployment, web, api
---
# aah Cluster Deployment

This document provides aah cluster deployment checklist for Web and RESTful API applications.

  * [Web Application Cluster](#web-application-cluster)
  * [RESTful API Cluster](#restful-api-cluster)

## Web Application Cluster

`aah` web application cluster deployment is the same as the industry known practice. Deploy as many nodes as required. The number of nodes varies depending on various factors such as traffic and high availability.

Typically, application configuration is the same for all nodes in the cluster except for node-specific config values; `instance_name`, as an example. The following checklist provides key configuration references:

  * **Instance Name:** Provide aah `instance_name` config value via Environment Variable or External Config file. `instance_name` value is an important one; it helps to distinguish application log from various nodes in the aah cluster.
      * As an illustration, an aah web application named "User Portal" (referred as 'up') deployed in SFO region with `n` nodes in a cluster, then the instance names will be `sfo-up-web-01`, `sfo-up-web-02`, `sfo-up-web-03`, ... `sfo-up-web-n`.
  * **Session Management:** In security.conf, ensure section `security.session { ... }` config values are same for all nodes in the cluster to process user session properly; notable configs are `sign_key` and `enc_key`.
  * **Anti-CSRF:** In security.conf, ensure section `security.anti_csrf { ... }` config values from `security.conf` is the same for all nodes in the cluster to prevent CSRF attacks properly; notable configs are `sign_key` and `enc_key`.


## RESTful API Cluster

`aah` RESTful API cluster deployment is also the same as the regular industry known practice. Deploy as many nodes as required. The number of nodes varies depending on various factors such as traffic and high availability.

Typically, application configuration is the same for all nodes in the cluster except for node-specific config values; `instance_name`, as an example.

  * **Instance Name:** Provide aah `instance_name` config value via Environment Variable or External Config file. `instance_name` value is an important one; it helps to distinguish application log from various nodes in the aah cluster.
      * As an illustration, an aah API service named "User Subscription Service" (referred as 'uss') deployed in SFO region with `n` nodes in a cluster, then the instance names will be `sfo-uss-api-01`, `sfo-uss-api-02`, `sfo-uss-api-03`, ... `sfo-uss-api-n`.
