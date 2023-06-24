# [gitlab_runner](#gitlab_runner)

Installs and configures a GitLab Runner.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-gitlab_runner/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-gitlab_runner/actions)|[![gitlab](https://gitlab.com/opensourceunicorn/ansible-role-gitlab_runner/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-gitlab_runner)|[![quality](https://img.shields.io/ansible/quality/58825)](https://galaxy.ansible.com/mullholland/gitlab_runner)|[![downloads](https://img.shields.io/ansible/role/d/58825)](https://galaxy.ansible.com/mullholland/gitlab_runner)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-gitlab_runner.svg)](https://github.com/mullholland/ansible-role-gitlab_runner/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-gitlab_runner/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  vars:
    # Only neede in molecule testing with no existing gitlab instance
    gitlab_runner_skip_registration: true
  roles:
    - role: "mullholland.gitlab_runner"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-gitlab_runner/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true

  tasks:
    - name: check if connection still works
      ansible.builtin.ping:
```


## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-gitlab_runner/blob/master/defaults/main.yml):

```yaml
---
# Repository
gitlab_runner_repository_key: 'https://packages.gitlab.com/runner/gitlab-runner/gpgkey'
gitlab_runner_repository_key_pub: 'https://packages.gitlab.com/runner/gitlab-runner/gpgkey/runner-gitlab-runner-4C80FB51394521E9.pub.gpg'  # noqa 204

# Runtime user and group for the gitlab_runner service
# Enable if you want an other user, as provided by the package
gitlab_runner_user_enable: false
gitlab_runner_user: "gitlab-runner"
gitlab_runner_user_id: "999"
gitlab_runner_group: "gitlab-runner"
gitlab_runner_group_id: "999"

# Package name which to install
gitlab_runner_package: "gitlab-runner"

# Only neede in molecule testing with no existing gitlab instance
gitlab_runner_skip_registration: false

# Gitlab Config GLOBAL options
# https://docs.gitlab.com/runner/configuration/advanced-configuration.html#the-global-section
# Limits how many jobs can run concurrently.
# The maximum number is all defined runners. 0 does not mean unlimited.
gitlab_runner_concurrent: '{{ ansible_processor_vcpus }}'
# Defines the interval length, in seconds, between new jobs check.
# The default value is 3. If set to 0 or lower, the default value is used.
gitlab_runner_check_interval: '5'
# Defines the log level.
# Options are debug, info, warn, error, fatal, and panic.
# This setting has lower priority than the level set by the
# command-line arguments --debug, -l, or --log-level.
gitlab_runner_log_level: 'warning'
# Specifies the log format.
# Options are runner, text, and json.
# This setting has lower priority than the format set by
# command-line argument --log-format. The default value is runner.
gitlab_runner_log_format: 'json'
# Enables tracking of all system level errors to Sentry.
gitlab_runner_sentry_dsn: ''
# Enables tracking of all system level errors to Sentry.
gitlab_runner_listen_address: '[::]:9299'

# Gitlab Config GLOBAL session_server
# https://docs.gitlab.com/runner/configuration/advanced-configuration.html#the-session_server-section
# Number of seconds the session can stay active after the job completes.
# The timeout blocks the job from finishing. Default is 1800 (30 minutes).
gitlab_runner_session_timeout: "1800"
# An internal URL for the session server.
gitlab_runner_session_listen_address: "[::]:8093"
# The URL to access the session server.
# GitLab Runner exposes it to GitLab
gitlab_runner_session_advertise_address: "{{ inventory_hostname }}:8093"

# Register options
gitlab_runner_configs:
  - name: "{{ inventory_hostname }}/{{ ansible_distribution }}/{{ ansible_distribution_major_version }}"
    url: "https://gitlab.example.com"
    registration_token: "lawfrhitea905"
    tags:
      - "tag1"
      - "tag2"
    executor: "docker"
    extra_options: []
  - name: "{{ inventory_hostname }}/{{ ansible_distribution }}/{{ ansible_distribution_major_version }}"
    url: "https://gitlab.example.com"
    registration_token: "lawfrhitea905"
    tags: []
    #  - "tag1"
    #  - "tag2"
    executor: "docker"
    extra_options:
      - "--access-level not_protected"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-gitlab_runner/blob/master/requirements.txt).


## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-gitlab_runner/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/mullholland/docker-centos-systemd/general)|all|
|[Amazon](https://hub.docker.com/repository/docker/mullholland/docker-amazonlinux-systemd/general)|Candidate|
|[Ubuntu](https://hub.docker.com/repository/docker/mullholland/docker-ubuntu-systemd/general)|all|
|[Debian](https://hub.docker.com/repository/docker/mullholland/docker-debian-systemd/general)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-gitlab_runner/issues)

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-gitlab_runner/blob/master/LICENSE).

## [Author Information](#author-information)

[Mullholland](https://mullholland.net)

Please consider [sponsoring me](https://github.com/sponsors/mullholland).
