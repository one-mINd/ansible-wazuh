# [ansible-wazuh](https://github.com/one-mINd/ansible-wazuh)

Ansible role for deploying and configuring Wazuh using Docker Compose.

This role installs a dockerized single-node Wazuh stack based on the official Wazuh deployment guide:

* [https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html#single-node-stack-deployment](https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html#single-node-stack-deployment)

## Features

* Automatically deploys Wazuh single-node stack
* Automatically changes the default `admin` password
* Enables Wazuh archive logs
* Allows extending and modifying `ossec.conf`
* Mounts all persistent Wazuh volumes to the host filesystem
* Supports mounting additional custom volumes into Wazuh containers

## Example Playbook

```yaml
- hosts: wazuh
  become: true

  roles:
    - role: ansible-wazuh
```

## Enabling archives

If both variables are enabled:

```yaml
wazuh_docker_bind_volumes_on_host: true
wazuh_enable_archives: true
```

the role automatically modifies the `filebeat.yml` configuration to enable Wazuh archive log collection.

After modification, the role sets the immutable (`+i`) attribute on the file to prevent Wazuh containers from overwriting the configuration during restarts or upgrades.

Because of this, manual modification or deletion of the file requires removing the immutable attribute first:

```bash
chattr -i filebeat.yml
```

You can verify the attribute using:

```bash
lsattr filebeat.yml
```

After deployment, you need to perform some manual actions in the UI - https://documentation.wazuh.com/current/user-manual/manager/event-logging.html#configuring-the-wazuh-dashboard

## References

* [https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html#single-node-stack-deployment](https://documentation.wazuh.com/current/deployment-options/docker/wazuh-container.html#single-node-stack-deployment)
* [https://documentation.wazuh.com/current/user-manual/capabilities/log-data-collection/monitoring-log-files.html](https://documentation.wazuh.com/current/user-manual/capabilities/log-data-collection/monitoring-log-files.html)
* [https://documentation.wazuh.com/current/user-manual/manager/event-logging.html#configuring-event-archiving](https://documentation.wazuh.com/current/user-manual/manager/event-logging.html#configuring-event-archiving)

