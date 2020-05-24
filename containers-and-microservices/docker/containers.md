---
description: Docker containers
---

# Containers

### Commands

#### Run container

```text
container run - p 80:80 -d --name webserver nginx
```

pass "`--rm`"  to remove container when it stops.

#### List all containers \(pass -a to see all containers\)

```text
docker container ls [-a]
```

#### Get logs from container

```text
docker container logs <container-name>
```

#### List running processes in the container

```text
docker container top <container-name>
```

#### Get system information about running containers

```text
docker container stats
```

#### Get metadata about running container

```text
docker container inspect <container-name>
```

Example:

```javascript
[
  {
    "Id": "2471fee7e410a4ba48e69e80ecdc7033f46f7012ca493d1dba92dc9a6fda46f7",
    "Created": "2020-01-13T01:27:41.8547458Z",
    "Path": "nginx",
    "Args": [
      "-g",
      "daemon off;"
    ],
    "State": {
      "Status": "running",
      "Running": true,
      "Paused": false,
      "Restarting": false,
      "OOMKilled": false,
      "Dead": false,
      "Pid": 2513,
      "ExitCode": 0,
      "Error": "",
      "StartedAt": "2020-01-13T01:27:42.7242686Z",
      "FinishedAt": "0001-01-01T00:00:00Z"
    },
    "Image": "sha256:c7460dfcab502275e9c842588df406444069c00a48d9a995619c243079a4c2f7",
    "ResolvConfPath": "/var/lib/docker/containers/2471fee7e410a4ba48e69e80ecdc7033f46f7012ca493d1dba92dc9a6fda46f7/resolv.conf",
    "HostnamePath": "/var/lib/docker/containers/2471fee7e410a4ba48e69e80ecdc7033f46f7012ca493d1dba92dc9a6fda46f7/hostname",
    "HostsPath": "/var/lib/docker/containers/2471fee7e410a4ba48e69e80ecdc7033f46f7012ca493d1dba92dc9a6fda46f7/hosts",
    "LogPath": "/var/lib/docker/containers/2471fee7e410a4ba48e69e80ecdc7033f46f7012ca493d1dba92dc9a6fda46f7/2471fee7e410a4ba48e69e80ecdc7033f46f7012ca493d1dba92dc9a6fda46f7-json.log",
    "Name": "/webserver",
    "RestartCount": 0,
    "Driver": "overlay2",
    "Platform": "linux",
    "MountLabel": "",
    "ProcessLabel": "",
    "AppArmorProfile": "",
    "ExecIDs": null,
    "HostConfig": {
      "Binds": null,
      "ContainerIDFile": "",
      "LogConfig": {
        "Type": "json-file",
        "Config": {}
      },
      "NetworkMode": "default",
      "PortBindings": {
        "80/tcp": [
          {
            "HostIp": "",
            "HostPort": "80"
          }
        ]
      },
      "RestartPolicy": {
        "Name": "no",
        "MaximumRetryCount": 0
      },
      "AutoRemove": false,
      "VolumeDriver": "",
      "VolumesFrom": null,
      "CapAdd": null,
      "CapDrop": null,
      "Capabilities": null,
      "Dns": [],
      "DnsOptions": [],
      "DnsSearch": [],
      "ExtraHosts": null,
      "GroupAdd": null,
      "IpcMode": "private",
      "Cgroup": "",
      "Links": null,
      "OomScoreAdj": 0,
      "PidMode": "",
      "Privileged": false,
      "PublishAllPorts": false,
      "ReadonlyRootfs": false,
      "SecurityOpt": null,
      "UTSMode": "",
      "UsernsMode": "",
      "ShmSize": 67108864,
      "Runtime": "runc",
      "ConsoleSize": [
        0,
        0
      ],
      "Isolation": "",
      "CpuShares": 0,
      "Memory": 0,
      "NanoCpus": 0,
      "CgroupParent": "",
      "BlkioWeight": 0,
      "BlkioWeightDevice": [],
      "BlkioDeviceReadBps": null,
      "BlkioDeviceWriteBps": null,
      "BlkioDeviceReadIOps": null,
      "BlkioDeviceWriteIOps": null,
      "CpuPeriod": 0,
      "CpuQuota": 0,
      "CpuRealtimePeriod": 0,
      "CpuRealtimeRuntime": 0,
      "CpusetCpus": "",
      "CpusetMems": "",
      "Devices": [],
      "DeviceCgroupRules": null,
      "DeviceRequests": null,
      "KernelMemory": 0,
      "KernelMemoryTCP": 0,
      "MemoryReservation": 0,
      "MemorySwap": 0,
      "MemorySwappiness": null,
      "OomKillDisable": false,
      "PidsLimit": null,
      "Ulimits": null,
      "CpuCount": 0,
      "CpuPercent": 0,
      "IOMaximumIOps": 0,
      "IOMaximumBandwidth": 0,
      "MaskedPaths": [
        "/proc/asound",
        "/proc/acpi",
        "/proc/kcore",
        "/proc/keys",
        "/proc/latency_stats",
        "/proc/timer_list",
        "/proc/timer_stats",
        "/proc/sched_debug",
        "/proc/scsi",
        "/sys/firmware"
      ],
      "ReadonlyPaths": [
        "/proc/bus",
        "/proc/fs",
        "/proc/irq",
        "/proc/sys",
        "/proc/sysrq-trigger"
      ]
    },
    "GraphDriver": {
      "Data": {
        "LowerDir": "/var/lib/docker/overlay2/a4d587b17605cd7d55e5e4e53ca94f85eb3873717774c7ccaf2ffdbfda7ede6f-init/diff:/var/lib/docker/overlay2/b0b1f6dce35e8f037a3f0bd7310c9d262bfbe50bf2a99d8d1d5bb9a8c2d674e7/diff:/var/lib/docker/overlay2/d2a2f0e8c23a861795bcab3d13f17ca643c17a9b97c74c85696393828186cb1a/diff:/var/lib/docker/overlay2/a20f4c7748e1f828f0d533783d04930c7f16d6435dbebe4b093dd7c05488b666/diff",
        "MergedDir": "/var/lib/docker/overlay2/a4d587b17605cd7d55e5e4e53ca94f85eb3873717774c7ccaf2ffdbfda7ede6f/merged",
        "UpperDir": "/var/lib/docker/overlay2/a4d587b17605cd7d55e5e4e53ca94f85eb3873717774c7ccaf2ffdbfda7ede6f/diff",
        "WorkDir": "/var/lib/docker/overlay2/a4d587b17605cd7d55e5e4e53ca94f85eb3873717774c7ccaf2ffdbfda7ede6f/work"
      },
      "Name": "overlay2"
    },
    "Mounts": [],
    "Config": {
      "Hostname": "2471fee7e410",
      "Domainname": "",
      "User": "",
      "AttachStdin": false,
      "AttachStdout": false,
      "AttachStderr": false,
      "ExposedPorts": {
        "80/tcp": {}
      },
      "Tty": false,
      "OpenStdin": false,
      "StdinOnce": false,
      "Env": [
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
        "NGINX_VERSION=1.17.7",
        "NJS_VERSION=0.3.7",
        "PKG_RELEASE=1~buster"
      ],
      "Cmd": [
        "nginx",
        "-g",
        "daemon off;"
      ],
      "Image": "nginx",
      "Volumes": null,
      "WorkingDir": "",
      "Entrypoint": null,
      "OnBuild": null,
      "Labels": {
        "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
      },
      "StopSignal": "SIGTERM"
    },
    "NetworkSettings": {
      "Bridge": "",
      "SandboxID": "dac7e1d502934740d4bdcedf1756d75514e4a5387bae842a6770026886d9eeaf",
      "HairpinMode": false,
      "LinkLocalIPv6Address": "",
      "LinkLocalIPv6PrefixLen": 0,
      "Ports": {
        "80/tcp": [
          {
            "HostIp": "0.0.0.0",
            "HostPort": "80"
          }
        ]
      },
      "SandboxKey": "/var/run/docker/netns/dac7e1d50293",
      "SecondaryIPAddresses": null,
      "SecondaryIPv6Addresses": null,
      "EndpointID": "d3731f0e83b1ee4bc67f9221c0056e2eef91b8e3ea2e86636765bbad13b35b9f",
      "Gateway": "172.17.0.1",
      "GlobalIPv6Address": "",
      "GlobalIPv6PrefixLen": 0,
      "IPAddress": "172.17.0.2",
      "IPPrefixLen": 16,
      "IPv6Gateway": "",
      "MacAddress": "02:42:ac:11:00:02",
      "Networks": {
        "bridge": {
          "IPAMConfig": null,
          "Links": null,
          "Aliases": null,
          "NetworkID": "45ecc9dd0aa8816755250010c1b01d899e2de4700db943e6ed8d6224c56bf9e7",
          "EndpointID": "d3731f0e83b1ee4bc67f9221c0056e2eef91b8e3ea2e86636765bbad13b35b9f",
          "Gateway": "172.17.0.1",
          "IPAddress": "172.17.0.2",
          "IPPrefixLen": 16,
          "IPv6Gateway": "",
          "GlobalIPv6Address": "",
          "GlobalIPv6PrefixLen": 0,
          "MacAddress": "02:42:ac:11:00:02",
          "DriverOpts": null
        }
      }
    }
  }
]
```

#### Stop running container

```bash
docker container stop <container-name|container-id>
```

#### Remove container

 \(pass -f to force remove container\)

```bash
docker container [-f] rm <container-name|container-id>
```

#### Start container with different process then defined in CMD or ENTRYPOINT

```bash
docker container run -it --name webserver nginx bash
```

will start "bash" in interactive mode in "nginx" container named "webserver". Once connected, a "ps" can be installed to make sure the only process running is "bash"

```bash
apt-get update
apt-get install -y procps
ps aux
```

#### Connect to the running docker container and run shell

 Connect to running container with shell \(depends whether specific shell is installed\)

```bash
docker container exec -it <container-name|id> bash
```

#### List container port mappings:

```bash
docker container port <container-name|container-id>
```

if container has a host to container port mappings, command should produce something like:  
`80/tcp -> 0.0.0.0:80`



