zabbix-rabbitmq
=========

A role to install and configure RabbitMQ monitoring on Zabbix.

Requirements
------------

This role requires a valid Zabbix repository on your distro to be able to install zabbix_sender.
You may use the dj-wasabi.zabbix-agent role to setup the Zabbix repository and also Zabbix Agent, but you can add the Zabbix repository manually.

This role is based on the work of https://github.com/jasonmcintosh/rabbitmq-zabbix so it includes the scripts mentionated on it's Readme and also it's Zabbix Template that should be imported on your Zabbix server.
You may find more details on the Readme.md on the Github page of the project or in the Readme.md inside the files folder.

Role Variables
--------------

**Define if this role should try to install Zabbix Sender**

    zabbix_install_sender: true

**Define the hostname the script should connect (localhost in case you make the monitoring user localhost only)** (string)

    zabbix_rabbit_hostname: localhost

**Define the path to the RabbitMQ config file** (string)

    rabbit_config_file: /etc/rabbitmq/rabbitmq.conf

**Define the path to the plugin log** (string)

    zabbix_rabbit_log_file: /var/log/zabbix/rabbitmq_zabbix.log

**Define the log level** (string)

    zabbix_rabbit_log_level: INFO

**Define a filter to the Zabbix Template Discovery** (string in json format)

    zabbix_rabbit_monitoring_filter: '{"durable": true}'

**Define the RabbiMQ Management port** (number)

    zabbix_rabbit_monitoring_port: 15672

**Let the role manage the RabbitMQ Monitoring User** (bool)

    zabbix_rabbit_manage_monitoring_user: true

**Default values to create a RabbitMQ Monitoring User** (dict)

    zabbix_rabbit_monitoring_user: 
      login: zabbix
      password: zabbix
      tags: monitoring
      localhost_only: true
      permissions:
        - vhost: /
          configure_priv: .*
          read_priv: .*
          write_priv: .* 

**Define if this role may restart RabbitMQ in the case it change some of it`s settings** (bool)

    zabbix_rabbit_restart_service: false
    
**The path to store the plugin files** (string)

    zabbix_scripts_path: /usr/local/zabbix/rabbitmq

**The path to Zabbix User Parameters directory** (string)

    zabbix_userparameter_path: "{{agent_include|default('/etc/zabbix/zabbix_agentd.d')}}"

**The path to Zabbix config directory** (string)

    zabbix_config_dir: "/etc/zabbix/" 

**The Zabbix config filename** (string)

    zabbix_config_file: "{{zabbix_agent_conf|default('zabbix_agentd.conf')}}"
  


Dependencies
------------

This role may dependes on dj-wasabi.zabbix-agent role, and in the case you use it and allow it to preserve they variables during the playbook run, this role will use some as his variables as default values to zabbix config directory and files.

Example Playbook
----------------

    - hosts: servers
      vars:
        zabbix_rabbit_monitoring_user: 
          login: myuser
          password: mypassword
          tags: monitoring
          localhost_only: true
          permissions:
            - vhost: /
              configure_priv: .*
              read_priv: .*
              write_priv: .* 
            - vhost: another_vhost
              configure_priv: .*
              read_priv: .*
              write_priv: .* 
        zabbix_rabbit_restart_service: true

      roles:
         - { role: zabbix-rabbitmq }

License
-------

BSD

Author Information
------------------

Claudinei Matos <claudineimatos@gmail.com>
