Title: Getting started with systemd for your aah application
Desc: Getting started with systemd for your aah application.
Keywords: systemd, service, aah, application
---
# Getting started with systemd

This document provides simple introduction of `systemd` and steps for your application to accomplish it.

`systemd` is an init system that provides many powerful features for starting, stopping, reload configuration and managing processes. Refer to [systemd man page](https://www.freedesktop.org/software/systemd/man/systemd.service.html).

`systemd` consists of two main concepts:

  * A unit is a configuration file that describes the properties of the process that you'd like to run.
  * A target is a grouping mechanism that allows systemd to start up groups of processes at the same time. This happens at every boot as processes are started at different run levels.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Ensure the rights are set correctly and provide ownership to the user you want to use. Here, I'll use the <code>aah</code> user account.</p>
</div>

**Table of Contents**

  * [Handy `generate` Command](#handy-generate-command)
  * [Steps to Configure and Enable](#steps-to-configure-and-enable)
  * [How to Start, Stop, Restart, Reload and Status](#how-to-start-stop-restart-reload-and-status)
  * [aahframework.org `systemd` service file](#aahframework-org-systemd-service-file)

## Handy `generate` Command

<span class="badge lb-sm">Since v0.10</span> aah CLI includes `generate` command to help developer in application development. To generate application `.service` file, use below command.

```bash
# run anywhere
aah generate script --name systemd --importpath github.com/user/appname

# OR if you're in app directory or path
aah generate script --name systemd

# OR short form
aah g s -n systemd
```

## Steps to Configure and Enable

  1. Place your service file at `/etc/systemd/system/`. For e.g.: `aahwebsite.service`
  2. We are gonna reload it, execute `sudo systemctl daemon-reload`.
  3. Enable `aahwebsite.service`, execute `sudo systemctl enable aahwebsite.service`.
  4. Congrats, you have successfully created and enabled your application service.

## How to Start, Stop, Restart, Reload and Status

Now let's see how to utilize the service file. Don't forget to replace your service file/name with below example commands :).

#### Start your application
```cfg
sudo systemctl start aahwebsite.service

# OR
sudo systemctl start aahwebsite

# OR
sudo service aahwebsite start
```

#### Stop your application
```cfg
sudo systemctl stop aahwebsite.service

# OR
sudo systemctl stop aahwebsite

# OR
sudo service aahwebsite stop
```

#### Restart your application
```cfg
sudo systemctl restart aahwebsite.service

# OR
sudo systemctl restart aahwebsite

# OR
sudo service aahwebsite restart
```

#### Reload your application
<span class="badge lb-sm">Since v0.10</span> Reload functionality available.
```cfg
sudo systemctl reload aahwebsite.service

# OR
sudo systemctl reload aahwebsite

# OR
sudo service aahwebsite reload
```

#### Status of your application
```cfg
sudo systemctl status aahwebsite.service

# OR
sudo systemctl status aahwebsite

# OR
sudo service aahwebsite status
```

#### You will get similar output:
```cfg
● aahwebsite.service - aahframework.org
   Loaded: loaded (/etc/systemd/system/aahwebsite.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2017-09-22 17:05:20 PDT; 6s ago
  Process: 16623 ExecStart=/home/aah/website/aah.sh start (code=exited, status=0/SUCCESS)
 Main PID: 16629 (aahwebsite)
    Tasks: 6
   Memory: 8.4M
      CPU: 748ms
   CGroup: /system.slice/aahwebsite.service
           └─16629 /home/aah/website/bin/aahwebsite -profile=prod

Sep 22 17:05:20 cn01-sfo.aahframework.org systemd[1]: Starting aahframework.org...
Sep 22 17:05:20 cn01-sfo.aahframework.org aah.sh[16623]: aahwebsite started.
Sep 22 17:05:20 cn01-sfo.aahframework.org systemd[1]: Started aahframework.org.
```

## aahframework.org `systemd` service file

For your reference. This is `aahframework.org` website service file. aah application deployed/extracted at location `/home/aah/website`.

```cfg
[Unit]
Description=aahframework.org
After=network.target

[Service]
EnvironmentFile=/home/aah/website/env_values
User=aah
Group=aah
Type=forking
ExecStart=/home/aah/website/aah.sh start
ExecStop=/home/aah/website/aah.sh stop
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
