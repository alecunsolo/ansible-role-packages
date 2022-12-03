Ansible Role: packages
=========

This role manages the packages installed in the target.

Requirements
------------

None.

Role Variables
--------------

```yaml
packages_install_epel: true
```
Whether to install EPEL repository. Only used for EL distros.
```yaml
packages_installed: []
packages_removed: []
```
The packages that should be installed and uninstalled on the target.
```yaml
packages_autoupdate: true
```
Whether to enable automatic updates
```yaml
packages_dnf_automatic_type: security
packages_dnf_automatic_timeout: 60
packages_dnf_automatic_download: yes
packages_dnf_automatic_apply: yes
```
`dnf-automatic` settings. See [templates/dnf-automatic.conf.j2](templates/dnf-automatic.conf.j2)
```yaml
# Ubuntu/Debian Automatic updates
packages_unattended_upgrades_reboot: "false"
packages_unattended_upgrades_reboot_time: 04:30
packages_unattended_upgrades_mail_to: root
packages_unattended_upgrades_mail_on_error: "true"

packages_unattended_upgrades_repos:
  # - origin=${distro_id},codename=${distro_codename}
  # - origin=${distro_id},codename=${distro_codename}-updates
  # - origin=${distro_id},codename=${distro_codename}-proposed-updates
  - origin=${distro_id},codename=${distro_codename}-security
packages_unattended_upgrades_blacklist: []
packages_unattended_upgrades_updateonly: true
```
`unattented-upgrades` settings. See the template [templates/dnf-automatic.conf.j2](templates/52unattended-upgrades-local.j2) for more informations.
NB the template uses `Unattended-Upgrade::Origins-Patter` config key. The syntax is different from `Unattended-Upgrade::Allowed-Origins`.

Dependencies
------------

None.

Example Playbook
----------------
```yaml
- name: Example
  hosts: all
  vars:
    packages_autoupdate: false
    packages_installed:
      - zsh
    packages_removed:
      - nano
  tasks:
    - name: Include packages.
      ansible.builtin.include_role:
        name: alecunsolo.packages
```

License
-------

MIT

Notes
-----

Testing with molecule (including the docker images used) is ~~stolen from~~ heavily inspired by [Jeff Geerling](https://www.jeffgeerling.com/). Watch [his video](https://youtu.be/FaXVZ60o8L8) (and the other ones as well).
