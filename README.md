# ssh

An Ansible role that manages ssh and sshd. Currently the role has the following
features:

* Install ssh-server
* Manage config settings in `/etc/ssh/sshd_config`, see
  [role variables](#role-variables)
* Manage global known hosts files for ssh clients (`/etc/ssh/ssh_config`)

## Requirements

No prerequisites necessary at the moment.

## Role Variables

Available variables are listed below, along with default values (see also `defaults/main.yml` as well as `man sshd_config`):

### ssh_port

    ssh_port: "22"

Specifies the port number that sshd listens on.

### ssh_address_family

    ssh_address_family: "inet"

Specifies which address family should be used by sshd.

### ssh_root_login

    ssh_root_login: "prohibit-password"

Specifies whether root can log in using ssh.  The argument must be yes, prohibit-password, forced-commands-only, or no.

### ssh_syslogfacility

    ssh_syslogfacility: "AUTHPRIV"

Gives the facility code that is used when logging messages from sshd

### ssh_password_auth

    ssh_password_auth: "no"

Specifies whether password authentication is allowed.

### ssh_pubkey_auth

    ssh_pubkey_auth: "yes"

Specifies whether public key authentication is allowed.

### ssh_kbdinteractive

    ssh_kbdinteractive: "yes"

Specifies whether to allow keyboard-interactive authentication.

### ssh_x11_forwarding

    ssh_x11_forwarding: "yes"

Specifies whether X11 forwarding is permitted.

### ssh_gssapi_authentication

    ssh_gssapi_authentication: "yes"

Specifies whether user authentication based on GSSAPI is allowed.

### ssh_permit_tunnel

    ssh_permit_tunnel: "no"

Specifies whether tun device forwarding is allowed.

### ssh_motd

    ssh_motd: "yes"

Specifies whether sshd should print /etc/motd when a user logs in interactively.

### ssh_maxstartups

    ssh_maxstartups: "1024"

Specifies the maximum number of concurrent unauthenticated connections to the SSH daemon.
    
### ssh_allowusers

    ssh_allowusers: []

This keyword can be followed by a list of user name patterns, separated by spaces.  If specified, login is allowed only for user
names that match one of the patterns.

### ssh_allowgroups

    ssh_allowgroups: []

This keyword can be followed by a list of group name patterns, separated by spaces.  If specified, login is allowed only for users whose primary group or
supplementary group list matches one of the patterns.  Only group names are valid; a numerical group ID is not recognized.

### ssh_global_known_hosts

    ssh_global_known_hosts: []

Specifies one or more files to use for the global host key database, separated by whitespace.

### ssh_manage_firewall

    ssh_manage_firewall: true

Whether to manage firewall (firewalld) or not

## Example Playbook

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: unibeid.ssh, ssh_port: "42" }

## Compatibility

This role has been written for and tested on and is therefore compatible with:

* Rocky-9
* Ubuntu 22.04

## License

MIT

## Author Information

The role was created in 2023 by the IT-Services Office of the University of Bern
