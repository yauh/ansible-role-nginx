# yauh.nginx
Ansible role for setting up Nginx web server

## Requirements
SSH access to the remote machine

## Role Variables
This role uses mandatory and optional variables. Mandatory variables include:

```
create_self_signed_cert: false
use_ssl: false
setup_default_site: false
default_http_port: 80
web_root: /var/www/default
```

Setting up SSL is done using these variables:

```
use_ssl: true                   # whether to use HTTPS
create_self_signed_cert: true   # if true, will create a certificate
ssl_cert_file: /etc/nginx/ssl/server.crt # where to store the certificate
ssl_key_file: /etc/nginx/ssl/server.key  # where to store the certificate key
cert_subj: /C=US/ST=VIRTUAL/L=Ansible Machines/O=DevOps  # certificate information
cert_common_name: ansible.local # certificate information
cert_validity_duration: 3650 # certificate validity in days
certificate_files:           # set if you want to copy an existing certificate
  - src_file: files/testfile # local certificate file
    dest_name: testfile      # name for cert file on server
```

When `web_users` is defined, a password file is generated with access credentials that will also be used for the default site.

```
web_users:
  - name: ansible            # login name
    password: elbisna        # login password
    passwdfile: /etc/nginx/passwdfile # passwd file on server
```

If defined, Ansible may set up a default site that mentions the FQDN in a simple HTML skeleton only.

```
setup_default_site: true
default_http_port: 80         # port to listen on
default_ssl_port: 443         # port to listen on if SSL is used
default_htpasswd_file: /etc/nginx/passwdfile  # if the default site should be password protected (requires web_users to be defined)
web_root: /var/www/default    # where to copy the default HTML file
site_configs:                 # additional site configs to enable
  - src_template: templates/testsite.j2 # local template file name
    dest_name: testsite.conf  # name to be used on server
```

## Dependencies
None

## Example Playbook
This role can be used to set up nginx, configure a default site, create a self-signed SSL certificate, secure the default site and add users to an `htpasswd` file.

```
- hosts: all
  roles:
     - { role: yauh.nginx }
```

## License
BSD

## Author Information
Stephan Hochhaus stephan@yauh.de

[https://github.com/yauh](https://github.com/yauh)
