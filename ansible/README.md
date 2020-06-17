# Ansible

> **NOTE:** Inventory `inventory.txt` refers only to this local node (*n0*) via
  `local` connection driver.

The `deploy-app.yml` playbook performs the following actions in order by applying
respective roles:

* Deploys HAProxy with configuration
* Deploys PHP-FPM
* Deploys Apache httpd with configuration
* Deploys NGINX with configuration
* Deploys PHP application (rsyncs contents of [../app/](../app) to
  `/var/www/html/` on a node)

Roles have simplified structure; non-needed parts have been excluded for
simplicity (e.g. `ansible-galaxy` meta information). Each role has corresponding
tag assigned (see `ansible-playbook --list-tags ...`). It is possible to run
specific role by specifying corresponding tag
(e.g. `ansible-playbook --tags apache-httpd ...`).

```code
ansible/
|-- roles/
|   |-- apache-httpd/
|   |   |-- defaults/
|   |   |   `-- main.yml
|   |   |-- handlers/
|   |   |   `-- main.yml
|   |   `-- tasks/
|   |       `-- main.yml
|   |-- app/
|   |   `-- tasks/
|   |       `-- main.yml
|   |-- haproxy/
|   |   |-- defaults/
|   |   |   `-- main.yml
|   |   |-- handlers/
|   |   |   `-- main.yml
|   |   |-- tasks/
|   |   |   `-- main.yml
|   |   `-- templates/
|   |       `-- haproxy.cfg.j2
|   |-- nginx/
|   |   |-- defaults/
|   |   |   `-- main.yml
|   |   |-- handlers/
|   |   |   `-- main.yml
|   |   `-- tasks/
|   |       `-- main.yml
|   `-- php-fpm/
|       `-- tasks/
|           `-- main.yml
|-- deploy-app.yml
`-- inventory.txt
```

## Customization

The `apache-httpd`, `nginx` and `haproxy` roles accept variables for port numbers. See
[deploy-app.yml](deploy-app.yml) and respective `{role}/defaults/main.yml` files.

## Conventions

Roles and playbooks should be linted by `ansible-lint` and/or `ansible-review`.
