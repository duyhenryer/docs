# Logging

!!! note "Work in progress"
    This document is a work in progress.


## Understanding Docker Logging and Log Files

Docker Logging Commands

`docker logs` is a command that shows all the information logged by a running container. 
The docker service logs command shows information logged by all the containers participating in a service.
 By default, the output of these commands, as it would appear if you run the command in a terminal, 
 opens up three I/O streams: `stdin, stdout, and stderr`. And the default is set to show only `stdout` and `stdout`.

* `stdin` is the command’s input stream, which may include input from the keyboard or input from another command.
* `stdout` is usually a command’s normal output.
* `stderr` is typically used to output error messages.

The docker logs command may not be useful in cases when a logging driver is configured to send logs to a file, 
database, or an external host/backend, or if the image is configured to send logs to a file instead of stdout and stderr. 
With `docker logs <CONTAINER_ID>`, you can see all the logs broadcast by a specific container identified by a unique ID

```
$ docker run --name test -d busybox sh -c "while true; do $(echo date); sleep 1; done"
$ date
Tue 06 Feb 2020 00:00:00 UTC
$ docker logs -f --until=2s test
Tue Aug 24 13:41:55 UTC 2021
Tue Aug 24 13:41:56 UTC 2021
Tue Aug 24 13:41:57 UTC 2021
Tue Aug 24 13:41:58 UTC 2021
Tue Aug 24 13:41:59 UTC 2021
```

`docker logs` works a bit differently in community and enterprise versions. In Docker Enterprise, 
`docker logs` read logs created by any logging driver, but in Docker CE, it can only read logs created by the `json-file`, local, and `journald` drivers.

For example, here’s the JSON log created by the hello-world Docker image using the json-file driver:

```
{"log":"Hello from Docker!\n","stream":"stdout","time":"2021-02-10T00:00:00.000000000Z"}
```
As you can see, the log follows a pattern of printing:

* Log’s origin
* Either stdout or stderr
* A timestamp
You can find this log in your Docker host at:
```
/var/lib/docker/containers/<container id>/<container id>-json.log  
```

## Logging Drivers Supported by Docker

Logging drivers, or log-drivers, are mechanisms for getting information from running containers and services, 
and Docker has lots of them available for you. Every Docker daemon has a default logging driver,
 which each container uses. Docker uses the json-file logging driver as its default driver.

 Currently, Docker supports the following logging drivers:

| Driver      | Description |
| :---        |    :----   |
| none       | No logs are available for the container and Docker logs do not return any output. |
| local      | Logs are stored in a custom format designed for minimal overhead.                 |
| json-file  | Logs are formatted as JSON. The default logging driver for Docker.                |
| syslog     | Writes logging messages to the Syslog facility. The Syslog daemon must be running on the host machine.|
| journald   | Writes log messages to journald. The journald daemon must be running on the host machine.|
| gcplogs    | Writes log messages to Google Cloud Platform (GCP) logging. |
| awslogs    | Writes log messages to Amazon CloudWatch logs. |
| splunk     | Writes log messages to Splunk using the HTTP Event Collector. |
| gelf       | Writes log messages to a Graylog Extended Log Format (GELF) endpoint, such as Graylog or Logstash. |
| fluentd    | Writes log messages to fluentd (forward input). The fluentd daemon must be running on the host machine. |
| etwlogs    | Writes log messages as Event Tracing for Windows (ETW) events. Only available on Windows platforms. |
| logentries | Writes log messages to Rapid7 Logentries. |


## Configuring the Logging Driver
To configure the Docker daemon to a logging driver, you need to set the value of log-driver to the name 
of the logging driver in the daemon.json configuration file. Then you need to restart Docker for
the changes to take effect for all the newly created containers. All the existing containers will remain as they are.

For example, let’s set up a default logging driver with some additional options.
```
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "25m",
    "max-file": "10",
    "labels": "production_bind",
    "env": "os,customer"
  }
}
```

To find the current logging driver for the Docker daemon:

```
$ docker info --format '{{.LoggingDriver}}'

json-file
```

## docker-compose with logging

```
version: '3'

services:
    logger:
        image: nginx
        container_name: "logger"
        hostname: "logger"
        restart: always

        logging:
          driver: json-file
          options:
              max-size: "10m"
              max-file: "5"

        environment:
            - LOG_FILES=true
            - LOG_SYSLOG=false
            - MAX_FILES=10
            - MAX_SIZE=50
            - MAX_AGE=20
            - DEBUG=false

        volumes:
            - ./logs:/srv/logs
```



https://earthly.dev/blog/understanding-docker-logging-and-log-files/
