# Drone Ansible Role

Install [drone](https://github.com/drone/drone), [docker](https://www.docker.io/), required and optional docker images for drone operation.

## Version Incompatibilities

Version 1.x.x of this Ansible role is incompatibile with installations created by Version 0.x.x.

Additionally, the role configuration has changed and is incompatible with previous configurations.

The documentation in this document, is only for version 1.x.x.

## Requirements

1. Ubuntu 12.04+

## Role Variables

### Upgrades

Drone does not currently offer an apt repository, to upgrade to a new verison supply `--extra-vars upgrade_drone=true` with your `ansible-playbook` run.

### drone\_images

`drone_images` is a list of hashes, each hash containing the following:

* `name` name of the docker repo/image
* `tag` image tag to pull. Default `latest`
* `state` absent/present. Default `present`

### drone\_users

`drone_users` is a list of hashes, each hash containing the following:

* `remote` The remote system that the user exists in. e.g. `github.com`
* `login` The remote login username of the user
* `admin` True/False (boolean). Default `False`
* `active` True/False (boolean). Default `True`
* `state` present/absent. Default `present`

#### Remotes

Valid values for the `remote` configuration are

1. github.com
1. enterprise.github.com
1. gitlab.com
1. bitbucket.org
1. gogs

### Email

* `drone_smtp_server` SMTP server address
* `drone_smtp_port` SMTP server port
* `drone_smtp_address` EMail address to send from
* `drone_smtp_username` SMTP authenticaiton username
* `drone_smtp_password` SMTP authentication password

### GitHub

* `drone_github_client` GitHub application client ID
* `drone_github_secret` GitHub application secret

### GitHub Enterprise

* `drone_github_enteprise_client` GitHub application client ID
* `drone_github_enteprise_secret` GitHub application secret
* `drone_github_enteprise_url` GitHub domain. e.g. `https://github.drone.io/`
* `drone_github_enteprise_api_url` GitHub Enterprise API URL. e.g. `https://github.drone.io/api/v3/`
* `drone_github_enteprise_private_mode` True/False (boolean) Whether your GitHub Enterprise installation is configured for Private Mode
* `drone_github_enteprise_skip_verify` True/False (boolean) Whether to Skip SSL verification

### Bitbucket

* `drone_bitbucket_client` Bitbucket application key
* `drone_bitbucket_secret` Bitbucket application secret

### GitLab

* `drone_gitlab_url` GitLab URL. e.g. `http://gitlab.drone.io`

### GOGS

* `drone_gogs_url` GOGS URL. e.g. `http://gogs.drone.io`
* `drone_gogs_secret` GOGS secret

### Daemon

* `drone_port` Port for drone to listen on
* `drone_sslcert` Path to SSL certificate
* `drone_sslkey` Path to SSL key
* `drone_worker_cert` Path to SSL certificate for Docker worker connection
* `drone_worker_key` Path to SSL key for DOcker worker connection
* `drone_worker_nodes` List of Docker connection strings.  Default `- "unix:///var/run/docker.sock"`

### Database

* `drone_database_driver` Database driver to use. Default `sqlite3`. Options are `sqlite3`, and `mysql`
* `drone_database_datasource` Database datasource connection string. Default `/var/lib/drone/drone.sqlite`. For `mysql` see https://github.com/go-sql-driver/mysql#dsn-data-source-name

MySQL support is limited, in order for mysql to be configured, use of the `mysql` command must not require username or password specification. Additionally, the database must be named `drone`.

If using MySQL, `mysql-server` must already be installed and the database created before running this role.

### Other

* `drone_open_invitations` True/False (boolean). Whether open sign up is enabled. Default `False`.
* `drone_session_secret` String to use when signing session tokens, leave unconfigured to allow Drone to randomly assign a secret at start
* `drone_session_expires` Session expiration time. Default `72h`. For time formating see http://golang.org/pkg/time/#ParseDuration

## Dependencies

1. [docker\_ubuntu](https://galaxy.ansible.com/list#/roles/292)

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    - hosts: drone
      roles:
        - role: sivel.drone
          drone_images:
            - name: bradrydzewski/python
              tag: 2.7
            - name: bradrydzewski/go
              tag: 1.2
            - name: bradrydzewski/php
              tag: 5.5
              state: absent
          drone_users:
            - remote: github.com
              login: jim.beam
              admin: True
            - remote: github.com
              login: jack.daniels
            - remote: Johnnie Walker
              login: johnnie.walker
              state: absent
          drone_port: 443
          drone_sslcert: /etc/drone/ssl.crt
          drone_sslkey: /etc/drone/ssl.key
          drone_github_client: 3c6e0b8a9c15224a8228
          drone_github_secret: 5ebe2294ecd0e0f08eab7690d2a6ee69ccc4a4d8
          drone_smtp_server: localhost
          drone_smtp_port: 25
          drone_open_invitations: True

## License

Apache License, Version 2.0
