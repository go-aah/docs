Title: Tutorial of Domain, Subdomain and Wildcard Subdomain
Desc: Tutorial application that demonstrates, how to use Domain, Subdomain and Wildcard Subdomain using aah framework.
Keywords: tutorial, domain, Subdomain, wildcard subdomain, aah framework
---
# Tutorial - Domain, Subdomain and Wildcard Subdomain

Goal of this tutorial application is to give you an idea how to use Domain, Subdomain and Wildcard Subdomain using aah framework. Reference to [Routes Config](/routes-config.html).

### How to get the aah tutorials source code?

```bash
go get -u -d github.com/go-aah/tutorials/...
```

Just focus on following files:

  * `domain-subdomain/config/routes.conf`
  * `domain-subdomain/app/controllers/wildcard/app.go`
  * `domain-subdomain/app/controllers/admin/app.go`

### Let's see it in the action

#### Configuring local DNS mapping
First we have to configure your hosts file. Purpose is do local DNS mapping for `sample.com`. This step is not applicable for production, typically these settings happens in your domain DNS manager. Learn more about [Wildcard DNS - wikipedia](https://en.wikipedia.org/wiki/Wildcard_DNS_record)

This [rackspace article](https://support.rackspace.com/how-to/modify-your-hosts-file/) covers the steps to modify-your-hosts-file for Mac, Linux and Windows.

```bash
127.0.0.1       sample.com admin.sample.com username1.sample.com username2.sample.com
```

#### Running the tutorial application
```
aah run -ip=github.com/go-aah/tutorials/domain-subdomain
```

#### Now visit these URL's in your browser :)

* http://sample.com:8080
* http://admin.sample.com:8080
* http://username1.sample.com:8080
* http://username2.sample.com:8080
