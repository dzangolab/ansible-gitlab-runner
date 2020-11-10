GitLab Runner
=============

This role will install the [official GitLab Runner](https://gitlab.com/gitlab-org/gitlab-runner) with updates. Forked and upgraded from 
https://galaxy.ansible.com/dzangolab/gitlab-runner/

Requirements
------------

This role requires Ansible 2.0 or higher.

Role Variables
--------------

`gitlab_runner_package_name`
**Since Gitlab 10.x** The package name of `gitlab-ci-multi-runner` has been renamed to `gitlab-runner`. In order to install a version >= 10.x you will need to define this variable `gitlab_runner_package_name: gitlab-runner`.

`gitlab_runner_concurrent`
The maximum number of global jobs to run concurrently.
Defaults to the number of processor cores.

`gitlab_runner_registration_token`
The GitLab registration token. If this is specified, a runner will be registered to a GitLab server.

`gitlab_runner_coordinator_url`
The GitLab coordinator URL.
Defaults to `https://gitlab.com/ci`.

`gitlab_runner_sentry_dsn`
Enable tracking of all system level errors to Sentry

`gitlab_runner_runners`
A list of gitlab runners to register & configure. Defaults to a single shell executor. See the [`defaults/main.yml`](https://github.com/riemers/ansible-gitlab-runner/blob/master/defaults/main.yml) file listing all possible options which you can be passed to a runner registration command.

Example Playbook
----------------
```yaml
- hosts: all
  remote_user: root
  vars_files:
    - vars/main.yml
  roles:
    - { role: dzangolab.gitlab-runner }
```

Inside `vars/main.yml`
```yaml
gitlab_runner_registration_token: 'GITLAB_RUNNER_REGISTRATION_TOKEN'
gitlab_runner_runners:
  - name: 'Example Docker GitLab Runner'
    executor: docker
    docker_image: 'alpine'
    tags:
      - node
      - ruby
      - mysql
    docker_volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
      - "/cache"
    extra_configs:
      runners.docker:
        memory: 512m
        allowed_images: ["ruby:*", "python:*", "php:*"]
      runners.docker.sysctls:
        net.ipv4.ip_forward: "1"
```
