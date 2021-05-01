# Ansible role for pihole
This role deploys and configures pihole to run in a docker
container.

Features:
- pihole in docker container
- docker container runs in bridge network mode (default, more secure) or host network mode
- services port 53 on your host
- ipv6 support (if your docker is configured for ipv6)

# Requirements
Provisioning host:
- ansible 2.9 or later

Host that will run pihole:
- Ubuntu 18.04 or later
- docker
 - An ansible role for installing docker is available from https://github.com/elgeeko1/elgeeko1-docker-ansible.git

# How to use this role
### Method 1: Install using ansible-galaxy

The most robust way to install this role is to use ansible-galaxy,
which is installed with ansible by default. ansible-galaxy is effectively a package manager for ansible. It installs roles
from the community, or from private git repos, into your local machine for use in your playbooks.

Start by adding this role to an ansible-galaxy dependency file. Typically this file lives alongside your playbook file and by convention is named `requirements.yml`.

Add the following section to `requirements.yml`:

```
roles:
  - name: elgeeko1-pihole-ansible
    src: https://github.com/elgeeko1/elgeeko1-pihole-ansible
    version: main
```

If there is already a `roles` section, simply append this role to
add it as a dependency.

Then, install the role using ansible-galaxy:

`ansible-galaxy install -r requirements.yml -v`

### Method 2: Clone this repository into a `roles` directory

Use this method if you plan to modify this role, or if for some
reason the ansible-galaxy method fails.

Starting from your playbook directory, change into the `roles`
directory and clone:

```
$ ls
 > playbook.yml
 > roles/
$ cd roles/
roles$ git clone https://github.com/elgeeko1/elgeeko1-pihole-ansible
```

# Configuring
Configurable variables are defined in `defaults/main.yml`.

A path in the host filesystem is used for persistant
storage of pihole configuration, gravity database,
and cache.

`pihole_path: /opt/pihole`

Docker network mode for the pihole container
may be 'bridge' or 'host'.

`pihole_network_mode: bridge`

Network addresses for pihole. These should be set
to the addresses that will be visible to the rest
of your network.
```
pihole_ipv4_address: "192.168.1.2"
pihole_ipv6_address: "::1"
pihole_ipv6_enabled: false
```

DNS servers for pihole to use. Default is quad9.

`pihole_dns: "9.9.9.9#53;149.112.112.112#53;2620:fe::fe#53;2620:fe::9#53"`

pihole web UI password (use something more secure!)

`pihole_web_password: pihole`

pihole privacy level, 0 is everything, 3 is none

`pihole_privacy_level: 0`

log queries in dnsmasque?

`pihole_query_logging: true`

dnssec?

`pihole_dnssec: false`

Timezone for pihole UI and logging

`pihole_timezone: "America/Los_Angeles"`

# Security

### Bridged network mode (default)
By default, pihole will run in a docker container within the default docker bridge network. This is the most secure as it has the most restrictive network access, however it can be more difficult to debug issues.

Bridged network mode is set with the role variable
`pihole_network_mode: bridge`

### Host network mode
This is less secure but is a more surefire configuration.

Host network mode is set with the role variable
`pihole_network_mode: host`
