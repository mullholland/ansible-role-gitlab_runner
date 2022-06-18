# [gitlab_runner](#gitlab_runner)

|GitHub|GitLab|
|------|------|
|[![github](https://github.com/mullholland/ansible-role-gitlab_runner/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-gitlab_runner/actions)|[![gitlab](https://gitlab.com/mullholland/ansible-role-gitlab_runner/badges/master/pipeline.svg)](https://gitlab.com/mullholland/ansible-role-gitlab_runner)|[![quality](https://img.shields.io/ansible/quality/unset)](https://galaxy.ansible.com/mullholland/gitlab_runner)|

description

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
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
gitlab_runner_session_advertise_address: "{{ inventory_hostname}}:8093"

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


## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
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

The machine needs to be prepared in CI this is done using `molecule/default/prepare.yml`:
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





## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

-   [debian9](https://hub.docker.com/r/mullholland/docker-molecule-debian9)
-   [debian10](https://hub.docker.com/r/mullholland/docker-molecule-debian10)
-   [debian11](https://hub.docker.com/r/mullholland/docker-molecule-debian11)
-   [ubuntu1804](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu1804)
-   [ubuntu2004](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu2004)
-   [ubuntu2204](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu2204)
-   [centos7](https://hub.docker.com/r/mullholland/docker-molecule-centos7)
-   [centos-stream8](https://hub.docker.com/r/mullholland/docker-molecule-centos-stream8)
-   [ubi8](https://hub.docker.com/r/mullholland/docker-molecule-ubi8)
-   [rockylinux8](https://hub.docker.com/r/mullholland/docker-molecule-rockylinux8)
-   [almalinux8](https://hub.docker.com/r/mullholland/docker-molecule-almalinux8)

The minimum version of Ansible required is 2.10, tests have been done to:

-   The previous versions.
-   The current version.
-   The [devel](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-devel-from-github-with-pip) version.



## [Exceptions](#exceptions)

Some variations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| Fedora | No packages available |
| centos-stream9 | No packages available |
| amazonlinux | No packages available |


If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-gitlab_runner/issues)

## [License](#license)

MIT


## [Author Information](#author-information)

[Mullholland](https://github.com/mullholland)

## [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
