Title: aah Deployment
Desc: Instructions for the aa application deployment.
Keywords: deployment, build, binary
---
# aah Deployment

Recommended way to deploy aah application is to create platform targeted binary. Use the build archive file on the server.

  * Ensure all the necessary configuration have been done for targeted profile (for e.g: `prod`).
      - aah supports domains and sub-domains routing out-of-the-box. You have to define route `host` name in the environment profile. [Refer to below example](#sample-route-host-name-config-in-environment-profile).
      - **Note:** It is recommend to have sensitive configuration outside version control. Use aah provided external configuration loading, Environment variables, etc.
  * Refer [Cross Compile Build](aah-cli-tool.html#cross-compile-build) to create platform targeted build artifact.
  * Refer [aah application binary](aah-application-binary.html) to know startup script.

Deploy the application.

### Sample Route Host name config in Environment profile

```bash
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
