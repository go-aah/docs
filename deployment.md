Title: aah Deployment
Desc: Instructions for the aa application deployment.
Keywords: deployment, build, binary
---
# aah Deployment

Recommended way to deploy aah application is to create platform targeted binary. Use the build archive file on the server instead of `aah run`.

### Table of Contents

  * [Checklist](#checklist)
  * [How to: Docker](#how-to-docker)
  * Many more deployment options to come...

### Checklist

  * Ensure all the necessary configuration have been done for targeted profile (for e.g: `prod`)
      - **Note:** It is recommended to have sensitive configuration outside the version control.
          * You can use flag `-config /path/to/configfile.conf` on aah binary during a startup
          * You can use Environment variables
          * etc.
  * Ensure you have configured `prod.routes { ... }`
      - aah supports domains and sub-domains routing out-of-the-box. You have to define route `host` name in the environment profile. [Refer to below example](#sample-route-host-name-config-in-environment-profile).
  * Refer to [Cross Compile Build](aah-cli-tool.html#cross-compile-build) to create platform targeted build artifact.
  * Refer to [aah application binary](aah-application-binary.html) to know startup script.

Deploy the application.

### Sample Route Host name config in Environment profile

```cfg
env {
  prod {
    #...
    routes {
      domains {
        aahframework {
          host = "aahframework.org"

          # Define port no, if listening port different from serve port
          # For example:
          #   aahframework.org website is running on port 8080
          #   and get served on port 443 via nginx
          port = "443"  
        }

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

### How to: Docker

For now create your own docker file refer to [doc](https://blog.golang.org/docker) and deploy your aah application.

**`Upcoming`** auto generate docker file vis build process.

### Many more deployment options to come...

... **`Upcoming`**
