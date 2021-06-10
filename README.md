# iptables-docker-filter

Filter `iptables -S` or `iptables-save` output removing rules that seem to have
been inserted by the [Docker
Engine](https://www.docker.com/products/container-runtime).

## About

Most of the time, on Docker hosts I do not use a full-fledged firewall manager
since the system firewall is generally used for minor adjustments (covering
inevitable any-address binds, occasional white/blacklists). I rely on the
simplicity of `iptables-persistent`/`netfilter-persistent`, and update my rules
using the good old `iptables-save` whenever I need to make changes to
them.

Docker needs to interact with the firewall for some of its needs, and [has its
"framework"](https://docs.docker.com/network/iptables/) to allow users to
establish a workflow to secure Docker applications. It however expects the user
to have a system for setting up the firewall that is unperturbed by its actions
and this is not true for `iptables-save`.

This script is a dirty, hackish way to try and determine which rules may have
been inserted by Docker, and filter them out.

## Usage

```txt
Usage: iptables-docker-filter [OPTIONS]

OPTIONS:
    -h:         Print help
    -q:         Quiet mode: suppress writing informational messages on STDERR
    -c:         Only comment filtered lines, do not remove them
    -i <FILE>:  Read rules from file instead of STDIN
    -o <FILE>:  Output rules to file instead of STDOUT
    -b:         Backup output file if existing (needs -o)

EXAMPLES:
    iptables-save | iptables-docker-filter > /etc/iptables/rules.v4
    iptables-save | iptables-docker-filter -o /etc/iptables/rules.v4 -b -c
    iptables-docker-filter -i /etc/iptables/rules.v4 -o /etc/iptables/rules.v4.without-docker
```
