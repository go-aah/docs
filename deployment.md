Title: aah Deployment
Desc: Instructions for the aah application deployment.
Keywords: deployment, build, binary
---
# aah Deployment

This document provides an information of deployment checklist and steps involved to prepare aah application for production use.

Recommended way to deploy aah application is to create platform targeted binary. Use the build archive file on the server instead of `aah run`.

### Table of Contents

  * [Preparation Checklist](#preparation-checklist)
  * [Getting started with systemd](getting-started-with-systemd.html)
  * [Getting started with Docker](getting-started-with-docker.html)

### Preparation Checklist

  * Ensure all the necessary configuration have been done for targeted profile (for e.g: `prod`)
      - **Note:** It is recommended to keep sensitive details outside the application codebase.
          * Use flag `-config /path/to/configfile.conf` from aah binary during a startup
          * Use Environment variables
  * Add `prod.routes { ... }` into `prod.conf`. This step is **not applicable** if you have only one domain.
      - By nature of aah supports domains and sub-domains routing (aka domain routing) out-of-the-box. It is must to configure domain name and port information. You have to define route `host` name in the `prod` environment profile. [Refer to below example](#sample-route-host-name-config-in-prod-environment-profile).
  * Refer to [Cross Compile Build](aah-cli-tool.html#cross-compile-build) to create platform targeted build artifact.
  * Refer to [aah application binary](aah-application-binary.html) to know more about capabilities of binary.

Deploy the application.

### Sample Route Host name config in prod Environment profile

```bash
# This is configuration of aahframework.org and docs.aahframework.org websites.
#
env {
  prod {
    #...
    routes {
      domains {
        # this section name is same domain config name in the routes.conf
        aahframework {
          host = "aahframework.org"

          # Define port no, if listening port different from serve port
          # For example:
          #   aahframework.org website is running on port 8080
          #   and get served on port 443 via nginx
          port = "443"  
        }

        # this section name is same domain config name in the routes.conf
        docs_aahframework {
          host = "docs.aahframework.org"

          # Define port no, if listening port different from serve port
          # For example:
          #   docs.aahframework.org website is running on port 8080
          #   and get served on port 443 via nginx
          port = "443"
        }
      }
    }
    #...
  }
}
```
