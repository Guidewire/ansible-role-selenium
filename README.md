# Selenium ansible role

## Description

Installs selenium on ubuntu 16.04 or centos 7. Creates `systemd` services.

`standalone` and `node` roles:
- run in `xvfb`
- come with `google-crhome` and `chromedriver`

## Requirements

- java, can be installed with [this role](/ansiblebit/oracle-java)

## Variables

|name | default | values | comment|
|-----|---------|--------|--------|
|selenium_role| standalone | standalone / hub / node | |
|selenium_service_name | `{{ selenium_role }}` | |can be used start multiple instances if the same service |
|selenium_version|3.3.1||Check out [selenium download page](http://www.seleniumhq.org/download/), can be changed at any time|
|selenium_chrome_driver_version|2.28||Check out [chrome driver download page](https://sites.google.com/a/chromium.org/chromedriver/downloads), can be changed at any time|

## Playbook

```yml
- hosts: grid-hosts
  become: yes
  roles:
    - role: oracle-java
      tags: java

    - role: selenium
      selenium_role: hub
      selenium_extra_options: "-maxSession 50 -timeout 90 -browserTimeout 90"
      selenium_version: 3.3.1
      tags: selenium

    - role: selenium
      selenium_role: node
      selenium_version: 3.3.1
      selenium_extra_options: "-browser browserName=chrome,maxInstances=50"
      tags: selenium

    - role: selenium/test
      tags:
        - selenium
        - test    
```

## Test your installaton with selenium/test

Currently only `x86_64`systems can run tests, you can update `ansible_go_arch_mappings` with more options.

```yml
    - role: selenium/test
      selenium_hub: "localhost:4444"
      tags:
        - selenium
        - test    
```


## Test

Run tests with [molecule](/metacloud/molecule)

```
molecule test
```
