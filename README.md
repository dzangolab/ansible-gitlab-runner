GitLab Runner Ansible role
==========================

This role will install a [GitLab Runner](https://gitlab.com/gitlab-org/gitlab-runner) and register it with a Gitlab server.

Requirements
------------

This role requires Ansible 2.0 or higher.

This role requires a **Gitlab secret registration token** issued byt the Gitlab server that will register the runner. The role expects to find this token from an AWS secret.

Prerequisites
-------------

* Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html).
* Obtain your AWS credentials from United Asian Engineering Limited and configure AWS CLI for a profile named `dzangolab`. If you want to ues a different profile name, make a note of it for later use.
* Add the role to your `requirements.yml` file
```
- src: git@gitlab.united-asian.com:devops/ansible-gitlab-runner.git
  name: dzangolab.gitlab-runner
  scm: git
  version: "0.2"
```
* Install the role
``` bash
ansible-galaxy install -r requirements.yml
```

Usage
-----

Add the role to your ansible playbook:

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
gitlab_runner_tags:
  - node
  - ruby
  - mysql
gitlab_runner_docker_volumes:
  - "/var/run/docker.sock:/var/run/docker.sock"
  - "/cache"
```

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

`gitlab_runner_description`
The description of the runner.
Defaults to the hostname.

`gitlab_runner_executor`
The executor used by the runner.
Defaults to `shell`.

`gitlab_runner_concurrent_specific`
The maximum number of jobs to run concurrently on this specific runner.
Defaults to 0, simply means don't limit.

`gitlab_runner_docker_image`
The default Docker image to use. Required when executor is `docker`.

`gitlab_runner_tags`
The tags assigned to the runner,
Defaults to an empty list.

`gitlab_runner_cache_type`
Variables to set s3 as a shared cache server. If set it requires variables listed below:
`gitlab_runner_cache_s3_server_address`
`gitlab_runner_cache_s3_access_key`
`gitlab_runner_cache_s3_access_key`
`gitlab_runner_cache_s3_bucket_name`
`gitlab_runner_cache_s3_insecure`
`gitlab_runner_cache_cache_shared`

See the [config for more options](https://github.com/riemers/ansible-gitlab-runner/blob/master/tasks/register-runner.yml)
