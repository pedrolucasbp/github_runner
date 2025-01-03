# GitHub Runner self-hosted Ansible role

An Ansible role that installs and configures GitHub Runner self-hosted

inside one or multiple hosts, you can re-use it for many different URLs
(repositories or organizations) inside the same host in order to re-use it as
much as possible.

Main goals of this role:

- _avoid waste_: re-use the same host to provide a build environment for
  multiple repositories or organizations;
- _idempotence_: executing the role many times won't make anything break, steps
  have checks that validate whether or not they should be executed;

## Variables

For an exhaustive list of variables check the [defaults](defaults/main.yaml)
file. Ideally, all values will have commentaries describing what are their
purposes and by the default value you can tell the type.

### Required variables

Following values are required since there is no way to register the self-hosted
Runner without them


| Name                       | Description                                        |
| ---------------------------- | ---------------------------------------------------- |
| github_runner_config_url   | GitHub Repository or Organization URL              |
| github_runner_config_token | GitHub Registration token to authenticate the host |

## Example Playbook

Simplest use case: Single repository configuration on one host.

```yaml
- hosts: 'github_runners'
  roles:
    - role: macunha1.github_runner
      vars:
        gh_runner_config_labels:
          - linux
          - self-hosted

        github_runner_config_url: https://github.com/macunha1/ansible-github-actions-runner
        github_runner_config_token: {{ github_runner_config_token_vault }}
```

Complex use case to which this role was created for

```yaml
- hosts: 'github_runners'
  roles:
    - role: macunha1.github_runner
      vars:
        gh_runner_config_labels:
          - linux
          - self-hosted

        github_runner_config_url: https://github.com/macunha1/ansible-github-actions-runner
        github_runner_config_token: {{ github_runner_config_token_vault }}

    - role: macunha1.github_runner
      vars:
        github_runner_config_url: https://github.com/macunha1/another-repository
        github_runner_config_token: {{ github_runner_config_token_vault }}

    - role: macunha1.github_runner
      vars:
        github_runner_config_url: https://github.com/macunha-acme-corp
        github_runner_config_token: {{ github_runner_config_token_vault }}
```

Note that despite using the same host, each one of these GitHub Actions Runner
configuration will have its own path and credentials. Therefore, they can live
well in harmony without killing each other.

## Contribute

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

Feel free to fill [an issue](https://github.com/macunha1/ansible-github-actions-runner/issues)
containing feature request(s), or (even better) to send me a Pull request, I
would be happy to collaborate with you.

If this role didn't work for you, or if you found some bug during the execution,
let me know.
