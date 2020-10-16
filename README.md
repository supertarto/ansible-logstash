# Ansible Logstash
[![Build Status](https://travis-ci.com/supertarto/ansible-logstash.svg?branch=master)](https://travis-ci.com/supertarto/ansible-logstash)

Install and configure Logstash with Ansible. It's meant to be used with elasticsearch (supertarto.elasticsearch role on Galaxy). For Debian only, the installation use the elasticsearch repository.

## Requirements
None, but can be used with Elasticsearch. (supertarto.elasticsearch)

## Tested plateform
* Debian 10 (Buster)

## Role variables
Logstash version. Tested with 7.0.1 and 7.5.1
```yml
logstash_version: "7.5"
```
Variables used during the installation. Repository, package, GPG key...
```yml
logstash_repo_base: "https://artifacts.elastic.co"
logstash_apt_key: "{{ logstash_repo_base }}/GPG-KEY-elasticsearch"
logstash_apt_key_id: "46095ACC8548582C1A2699A9D27D666CD88E42B4"
logstash_apt_url: "deb {{ logstash_repo_base }}/packages/{{ logstash_major_version }}/apt stable main"
logstash_package_name: "logstash"
```
Set to True if you want to set the package on hold, to prevent unwanted upgrade.
```yml
logstash_version_lock: false
```
Information abour patterns and specific configuration files. An exemple is provided in this role templates folder, but you should write and use yours in your project template directory, and NOT in this role templates folder.
```yml
logstash_conf_path: /etc/logstash/conf.d
logstash_pattern_path: /opt/logstash/patterns
logstash_pattern_list: []
logstash_conf_list: []
```
Information about SSL certs, if used for filebeat.
```yml
logstash_beats_use_ssl: false
logstash_beats_ssl_certificate_authorities: []
logstash_beats_ssl_certificate: []
logstash_beats_ssl_key: []
```
Do we need to convert our certs in pkcs8 format? If true, set the path of the certificate you want to convert and where to copy it.
```yml
logstash_key_src_path: []
logstash_key_dest_path: []

logstash_use_pkcs8_key: false
```
## Examples
```yml
---
- hosts: somehost
  roles:
    - supertarto.logstash
  vars:
    logstash_version: "7.0.1"
    logstash_version_lock: false
    logstash_beats_use_ssl: true
    logstash_use_pkcs8_key: false

    logstash_conf_list:
        - beats.conf
    logstash_beats_ssl_key: /etc/ssl/private/elk.key
    logstash_beats_ssl_certificate_authorities: /etc/ssl/certs/elk_ca.crt
    logstash_beats_ssl_certificate: /etc/ssl/certs/elk.crt

```
## Installation
```
ansible-galaxy install supertarto.logstash
```
## License
GPL V3.0
