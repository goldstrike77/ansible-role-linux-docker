![](https://img.shields.io/badge/Ansible-docker-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_docker.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Docker Versions](#docker-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
Docker is a set of platform as a service product that uses OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries and configuration files, they can communicate with each other through well-defined channels. Docker enables developers to package applications into containers-standardized executable components that combine application source code with all the operating system (OS) libraries and dependencies required to run the code in any environment. While developers can create containers without Docker, Docker makes it easier, simpler, and safer to build, deploy, and manage containers. It’s essentially a toolkit that enables developers to build, deploy, run, update, and stop containers using simple commands and work-saving automation. This Ansible role installs Docker on linux operating system, including establishing a filesystem structure and server configuration with some common operational features.

## Requirements
### Operating systems
This role will work on the following operating systems:

  * CentOS 7

### docker versions

The following list of supported the docker releases:

* Docker 18.0x.x+

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:
##### General parameters
* `docker_edition`: Specify the Docker edition.
* `docker_version`: Specify the Docker version.
* `docker_channel`: Define Docker distribution.
* `docker_native_cgroupdriver`: Specifies the management of the container's cgroups, cgroupfs or systemd.
* `docker_path`: Specify the Docker data folder.
* `docker_bip`: Specify network bridge IP.
* `docker_registry_mirrors`: Registry mirrors.
* `docker_proxy_server`: Proxy server.
* `docker_dns`: DNS server.
* `docker_insecure_registries`: Configures Docker to entirely disregard security for registry.

##### Syslog parameters
* `syslog`: A boolean value,  Enable or Disable send console and access log to remote Syslog server.
* `syslog_port`: Syslog server port.
* `syslog_protocol`: Syslog server protocol.
* `syslog_server`: List of syslog server list.


##### Docker System Variables
* `docker_arg.max_concurrent_downloads`: Set the max concurrent downloads for each pull.
* `docker_arg.max_concurrent_uploads`: Set the max concurrent uploads for each push.
* `docker_arg.icc`: Enable or disable inter-container communication.
* `docker_arg.live_restore`: Live restore of docker when containers are still running.
* `docker_arg.log_level`: Set the logging level.
* `docker_arg.userland_proxy`: Userland proxy for loopback traffic.
* `docker_arg.prune_interval`: Docker system prune job interval.
* `docker_arg.prune_keep`: Only remove unused containers, images, and networks created before given time.
* `docker_arg.storage_driver`: Storage driver to use.
* `docker_arg.storage_opts`: Options per storage driver.

##### Listen port
* `docker_port.metrics_port`: Prometheus exporter port

##### Service Mesh
* `environments`: Define the service environment.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
- Ansible versions >= 2.8
- Python >= 2.7.5

## Example

### Hosts inventory file
See tests/inventory for an example.

    node01 ansible_host='192.168.1.10' docker_version='18.09.9'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: all
  roles:
     - role: ansible-role-linux-docker
       docker_version: '18.09.9'
```

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

```yaml
docker_edition: 'ce'
docker_version: '18.09.9'
docker_channel: 'stable'
docker_native_cgroupdriver: 'systemd'
docker_path: '/data'
docker_bip: '172.17.0.1/16'
docker_registry_mirrors:
  - 'http://dockerhub.azk8s.cn'
  - 'http://reg-mirror.qiniu.com'
docker_dns:
  - '223.5.5.5'
  - '119.29.29.29'
  - '8.8.8.8'
docker_insecure_registries:
  - 'https://gcr.azk8s.cn'
  - 'https://sharedprdpaasacr0.azurecr.cn'
syslog: false
syslog_port: '12201'
syslog_protocol: 'udp'
syslog_server:
  - '127.0.0.1'
docker_arg:
  max_concurrent_downloads: '3'
  max_concurrent_uploads: '5'
  icc: false
  live_restore: true
  log_level: 'info'
  userland_proxy: false
  prune_interval: 'monthly'
  prune_keep: '720h'
  storage_driver: 'overlay2'
  storage_opts:
    overlay2_override_kernel_check: true
docker_port:
  metrics_port: '9323'
docker_compose:
  install: true
  version: '1.25.5'
  path: '/usr/local/bin/docker-compose'
environments: 'Development'
tags:
  subscription: 'default'
  owner: 'nobody'
  department: 'Infrastructure'
  organization: 'The Company'
  region: 'IDC01'
exporter_is_install: false
consul_public_register: false
consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
consul_public_http_prot: 'https'
consul_public_http_port: '8500'
consul_public_clients:
  - '127.0.0.1'
```

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
