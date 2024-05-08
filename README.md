# ssh

An Ansible role that manages ssh and sshd. Currently the role has the following
features:

* Install sshd server and SSH clients
* Manage server config settings in `/etc/ssh/sshd_config`, see
  [role variables](#role-variables)
* Manage SSH client config settings in `/etc/ssh/ssh_config`, see
  [role variables](#role-variables)
* Support global known hosts files for ssh clients (`/etc/ssh/ssh_config`)

> [!NOTE]
> This role does NOT include firewall configuration. If your system is
> protected by a firewall, which it probably should, you need to manage the
> respective firewall rules independently, e.g. in the playbook that
> includes or imports this role.

## Requirements

No prerequisites necessary at the moment.

## Role Variables

Available variables are listed below, along with default values (see also
`defaults/main.yml` as well as `man sshd_config`):

### ssh_manage_sshd

    ssh_manage_sshd: true

Specifies whether the sshd service should be managed by this role or not.
Usually this is left to true, but might be temporarily set to false when this is
needed.

### ssh_manage_motd_file

    ssh_manage_motd_file: false

Specifies whether to craft a custom `/etc/motd` file showing some system
informations like OS name/version, IP addresses and CPU and memory information.
If set to false, the file `/etc/motd` is not modified, if set to true the file
is managed an updated if needed.

### ssh_port

    ssh_port: "22"

Specifies the port number that sshd listens on. If you want to change the port
on a SELinux system, you have to tell SELinux about this change, for example:
`semanage port -a -t ssh_port_t -p tcp #PORTNUMBER`

### ssh_address_family

    ssh_address_family: "inet"

Specifies which address family should be used by sshd.  Valid arguments are
`any` (the default), `inet` (use IPv4 only), or `inet6` (use IPv6 only). Specifies
which address family should be used by sshd. The default is IPv4 only.

### ssh_permit_root_login

    ssh_permit_root_login: "prohibit-password"

Specifies whether root can log in using ssh. The argument must be one of `yes`,
`prohibit-password`, `forced-commands-only`, or `no`. The default is
`prohibit-password`, which disables password and keyboard-interactive
authentication for root.

### ssh_syslogfacility

    ssh_syslogfacility: ""

Gives the facility code that is used when logging messages from sshd. The
default is OS-dependent, e.g. `AUTHPRIV` for Enterprise Linux and `AUTH` for
Ubuntu. For possible values see `man sshd_config`.

### ssh_use_pam

    ssh_use_pam: "yes"

Enables the Pluggable Authentication Module interface. See `man sshd_config` for
detailed information. This defaults to `yes` as having this set to `no` is not
supported in RHEL!

### ssh_password_auth

    ssh_password_auth: "no"

Specifies whether password authentication is allowed. Leave this to `no` when
using PAM. Default to `no` to disable tunneled clear text passwords.

### ssh_kbdinteractive

    ssh_kbdinteractive: "yes"

Specifies whether to allow keyboard-interactive authentication.

### ssh_gssapi_auth

    ssh_gssapi_auth: "no"

Specifies whether user authentication based on GSSAPI is allowed.

### ssh_pubkey_auth

    ssh_pubkey_auth: "yes"

Specifies whether public key authentication is allowed.

### ssh_x11_forwarding

    ssh_x11_forwarding: "yes"

Specifies whether X11 forwarding is permitted.

### ssh_permit_tunnel

    ssh_permit_tunnel: "no"

Specifies whether tun device forwarding is allowed.

### ssh_maxstartups

    ssh_maxstartups: "10:30:100"

Specifies the maximum number of concurrent unauthenticated connections to the
SSH daemon. The role features the OpenSSH default value. Overriding this
option is rarely needed but then important, like for the GPFS file system daemon
that may rapidly spawn a lot of SSH connections and therefore needs this option
set to 1024.

### ssh_use_dns

    ssh_use_dns: "no"

Specifies whether sshd should look up the remote host name, and to check that
the resolved host name for the remote IP address maps back to the very same IP
address.  
If this option is set to no (the default) then only addresses and not host names
may be used in ~/.ssh/authorized_keys.

### ssh_sftp_server

    ssh_sftp_server: ""

Configures an external sftp server. By default, when set to an empty value, the
external command `sftp-server` is used, for which the path is OS-dependent.
Override this config value if you want to use another sftp server, i.e. `internal-sftp`
for the in-process SFTP server.

### ssh_allowusers

    ssh_allowusers: []

This keyword can be followed by a list of user name patterns, separated by
spaces.  If specified, login is allowed only for user names that match one of
the patterns.

### ssh_allowgroups

    ssh_allowgroups: []

This keyword can be followed by a list of group name patterns, separated by
spaces.  If specified, login is allowed only for users whose primary group or
supplementary group list matches one of the patterns.  Only group names are
valid; a numerical group ID is not recognized.

### ssh_global_known_hosts

    ssh_global_known_hosts: []

Specifies one or more files to use for the global host key database, separated
by whitespace.

### ssh_crypto_hostkey_algos

    ssh_crypto_hostkey_algos: []

This role configures only secure algorithms by default in order to have
`ssh-audit` pass with all green. Use this variable to override the host key
algorithms that this role defines by defaults. See `vars/*.yml` for details.

### ssh_crypto_kex_algos

    ssh_crypto_kex_algos: []

This role configures only secure algorithms by default in order to have
`ssh-audit` pass with all green. Use this variable to override the key exchange
algorithms that this role defines by defaults. See `vars/*.yml` for details.

### ssh_crypto_ciphers

    ssh_crypto_ciphers: []

This role configures only secure algorithms by default in order to have
`ssh-audit` pass with all green. Use this variable to override the encryption
algorithms that this role defines by defaults. See `vars/*.yml` for details.

### ssh_crypto_macs

    ssh_crypto_macs: []

This role configures only secure algorithms by default in order to have
`ssh-audit` pass with all green. Use this variable to override the message
authentication code algorithms that this role defines by defaults. See
`vars/*.yml` for details.

## Example Playbook

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: unibeid.ssh
           vars:
             ssh_port: "42"

## Compatibility

This role has been written for and tested on and is therefore compatible with:

* CentOS-7
* Rocky-8
* Rocky-9
* Ubuntu-20.04
* Ubuntu-22.04

## Dependencies

This role has no dependencies.

## License

MIT

## Author Information

The role was created in 2023 by the IT-Services Office of the University of Bern
