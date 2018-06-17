Wordpress LEMP Ansible Playbook
===============================

This playbook will install Nginx, PHP 7 and MariaDB as well as install a free [SSL certificate](https://letsencrypt.org/).

It will also import an existing database (created with mysqldump) and a wp-config/uploads directory.

> **WARNING:** this this playbook is currently only designed to be run once. It will override any existing data and images on the server.

## Server setup

For an Ubuntu 16.04 Server:

SSH in as the Ubuntu user.

Add user and give them `sudo` access:

```
$ sudo adduser USERNAME
$ sudo usermod -aG sudo USERNAME
```

Install [Ansible](https://www.ansible.com/):

```
sudo apt-get update && \
sudo apt-get install software-properties-common && \
sudo apt-add-repository ppa:ansible/ansible && \
sudo apt-get update && \
sudo apt-get install ansible -y
```

Exit the server

> **Note:** you'll probably want to set up firewalls etc as well but I'll leave that up to you.


## Local setup

Clone repository and move into directory:

```
$ git clone wordpress-example && cd wordpress-example
```

Install Ansible locally. I've included a Pipfile. If you have [pipenv](https://docs.pipenv.org/) installed you can simply:

```
$ pipenv install
```

Then run virtual environment shell:

```
$ pipenv shell
```

Create vars file:

```
$ cp group_vars/website/vars.default group_vars/website/vars
```

Then edit group_vars/website/vars to add your own variables.

> **NOTE:** You should consider using [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) for passwords etc

Create an inventory file called `hosts` which should look like this:

```
[website]
111.222.333.444 ansible_user=USERNAME

```

Put your uploads folder in `roles/wordpress/files/uploads`

Put your database dump file in `roles/mariadb/files/database.sql`

Run playbook:

```
$ ansible-playbook -i hosts playbook.yml --ask-become-pass
```

And enter your SSH/sudo password

Site back and let the playbook take care of the rest.

Once you're done, don't forget to go to the admin and run any database upgrade that might be required.
