# ansible-wowza

Ansible role to install [Wowza Streaming Engine](https://www.wowza.com/products/streaming-engine) on Linux.

## Idempotence note

Please note: checking for already installed on the server Wowza Streaming Engine realised by determining status of the Wowza license file which installed by default to the /usr/local/WowzaStreamingEngine/conf/Server.license path.

## Requirements

Requirements are listed in the role metadata.

## Role Variables

### Default role variables

``` yaml
# Just install WSE without any tweaks and configuration changes
wowza_default_install: no

wowza_download_path: "https://www.wowza.com/downloads/WowzaStreamingEngine-4-8-5/"
wowza_installer: "WowzaStreamingEngine-4.8.5-linux-x64-installer.run"
wowza_installer_checksum: "sha1:ec48f072b8c54948ce8e1f4cc4f3af361d5e1665"

# Must be located on the file system with exec mount option!!!
wowza_install_workdir: "/root"

wowza_install_script: wowza_install_script.exp

# Wowza admin user
wowza_admin_user: ""
# Wowza admin password
wowza_admin_password: ""
# Wowza Streaming Engine license key
wowza_license_key: ""

wowza_users: ""
#  sampleuser:
#    password: sampleuser_secret
#    groups:
#      - admin
#      - advUser

# Wowza Streaming Engine default path variables
wowza_directory: "/usr/local/WowzaStreamingEngine"
wowza_config_directory: "{{ wowza_directory }}/conf"
wowza_manager_directory: "{{ wowza_directory }}/manager"
wowza_manager_counfig_directory: "{{ wowza_manager_directory }}/conf"
wowza_applications_directory: "{{ wowza_directory }}/applications"
wowza_lib_directory: "{{ wowza_directory }}/lib"
wowza_log_directory: "/var/log/wowza"
wowza_systemd_include_dir: "/etc/systemd/system/WowzaStreamingEngine.service.d"
wowza_systemd_service_files:
  - "/usr/lib/systemd/system/WowzaStreamingEngine.service"
  - "/usr/lib/systemd/system/WowzaStreamingEngineManager.service"
wowza_uninstall_files:
  - "{{ wowza_directory }}/uninstall"
  - "{{ wowza_directory }}/Uninstall\ Wowza\ Streaming\ Engine.desktop"

wowza_license_file: "{{ wowza_config_directory }}/Server.license"
wowza_config_server: "{{ wowza_config_directory }}/Server.xml"
wowza_config_vhost: "{{ wowza_config_directory }}/VHost.xml"
wowza_config_vhosts: "{{ wowza_config_directory }}/VHosts.xml"
wowza_config_tune: "{{ wowza_config_directory }}/Tune.xml"
wowza_config_mediacache: "{{ wowza_config_directory }}/MediaCache.xml"
wowza_config_startup_streams: "{{ wowza_config_directory }}/StartupStreams.xml"
wowza_config_admin_password: "{{ wowza_config_directory }}/admin.password"
wowza_config_publish_password: "{{ wowza_config_directory }}/publish.password"
wowza_config_jmxremote_access: "{{ wowza_config_directory }}/jmxremote.access"
wowza_config_jmxremote_password: "{{ wowza_config_directory }}/jmxremote.password"
wowza_config_log4j: "{{ wowza_config_directory }}/log4j.properties"
wowza_manager_config_log4j: "{{ wowza_manager_counfig_directory }}/log4j.properties"
wowza_manager_config_tomcat_log4j: "{{ wowza_manager_counfig_directory }}/tomcat-log4j.properties"

wowza_config_files:
  - "{{ wowza_config_server }}"
  - "{{ wowza_config_vhost }}"
  - "{{ wowza_config_vhosts }}"
  - "{{ wowza_config_tune }}"
  - "{{ wowza_config_mediacache }}"
  - "{{ wowza_config_startup_streams }}"
  - "{{ wowza_config_admin_password }}"
  - "{{ wowza_config_publish_password }}"
  - "{{ wowza_config_jmxremote_access }}"
  - "{{ wowza_config_jmxremote_password }}"
  - "{{ wowza_config_log4j }}"
  - "{{ wowza_manager_config_log4j }}"
  - "{{ wowza_manager_config_tomcat_log4j }}"

# Wowza Streaming Engine configs with sensitive data
wowza_config_secret_files:
  - "{{ wowza_license_file }}"
  - "{{ wowza_config_admin_password }}"
  - "{{ wowza_config_publish_password }}"
  - "{{ wowza_config_jmxremote_access }}"
  - "{{ wowza_config_jmxremote_password }}"
  - "{{ wowza_config_mediacache }}"

# Wowza Vhost config
wowza_streaming_ip: '*'
wowza_streaming_port: ['1935']
wowza_tcp_nodelay: no
wowza_acceptor_backlog: 100
# Defaul AuthenticationMethod for HTTP Providers and WSE Manager
# none, admin-basic, admin-digest, admin-file-digest
# Set to admin-file-digest if digestfile REST authentication enabled.
wowza_admin_auth_method: "{{ 'admin-file-digest' if wowza_rest_auth_method == 'digestfile' else 'admin-digest' }}"
wowza_admin_ip: '*'
wowza_admin_port: 8086
# Wowza SSL
wowza_ssl_enable: no
wowza_streaming_ssl_ip: '*'
wowza_streaming_ssl_port: 443
wowza_ssl_keystore_file: 'keystore.jks'
wowza_ssl_keystore_password: ''
# Enable Wowza SSL above to use WebRTC
wowza_webrtc_enable: no
wowza_webrtc_admin_enable: no
wowza_webrtc_admin_ip: '*'
wowza_webrtc_admin_port: 9443

# Wowza REST API
wowza_rest_enable: yes
# REST API listen address
wowza_rest_ip: '*'
wowza_rest_port: 8087
wowza_rest_whitelist: ['127.0.0.1']
wowza_rest_blacklist: []
# Defaul AuthenticationMethod for REST API.
# 'none' and 'basic' are no longer compatible starting from WSE 4.8.5.
# Use digest, digestfile or remotehttp
wowza_rest_auth_method: digest
wowza_rest_docs_enable: no
wowza_rest_docs_port: 8089

# Wowza command interface
wowza_command_enable: no
wowza_command_ip: '127.0.0.1'
wowza_command_port: 8083

# Wowza JMX interface
wowza_jmx_enable: no
wowza_jmx_ip: '127.0.0.1'
wowza_jmx_rmi_hostname: localhost
wowza_jmx_rmi_connection_port: 8084
wowza_jmx_rmi_registry_port: 8085

# Wowza Mediacache
wowza_mediacache_enable: no

# Production or Development or custom size e.g. 8000M
wowza_tuning_heapsize: Development

# WARNING!!! Wowza Load Testing Tool disables Wowza Streaming Engine Manager.
# Do not use Wowza Load Testing Tool on production streaming servers!
# You need WowzaStreamingEngineLoadTestTool.jar file from separate package:
# https://www.wowza.com/docs/test-wowza-streaming-engine-media-server-streaming-load
wowza_loadtesttool_enable: no

# Wowza modules
wowza_modules_collection_url: 'https://www.wowza.com/downloads/forums/collection'
wowza_modules_streampublisher_enable: no
wowza_modules_streampublisher: 'wse-plugin-streampublisher.zip'
wowza_modules_dvrrecordercontrol_enable: no
wowza_modules_dvrrecordercontrol: wse-plugin-dvrrecordercontrol.zip
wowza_modules_packetizercontrol_enable: no
wowza_modules_packetizercontrol: wse-plugin-packetizercontrol.zip
wowza_modules_transcodercontrol_enable: no
wowza_modules_transcodercontrol: wse-plugin-transcodercontrol.zip
```

## Ansible Galaxy

ansible-galaxy install me_vlad.wowza


## Example Playbook

``` yaml
- hosts: mediaservers
  roles:
    - me_vlad.wowza
```

## License

MIT

## Author Information

Vlad V. Teteria
