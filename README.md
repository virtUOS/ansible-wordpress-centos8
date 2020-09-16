<h1 align="center">Welcome to ansible-wordpress-centos8</h1>
<p>
  <img alt="Version" src="https://img.shields.io/badge/version-1.0-blue.svg?cacheSeconds=2592000" />
  <a href="https://github.com/ffeyen/wordpress-ansible" target="_blank">
    <img alt="Documentation" src="https://img.shields.io/badge/documentation-yes-brightgreen.svg" />
  </a>
  <a href="#" target="_blank">
    <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-yellow.svg" />
  </a>
</p>

> Ansible script to install and configure wordpress in a simple way.
> Stack: CentOS8, nginx including lets-ecnrypt certs and mariadb.
> Define a plugin list and a theme to install them automatically. Including SMTP support.

## Install

1. Clone this repository
```sh
git clone URL
```

2. Edit hosts.yml (domain, lets-encrypt mail, theme and plugins)
Template (group_vars/all/vars.yml)
```yml
production:
  hosts:
    HOST1:
      site_domain: "{{ inventory_hostname }}"
      letsencrypt_email: mail@example.com
      wordpress_theme:
        - name: THEMENAME # e.g. 'oceanwp'
          version: THEMEVERSION # e.g. '1.8.9'
          child_theme_file: CHILD_THEME_FILE # if provided in ./roles/wordpress/files
      wordpress_plugins:
        # use plugin name from wordpress route
        # e.g. https://wordpress.org/plugins/LOOK-HERE-FOR-PLUGIN-NAME/
        - name: WP_PLUGIN_NAME_A
        - name: WP_PLUGIN_NAME_B
        - name: WP_PLUGIN_NAME_C
```

3. Generate secrets (db_password_user, db_password_db and if necessary smtp config) and save them to vault.yml
```sh
pwgen -n1 64
```
Template (group_vars/all/vault.yml)
```yml
hosts_vault:
  HOST1:
    db_password: DB_PASSWORD # use strong generated password
    db_password_root: DB_PASSWORD_ROOT # use strong generated password
    wordpress_mail: # if defined, the mail config will be set in wp-config.php
       smtp_user: SMTP_USER # smtp login
       smtp_pass: SMTP_PASS
       smtp_host: SMTP_HOST # smtp server/host (e.g. mail.example.com)
       smtp_from: SMTP_FROMT # email is send from MAIL (e.g. contact@example.com)
       smtp_name: SMTP_NAME # email is send from NAME (e.g. 'My example contact')
       smtp_port: SMTP_PORT
       smtp_secure: tls
       smtp_auth: true
       smtp_debug: 0 # choos 1 for debug mode
```

4. Generate vault password for vault.yml and encrypt the vault.yml file
```
ansible-vault encrypt FILE
```

5. Deploy via ansible
```sh
ansible-playbook wordpress.yml
```

## Author

:computer: **virtUOS (University Osnabrück)**

* www: [virtuos.uni-osnabrueck.de](https://virtuos.uni-osnabrueck.de/)
* Github: [@virtUOS](https://github.com/virtUOS)

👤 **Florian Feyen**

* Github: [@ffeyen](https://github.com/ffeyen)

## 🤝 Contributing

Contributions, issues and feature requests are welcome!<br />Feel free to check [issues page](https://github.com/ffeyen/wordpress-ansible/issues).

## Show your support

Give a ⭐️ if this project helped you!

***
_This README was generated with ❤️ by [readme-md-generator](https://github.com/kefranabg/readme-md-generator)_
