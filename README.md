# Ansible Nginx role

This is a very easy role to install Nginx on a Debian based system.

### Why another Nginx role?
Most roles you'll find on GitHub or Ansible Galaxy use a myriad of variables or are really complex because they often support many different operating systems and use-cases. This role is meant to be as simple as possible and only installs Nginx with a basic configuration on Debian systems.

## Configuration
The concept of this rule is to use nginx configs with Jinja2 variables whereever you need them. It's more like a configuration template copier than a "nginx config generator from variables".  

The role can be configured by setting the following variables in your playbook.
If nothing is set, it will install the latest Nginx available without any configuration for your sites.

Example configuration:
```yaml
nginx_packages:
  - nginx-extras       ## required for lua module
```
All available `nginx_*` variables can be found in [`defaults/main.yml`](defaults/main.yml).

### sites-enabled, modules-enabled and conf.d

By default, this role assumes you hve the  following file structure in your `{{ playbook_dir }}/files` directory.

```
nginx
|── sites
│   |── 10-api
│   └── 10-admin
|       
|── snippets
|   └── ssl-example.com.conf
|
|── ssl
|   |── mycert.key
|   └── mycert.pem
|
|── conf.d
|   └── 10-nginx-overrides.conf
|   
└── modules      ## you'll most likely not need this
```

### Further configuration

If you need different sets of configurations for different hosts or environments you could either use Jinja2 variables in those files or adjust your file structure under `{{ playbook_dir }}/files/nginx`; i.e. add a layer for the host or environment.

Using variables is only recommended if differences between hosts or environments are rather small, otherwise your files might get unreadable quickly and this is what this role tries to fix.

In case of larger differences, adjust the `nginx_sites_files`, `nginx_modules_files`, `nginx_ssl_files`, 
`nginx_snippets_files` and `nginx_confd_files` search paths to match your needs for maintaining multiple sets of configurations for different hosts or environments.

For ease of use (so that you don't have to manually adjust 5 paths), you can also set the `nginx_environment` variable that is part of the search paths (`files/nginx/{{ nginx_environment }}/...`).
