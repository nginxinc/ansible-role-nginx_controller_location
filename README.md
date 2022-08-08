NGINX Controller Location
==========================

#  This repository has been archived. There will likely be no further development on the project and security vulnerabilities may be unaddressed.

Define an environment with NGINX Controller to hold applications (apps), locations, certificates, etc.

Requirements
------------

[NGINX Controller](https://www.nginx.com/products/nginx-controller/)

Role Variables
--------------

### Required Variables

`controller.fqdn` - FQDN of the NGINX Controller instance

`controller.auth_token` - Authentication token for NGINX Controller

`location.metadata.name` -  Name of the environment

### Template Variables

This role has multiple template related variables. The descriptions and defaults for all these variables can be found in **[vars/main.yml](./vars/main.yml)**

Dependencies
------------

Example Playbook
----------------

To use this role you can create a playbook such as the following (let's name it `nginx_controller_location.yaml` for the purposes of this example).

```yaml
- hosts: localhost
  gather_facts: no

  vars:
    nginx_controller_user_email: "user@example.com"
    nginx_controller_user_password: "mySecurePassword"
    nginx_controller_fqdn: "controller.mydomain.com"
    nginx_controller_validate_certs: false

  tasks:
    - name: Retrieve the NGINX Controller auth token
      include_role:
        name: nginxinc.nginx_controller_generate_token

    - name: Configure the location
      include_role:
        name: nginxinc.nginx_controller_location
      vars:
        nginx_controller_location:
          metadata:
            name:  test-location
            displayName:  "Test Location"
            description:  "the location for testing"
            tags:
              - tag1
              - tag2
          desiredState:
            type: OTHER_LOCATION
```

You can then run `ansible-playbook nginx_controller_location.yaml` to execute the playbook.

Alternatively, you can also pass/override any variables at run time using the `--extra-vars` or `-e` flag like so `ansible-playbook nginx_controller_location.yaml -e "nginx_controller_user_email=user@company.com nginx_controller_user_password=notsecure nginx_controller_fqdn=controller.example.local nginx_controller_validate_certs=false"`

You can also pass/override any variables by passing a `yaml` file containing any number of variables like so `ansible-playbook nginx_controller_location.yaml -e "@nginx_controller_location_vars.yaml"`

License
-------

[Apache License, Version 2.0](./LICENSE)

Author Information
------------------

[Brian Ehlert](https://github.com/brianehlert)

&copy; [NGINX, Inc.](https://www.nginx.com/) 2020
