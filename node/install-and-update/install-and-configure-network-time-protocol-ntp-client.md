# Install & Configure Network Time Protocol (NTP) Client

In this guide you will learn how to:

* Install NTP
* Configure NTP

## Overview

NTP is a networking protocol designed to synchronize the clocks of computers over a network. NTP allows you to synchronize the clocks of all the systems within the network. Currently, it is required that validators' local clocks stay reasonably in sync, so you should be running NTP or a similar service. You can check whether you have the NTP client by running:

## Linux

If you are using Ubuntu 18.04 / 19.04, NTP Client should be installed by default.

```
timedatectl
```

If NTP is installed and running, you should see System clock synchronized: `yes` (or a similar message). If you do not see it, you can install it by executing:

```
sudo apt-get install ntp
```

`ntpd` will be started automatically after install. You can query `ntpd` for status information to verify that everything is working:

```
sudo ntpq -p
```
