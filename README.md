# Ansible roles for Rails production deployment

Tested on Ubuntu 18.04

## hosts

```ini
[sampleapp]
1.1.1.1 domain=api.sample.com frontend_domain=sample.com
2.2.2.2 domain=api.staging.sample.com staging=yes frontend_domain=staging.sample.com
[all:vars]
ansible_ssh_user=root
ansible_python_interpreter=/usr/bin/python3
```

## site.yml
```yaml
---
- hosts: sampleapp
  vars_files:
    - ssh_keys.yml
  vars:
    ruby_version: 2.6
    postgresql_version: 11
    nodejs_version: 12.x
    app_name: sample_app
    hostname: "{{domain}}"
    deploy_user: "{{app_name}}"
    app_path: /home/{{deploy_user}}/{{app_name}}
    app_log_path: "{{app_path}}/log"
    database: "{{app_name}}"
    dbuser: "{{app_name}}"
    dbpass: password
    railsapp_webpack: on
    ssl: on
    rpush: on
    additional_packages:
      - imagemagick
    postgresql_extentions:
      - pg_trgm
      - fuzzystrmatch
    passenger_max_pool_size: 3
    passenger_max_requests: 1000
    frontend:
      user: react
      app_path: /home/react/{{app_name}}
      frontenders:
        - ssh-rsa AAAABxxx== developer_name
    revoke_ssh_keys: ["{{developers.developer_name}}"]
    remote_postgres_backup:
      minute: 21
      server: backup.host
      user: remote_backup
      password: xxx
  roles:
    - { role: common, tags: [ 'common' ] }
    - { role: motd, tags: [ 'motd' ] }
    - { role: nodejs, tags: [ 'nodejs' ] }
    - { role: railsapp, tags: [ 'railsapp' ] }
    - { role: sshkeys, tags: [ 'sshkeys' ] }
    - { role: ruby, tags: [ 'ruby' ] }
    - { role: postgresql, tags: [ 'postgresql' ] }
    - { role: passenger, tags: [ 'passenger' ] }
    - { role: logrotate, tags: [ 'logrotate' ] }
    - { role: ufw, tags: [ 'ufw' ] }
    - { role: yarn, tags: [ 'yarn' ] }
    - { role: remote-postgres-backup, tags: [ 'remote-postgres-backup' ] }
    - { role: redis-server, tags: [ 'redis-server' ] }
    - { role: certbot, tags: [ 'certbot' ] }
    - { role: sidekiq, tags: [ sidekiq ] }
    - { role: bash, tags: [ 'bash' ] }
    - { role: frontend, tags: [ 'frontend' ] }
```
