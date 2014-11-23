# Ansible Go Continuous Delivery Agent Role

## Description

*ansible-gocd-agent* is an [Ansible](http://ansible.com) role.
Use this role to install Go Continuous Delivery Agent.

## Provides

1. Latest Go Continuous Delivery agent with Docker, Maven, Scala, Gradle, Sbt, NodeJS and Ant.

## Requires

1. Ansible 1.7 o higher
2. Ubuntu Server 14.04
3. Vagrant (optional)

## Usage

### Get the code

```bash
$ git clone git@github.com:codingbunch/ansible-gocd-agent.git
```

The code should reside in the roles directory of ansible ( See [ansible documentation](http://docs.ansible.com/playbooks.html#roles) for more information on roles ), in a folder named gocd-agent.

### Create a host file

Following example make ansible aware of the Vagrant box reachable on localhost port 2222. Add a gocd-server group in order to register the agent to a Go server.

```bash
$ vi ansible.host
```

with

```ini
[gocd-agent]
127.0.0.1 ansible_ssh_port=2222

[gocd-server]
<server-ip-address>
```

### Create host specific variables

Make the host_vars directory where *ansible.host* file is located.

```bash
$ mkdir host_vars
```

Create a file in the newly created directory matching your host.

```bash
$ cd host_vars
$ vi 127.0.0.1
```

with

```yaml
---

timezone: Europe/Madrid

# NTP servers
ntp_server1: ntp0.ox.ac.uk
ntp_server2: ntp1.ox.ac.uk
ntp_server3: ntp2.ox.ac.uk
ntp_server4: ntp3.ox.ac.uk
ntp_server5: 0.uk.pool.ntp.org
ntp_server6: 1.uk.pool.ntp.org
ntp_server7: ntp.ubuntu.com # fallback

# Scala
scala_version: 2.10.4

# Maven
maven_version: 3.2.3

# Ant
ant_version: 1.9.4

# Gradle
gradle_version: 2.2

# SBT
sbt_version: 0.13.6

# NodeJs
nodejs_version: v0.10.33
nodejs_global_packages:
  - nodemon
  - debug
  - foreman

# GOCD
gocd_version: 14.3.0-1186

```

### Run the playbook

First create a playbook including the gocd-agent role, naming it gocd.yml

```yml
- name: gocd
  hosts: gocd-agent
  roles:
    - gocd-agent
```

Use *ansible.host* as inventory. Run the playbook only for the remote host *gocd-agent*. Use *vagrant* as the SSH user to connect to the remote host. *-k* enables the SSH password prompt.

```bash
$ ansible-playbook -k -i ansible.host gocd.yml -u vagrant
```
