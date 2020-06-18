# Boozt lab

![CI-linting](https://github.com/gorshunovr/boozt-lab/workflows/CI-linting/badge.svg?branch=master)

## Environment

Environment is expected to be launched via `vagrant up` command. Vagrant would
bring up one CentOS 8 virtual machine:

* *n0* - this is the main VM:

  * Ansible (VM manages itself trough Ansible)
  * HAProxy load balancer
  * PHP-FPM PHP backend (for both Nginx and Apache httpd)
  * NGINX web server
  * Apache httpd web server
  * App – simple PHP app, prints phpinfo()
  * VM has an additional private network interface with static IP address
    192.168.100.101

```code
.
|-- ansible/
|-- app/
|-- bootstrap.sh*
|-- LICENSE
|-- README.md
`-- Vagrantfile
```

## Exercise

Ansible is installed on *n0* virtual machine via `bootstrap.sh` shell script,
which is called by Vagrant during provisioning process. After Ansible is
installed, Vagrant runs Ansible playbook `deploy-app.yml` automatically:

```bash
cd /vagrant/ansible/ && ansible-playbook -i inventory.txt deploy-app.yml
```

The [deploy-app.yml](ansible/deploy-app.yml) Ansible playbook deploys the
following [roles](ansible/roles/) onto a VM:

* HAProxy load balancer
* PHP-FPM PHP backend (for both Nginx and Apache httpd)
* NGINX web server
* Apache httpd web server
* App – simple PHP app, prints phpinfo()

1. Installable souce code in this repository allows to bootstrap environment by
   running simple `vagrant up` command.

1. Source code of the application `app` is in [app/](app/) subdirectory, and is
   being synced by Ansible to `/var/www/html/` on the `n0` VM. Application simply
   prints phpinfo().

1. HAProxy load balancer listens on `*:80` and passes traffic to two backends:
   `127.0.0.1:81` and `127.0.0.1:82`.

1. Nginx listens on `127.0.0.1:81` and serves `/var/www/html/index.php` PHP
   application.

1. Apache httpd listens on `127.0.0.1:82` and serves same
   `/var/www/html/index.php` PHP application.

1. The URL [http://192.168.100.101](http://192.168.100.101) being opened from the
   host machine would show balanced PHP application.

1. Operating system on the VM is CentOS 8.

1. Exercise uses Vagrant to bring up whole environment by running `vagrant up`
   command.

   Although VirtualBox has been recommended to be used as a virtualization
   software, another virtualization software has been used for tests, and
   reasonable efforts have been made to ensure that environment would also work
   on VirtualBox.

1. SELinux remains enabled.

## Conventions

* Ansible roles and playbooks should be linted by `ansible-lint` and/or
  `ansible-review` tools
* Documentation should be in Markdown format and linted by `markdownlint`
