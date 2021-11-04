# OpenVPN

An Ansible Role that installs and configures OpenVPN server on Debian.


## Role Variables

### General variables
Available variables are listed below, along with default values (see `defaults/main.yml`):


    openvpn_etcdir: /etc/openvpn

OpenVPN server configuration location

    openvpn_datadir: "{{ openvpn_etcdir }}/data"

    openvpn_pki_dir: "{{ openvpn_etcdir }}/pki"

Directory for our internal PKI

    openvpn_configure_forwarding: true

Enable IP fowarding

    openvpn_host: "{{ inventory_hostname }}"

Hostname used for the OpenVPN server

    openvpn_port: 1194
    openvpn_proto: udp

Protocol and port to use for the OpenVPN server

    openvpn_dev: tun
    openvpn_server: 10.8.0.0 255.255.255.0
    openvpn_bridge: {}
    openvpn_max_clients: 100
    openvpn_log: /var/log/openvpn.log
    openvpn_keepalive: "10 120"
    openvpn_ifconfig_pool_persist: ipp.txt

    openvpn_comp_lzo: true

Enable or disable LZO compression

    openvpn_cipher: BF-CBC
    openvpn_status: openvpn-status.log

Status log file name

    openvpn_verb: 3

Verbosity level of our VPN server

    openvpn_tls_key: "{{ openvpn_pki_dir }}/ta.key"

    openvpn_user: nobody
    openvpn_group: nogroup

User and group running the OpenVPN server (we recommand to use non privileged user)

    openvpn_resolv_retry: infinite
    openvpn_client_to_client: true

    openvpn_route_ranges: []

Network ranges that the connecting clients should try to reach using the VPN 
connection. 

    openvpn_dns_servers: []
DNS servers to push to the connecting client to avoid leaks via DNS queries

    openvpn_client_options: []

Allows to define additional client options

    openvpn_server_options: []

Allows to define additional server options

### Certificates and keys related variables

    openvpn_key_country: US
    openvpn_key_province: CA
    openvpn_key_city: SanFrancisco
    openvpn_key_org: Fort-Funston
    openvpn_key_email: me@myhost.mydomain
    openvpn_key_size: 2048

Variables to manages server keys and certificates


### Clients and authentication variables

#### Clients


    openvpn_clients_revoke: []

List of clients certificates to revoke

    openvpn_unified_client_profiles: false

Allows to generate unified ovpn file for clients (one file containing config, certificates and keys)

    openvpn_clients: []

List of known clients (this variable is used to generate keys, certificates, config,... related to the known clients)

#### LDAP authentication related variable (default is disabled)

List of variables available to configure OpenVPN

    openvpn_use_ldap: false

Enable or disable LDAP authentication

    openvpn_ldap_tlsenable: false

Enable or disable TLS when using LDAP authentication    

    openvpn_ldap_follow_referrals: false

Enable or disable to follow referrals when using LDAP authentication


#### PAM authentication related variable (default is enabled)

    openvpn_use_pam: true
    openvpn_use_pam_users: []

#### Use simple authentication (default is disabled)

    openvpn_simple_auth: false
    openvpn_simple_auth_password: ""

### Download certificates

Allow to download the created client credentials to a specified local (relative to control host) directory
    openvpn_download_clients: false
    openvpn_download_dir: "{{ playbook_dir }}/client_credentials/"

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: vpn-user.user.dalibo.net
      become: true
      vars:
        openvpn_key_size: 2048
        openvpn_key_country: FR
        openvpn_key_province: Paris
        openvpn_key_city: Paris
        openvpn_key_org: MyCompany
        openvpn_key_email: admin@mycompany.fr
        openvpn_unified_client_profiles: true
        openvpn_clients:
            - john
            - doe
            - smith
      roles:
        - {hanxhx.openvpn}


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
