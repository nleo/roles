    ---
    - hosts: sampleapp
      vars_files:
        - ssh_keys.yml
      vars:
        ruby_version: 2.5
        postgresql_version: 9.6
        nodejs_version: 8.x
        app_name: SampleApp
        hostname: SampleApp
        domain: example.com
        deploy_user: rails
        app_path: /home/{{deploy_user}}/{{app_name}}
        app_log_path: "{{app_path}}/log"
        database: "{{app_name}}"
        dbuser: "{{app_name}}"
        dbpass: password
        backup_user: backup_user
        backup_server: 1.1.1.1
        repo_url: git@bitbucket.org:some/app.git
        railsapp_webpack: on
        ssl: off
      roles:
        - { role: common, tags: [ 'common' ] }
        - { role: nodejs, tags: [ 'nodejs' ] }
        - { role: railsapp, tags: [ 'railsapp' ] }
        - { role: sshkeys, tags: [ 'sshkeys' ] }
        - { role: ruby, tags: [ 'ruby' ] }
        - { role: postgresql, tags: [ 'postgresql' ] }
        - { role: passenger, tags: [ 'passenger' ] }
        - { role: logrotate, tags: [ 'logrotate' ] }
        - { role: ufw, tags: [ 'ufw' ] }
        - { role: bash, tags: [ 'bash' ] }
        - { role: remote-postgres-backup, tags: [ 'remote-postgres-backup' ] }
