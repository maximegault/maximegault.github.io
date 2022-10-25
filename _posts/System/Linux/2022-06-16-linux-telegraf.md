---
title: Telegraf
date: 2022-06-16 09:10:00 HH:MM:SS +0200
categories: [Telegraf]
tags: [Telegraf, tips]
---

## Telegraf

### Global configuration

By default, file `/etc/telegraf/telegraf.conf`:

```ini
# Custom global tags (will be sent with each metric)
[global_tags]
    env = "myEnv"
    environment = "DEVELOPPEMENT"
    etat = "Installed"

[agent]
    # Default data collection interval for all inputs
    interval = "10s"
    # Rounds collection interval to interval. For example, if interval is set to 10s then always collect on :00, :10, :20, etc.
    round_interval = true
    # Telegraf will send metrics to output in batch of at most metric_batch_size metrics - Maximum number of metrics to send at once
    metric_batch_size = 10000
    # Telegraf will cache metric_buffer_limit metrics for each output, and will flush this buffer on a successful write. This should be a multiple of metric_batch_size and could not be less than 2 times metric_batch_size - Maximum number of unsent metrics to buffer
    metric_buffer_limit = 10000
    # Collection jitter is used to jitter the collection by a random amount. Each plugin will sleep for a random time within jitter before collecting. This can be used to avoid many plugins querying things like sysfs at the same time, which can have a measurable effect on the system
    collection_jitter = "0s"
    # Default data flushing interval for all outputs. You should not set this below interval. Maximum flush_interval will be flush_interval + flush_jitter
    flush_interval = "60m"
    # Jitter the flush interval by a random amount. This is primarily to avoid large write spikes for users running a large number of Telegraf instances. For example, a flush_jitter of 5s and flush_interval of 10s means flushes will happen every 10-15s
    flush_jitter = "60m"
    # Collected metrics are rounded to the precision specified as an interval (integer + unit, ex: 1ns, 1us, 1ms, and 1s . Precision will NOT be used for service inputs, such as logparser and statsd
    precision = ""
    # Run Telegraf in debug mode
    debug = false
    # Run Telegraf in quiet mode (error messages only)
    quiet = true
    # Specify the log file name. The empty string means to log to stderr
    logfile = "/dev/null"
    # If true, do no set the host tag in the Telegraf agent
    omit_hostname = false
```

### Configuration files directory

By default, directory `/etc/telegraf/telegraf.d`: telegraf reads each `.conf` file and concatenates everything to make a big unique configuration file for him.

### Test configuration

With the `telegraf -test` command:

```bash
 telegraf -test -config /etc/telegraf/telegraf.conf -config-directory /etc/telegraf/telegraf.d --input-filter=exec
```

It's possible to filter on a given input type, ie. `exec`:

```bash
 telegraf -test -config /etc/telegraf/telegraf.conf -config-directory /etc/telegraf/telegraf.d --input-filter=exec
```

Or an output type, ie. `dynatrace`:

```bash
 telegraf -test -config /etc/telegraf/telegraf.conf -config-directory /etc/telegraf/telegraf.d --output-filter=dynatrace
```

Filters can be combined (it performs an or between different filters):

```bash
 telegraf -test -config /etc/telegraf/telegraf.conf -config-directory /etc/telegraf/telegraf.d --input-filter=exec --output-filter=dynatrace
```

[Source](https://docs.influxdata.com/telegraf/v1.21/administration/configuration/)
