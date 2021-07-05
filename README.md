Maxscale
=========

Role for basic installation and configuring of maxscale ( Debian 10). This role is inspired by the role of HanXHX: https://github.com/HanXHX/ansible-maxscale


Requirements
------------

You need to define the users that you'll use in maxscale configuration in your databases. See https://mariadb.com/kb/en/mariadb-maxscale-20-setting-up-mariadb-maxscale/ for more information.

Role Variables
--------------

| Variable | Description | Default |
|----------|-------------|---------|
| `maxscale_version` | Maxscale version to install | `latest` |
| `maxsacle_apt_repo` | The repository from where the package will be download | `https://downloads.mariadb.com/MaxScale/{{ maxscale_version }}/{{ ansible_distribution | lower }}` |
| `maxsacle_apt_repo_distrib` | User's password  | `buster` |
| `maxsacle_apt_repo_name` | The name of the sourcelist file  | `maridb_maxscale_{{ maxscale_version }}` |
| `apt_url_maxscale_key` | The url from where the key will be downaloded  | `maridb_maxscale_{{ maxscale_version }}` |
| `validate_certs` | If no, SSL certificates for the target url will not be validated (apt_url_maxscale_key)  | `yes` |
| `maxscale_threads` | This parameter controls the number of worker threads  | `auto` |
| `maxscale_service_state` | State of maxscale service after installation  | `started` |
| `maxscale_service_enabled` | Whether or not the service is enabled | `yes` |
| `maxscale_threads` | This parameter controls the number of worker threads  | mandatory |
| `maxscale_services` | See https://mariadb.com/kb/en/mariadb-maxscale-23-mariadb-maxscale-configuration-usage-scenarios/#service_1 for more information  | mandatory |
| `maxscale_monitors` | See https://mariadb.com/kb/en/mariadb-maxscale-23-mariadb-maxscale-configuration-usage-scenarios/#monitor for more information  | mandatory |
| `maxscale_listeners` | See https://mariadb.com/kb/en/mariadb-maxscale-23-mariadb-maxscale-configuration-usage-scenarios/#listener for more information  | mandatory |
| `maxscale_servers` | See https://mariadb.com/kb/en/mariadb-maxscale-23-mariadb-maxscale-configuration-usage-scenarios/#server for more information  | mandatory |


Dependencies
------------

This role doesn't have any dependancies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: DB-proxy
      vars:
      - maxscale_services:
          read-write:
            router: readconnroute
            router_options: master
            servers: maridb-01,zabbix-db-dc2-02
            user: maxcli
            password: B3tterP@ss!
      - maxscale_monitors:
          MariaDB:
            module: mariadbmon
            servers: maridb-01,zabbix-db-dc2-02
            ignore_external_masters: true
            auto_failover: true
            auto_rejoin: true
            detect_replication_lag: true
            replication_user: replication
            replication_password: similarly-secure-password
            user: maxmon
            password: P@ss!
            monitor_interval: 1000
      - maxscale_listeners:
          read-write:
            service: read-write-Service
            protocol: MariaDBClient
            port: 3306
      - maxscale_servers:
          maridb-01:
            address: maridb-01.exemple.com
            port: 3306
            protocol: MariaDBBackend
          maridb-02:
            address: maridb-02.exemple.com
            port: 3307
            protocol: MariaDBBackend
      roles:
         - role: ansible-role-maxscale

License
-------

ISC

Author Information
------------------

To get in touch with author you can create a issue on github when requesting some feature.
