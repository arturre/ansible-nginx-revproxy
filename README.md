ansible-nginx-revproxy
=========

Install and configures Nginx as reverse proxy for multiple website.
Artur: added support for Mutual Auth TLS

Requirements
------------

This role requires Ansible 2.4 or higher.

Role Variables
--------------

Default values:

```yaml
nginx_revproxy_sites:                                         # List of sites to reverse proxy
  default:                                                    # Set defualt site to return 444 (Connection Closed Without Response)
    ssl: false                                                # Set to True if you want to redirect http to https
    letsencrypt: false

  example.com:                                                # Domain name
    domains:                                                  # List of server_name aliases
      - example.com
      - www.example.com
    upstreams:                                                # List of Upstreams
      - { backend_address: 192.168.0.100, backend_port: 80 }
      - { backend_address: 192.168.0.101, backend_port: 8080 }
    auth:                                                     # Define this block for a single HTTP user/password, or leave undefined for unauthenticated vhosts
      login: myusername
      password: mysecretpassword
    listen: 9000                                              # Specify which port you want to listen to with clear HTTP, or leave undefined for 80
    ssl: false                                                # Set to True if you want to redirect http to https
    letsencrypt: false                                        # Set to True if you are using hispanico.letsencrypt-nginx-revproxy role

  example.org:                                                # Domain name
    domains:                                                  # List of server_name aliases
      - example.org
      - www.example.org
    upstreams:                                                # List of Upstreams
      - { backend_address: 192.168.0.200, backend_port: 80 }
      - { backend_address: 192.168.0.201, backend_port: 8080 }
    listen: 9000                                              # Specify which port you want to listen to with clear HTTP, or leave undefined for 80
    listen_ssl: 9001                                          # Specify which port you want to listen to with HTTPS, or leave undefined for 443
    check_client_ssl: on                                      # Set to value  on|off|optional|optional_no_ca if you want to use mutual auth for TLS, more details http://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_verify_client
    ssl: true                                                 # Set to True if you want to redirect http to https
    letsencrypt: false                                        # Set to True if you want use letsencrypt
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
  - hosts: all
    roles:
      - ansible-nginx-revproxy
    vars:
      nginx_revproxy_sites:
        default:
          ssl: false
          letsencrypt: false

        example.com:
          domains:
            - example.com
            - www.example.com
          upstreams:
            - { backend_address: 192.168.0.100, backend_port: 80 }
            - { backend_address: 192.168.0.101, backend_port: 80 }
          ssl: true
          check_client_ssl: true
          letsencrypt: false
```

License
-------

Licensed under the GPLv3 License. See the LICENSE file for details.

Author Information
------------------

Hispanico
