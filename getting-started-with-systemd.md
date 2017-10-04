Title: Getting started with systemd for your aah application
Desc: Getting started with systemd for your aaah application.
Keywords: systemd, service, aah, application
---
# Getting started with systemd

This document describes the simple intro of `systemd`, sample systemd service file for your aah application and steps to accomplish it.

`systemd` is an init system that provides many powerful features for starting, stopping, reload configuration and managing processes. Refer to [systemd man page](https://www.freedesktop.org/software/systemd/man/systemd.service.html).

systemd consists of two main concepts:

  * A unit is a configuration file that describes the properties of the process that you'd like to run.
  * A target is a grouping mechanism that allows systemd to start up groups of processes at the same time. This happens at every boot as processes are started at different run levels.

**Table of Contents**

  * [Sample `systemd` service file](#sample-systemd-service-file)
  * [Steps to Configure and Enable](#steps-to-configure-and-enable)
  * [How to Start, Stop, Restart and Status](#how-to-start-stop-restart-and-status)


## Sample `systemd` service file

This is `aahframework.org` website service file. aah application build archive extracted at location `/home/aah/website`.

```cfg
[Unit]
Description=aahframework.org
After=network.target

[Service]
EnvironmentFile=/home/aah/env_values
User=aah
Group=aah
Type=forking
ExecStart=/home/aah/website/aah.sh start
ExecStop=/home/aah/website/aah.sh stop
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## Steps to Configure and Enable

  * **Step 1:** create a service file at `/etc/systemd/system/aahwebsite.service` with above reference for your application.
  * **Step 2:** we are gonna reload it `sudo systemctl daemon-reload`.
  * **Step 3:** to enable `aahwebsite.service` file; execute `sudo systemctl enable aahwebsite.service`.
  * **Step 4:** congrats, you have successfully created service file.

## How to Start, Stop, Restart and Status

We have successfully created a service file, let's see how to run it. Don't forget to replace your service filename with below example commands.

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

#### Status of your application
```cfg
sudo systemctl status aahwebsite.service

# OR
sudo systemctl status aahwebsite

# OR
sudo service aahwebsite status
```
<br>
You will get similar output:
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
