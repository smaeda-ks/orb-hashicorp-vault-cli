# orb-hashicorp-vault-cli

[![CircleCI Build Status](https://circleci.com/gh/smaeda-ks/orb-hashicorp-vault-cli.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/smaeda-ks/orb-hashicorp-vault-cli) [![CircleCI Orb Version](https://badges.circleci.com/orbs/smaeda-ks/orb-hashicorp-vault-cli.svg)](https://circleci.com/orbs/registry/orb/smaeda-ks/orb-hashicorp-vault-cli) [![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://raw.githubusercontent.com/smaeda-ks/orb-hashicorp-vault-cli/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)


A custom CircleCI Orb for HashiCorp Vault CLI.

This Orb provides two [commands](https://circleci.com/docs/2.0/orb-concepts/#commands) as below:

| Command | Description | Documentation |
| ---| --- | --- |
| `install`   | Install Vault binary to the current executor environment. | [link](https://circleci.com/developer/orbs/orb/smaeda-ks/orb-hashicorp-vault-cli#commands-install) |
| `auth-oidc` | Authenticate with Vault using OIDC and obtain a token. Upon successful authentication, the obtained token will be set to the `VAULT_TOKEN` environment variable using `$BASH_ENV`. | [link](https://circleci.com/developer/orbs/orb/smaeda-ks/orb-hashicorp-vault-cli#commands-auth-oidc) |

## Example Orb usage

```yaml
description: |
  Install Vault binary, authenticate using OIDC, and get secrets.
usage:
  version: 2.1
  orbs:
    orb-hashicorp-vault-cli: smaeda-ks/orb-hashicorp-vault-cli@0.1.2
  jobs:
    my-job:
      machine: true
      steps:
        - checkout
        # Install Vault
        - orb-hashicorp-vault-cli/install
        # Authenticate using OIDC and obtain token
        # This will automatically set VAULT_TOKEN env variable
        - orb-hashicorp-vault-cli/auth-oidc:
            vault-address: "http://localhost:8200"
            vault-role: "circleci-dev"
        - run:
            name: Get secret
            command: |
              # export secret using $BASH_ENV
              # so it can be referenced by subsequent steps within the job
              FOO=$(vault kv get -field=password secret/circleci/dev)
              echo "export SECRET_FOO=${FOO}" >> $BASH_ENV
  workflows:
    use-my-orb:
      jobs:
        - my-job
```

## Example Vault configuration

An example Vault configuration can be found in this repository's `.circleci` folder:

https://github.com/smaeda-ks/orb-hashicorp-vault-cli/blob/main/.circleci

## Resources

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/smaeda-ks/orb-hashicorp-vault-cli) - The official registry page of this orb for all versions, executors, commands, and jobs described.
[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using and creating CircleCI Orbs.

### How to Contribute

We welcome [issues](https://github.com/smaeda-ks/orb-hashicorp-vault-cli/issues) to and [pull requests](https://github.com/smaeda-ks/orb-hashicorp-vault-cli/pulls) against this repository!
