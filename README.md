# Ansible role for pihole
This role deploys and configures pihole to run in a docker
container.

Features:
- pihole in docker container
- docker container runs in bridge network mode (default, more secure) or host network mode
- services port 53 on your host
- ipv6 support (if your docker is configured for ipv6)

## Requirements
- ansible
- docker

## Configuring
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

## Security

### Bridged network mode (default)
By default, pihole will run in a docker container within the default docker bridge network. This is the most secure as it has the most restrictive network access, however it can be more difficult to debug issues.

Bridged network mode is set with the role variable
`pihole_network_mode: bridge`

### Host network mode
This is less secure but is a more surefire configuration.

Host network mode is set with the role variable
`pihole_network_mode: host`
