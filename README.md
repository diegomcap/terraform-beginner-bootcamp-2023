# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This Project is going utilize semantic versioning for its tagging.
[semver.org](https://semver.org)

The general format:

**MAJOR.MINOR.Patch**, eg. `1.0.1`

- MAJOR
- MINOR
- PATCH

## Install the Terraform CLI

### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions have change due to gpg keyring changes. So we needed refer to the latest install CLI instruction via terraform Documentation and change the scripting for install.

[Install terraform CLI](https://developer.hashicorp.com/well-architected-framework/operational-excellence/verify-hashicorp-binary)

### Consideration for Linux Distribution

This project is built against Ubuntu.
Please consider checking your Linux Distribution and change accordingly to distribution needs. 

[How to check Os Version in Linux OS](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)


Example of checking OS Version:

```sh
$ cat /etc/os-release 
PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Script

While fixing the Terraform CLI gpg depreciatiuon issues we notice that the bash scripts steps were a considerable amount more code. So we decided to create a bash script to install Terraform CLI. 

This bash script is locate here: [./bin/install_terraform_cli](./bin/install_terraform_cli.sh)

- This wil keep the Gitpod File ([.gitpod.yml](.gitpod.yml)) tidy. 
- This allow us an easier to debug and execute manually Terraform CLI install.
- This will allow better portability for others projects that need to install Terraform CLI.

### Shebang Considerations

A shebang (pronunced Sha-bang) tells the bash script what program that will interpet the script. eg. `#!/bin/bash` 

ChatGPT recommended this format for bash: `#!/ur/bin/env bash`

- For portability for different OS distributions
- will search the user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_%28Unix%29

#### Execution Considerations

When executing the bash Script we can use the `./` shorthand notation to execute the bash script.

eg `./bin/install_terraform_cli`

If wwe are using a script in .gitpod.yml we need to point the script to a program to interpert it.

eg `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations

Linux Permissions works as follows:

In order to make our bash scripts executable we need to change linux permission for the fix to be executable at the user mode.

```sh 
chmod u+x ./bin/install_terraform_cli
``` 
Alternatively:

```sh
chmod 744 ./bin/install_terraform_cli

```

https://en.wikipedia.org/wiki/Chmod

### Github Lifecycle (Before, Init, Command)

We need to be careful when using the Init becaue it will  not rerun if restart an existig workspace.

https://www.gitpod.io/docs/configure/worspaces/tasks


### Working Env Vars

#### env command
We can list out all Enviroment variables (Env Vars) using the `env` command

We can Filter specific env vars using GREP eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars 

In the terminal we can set using `export HELLO=´world`

In the terminal we can unset using `unset HELLO`

We can sewt an env var temporarily when just running a command

```sh
HELLO='world' ./bin/print_message
```
Within a bash script we can set env var without writing export eg.
```sh
#!/usr/bin/env bash

HELLO='world'

echo $HELLO
```

## Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open up new bash terminals in VSCode it will not be aware of env vars that you hace set in another window.

If you want to Env Vars to persist acrss all future bash terminals are open you need to set env vars in your bash profile. eg.`.bash_profile`

#### Persisting Env VArs in Gitpod 

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash teriminals opened in thoes workspaces.

Ypu can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation

 AWS CLI is installed for the project via bash script [`.;bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

[](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctl by running the following AWS CLI command:
```sh
aws sts get-caller-identity
```

if it is succesful you should see a json payload return that looks like this:

```json
{
UserId:"ASAD123232EXAMPLE"
Account: "123456789012'
Arn: "arn:aws:iam:233451234567890126789:user/project
}
```

We'll need to generate AWS CLI credentials from AIM User in orde to the user AWS CLI.

## Terraform Basics

### Terraform Registry

Terraform sources their providers and modules from the Terraform 
[registry which located at the registry.terraform.io] (https://registry.terraform.io/)
[Randon Terraform Provider](https://registry.terraform.io/providers/hashicorp/random)

- **Providers** is an interface to APIs that will allow you to create resources in terraform.
- **Modules** are a way tomake large amount of terrafom code modular, portable and sharable. 

### Terraform Console

We can see a list of all the Terraform commands by simply typing `teraform`

#### Terraform Init

At the start of a new terraform project or when we will run `terraform init`
to download the binaries for the terraform providers that we'll use in this project.

#### Terraform Plan

`terraform plan `
This will generate out a chageset, about the state of our infrastructure and what will be changed. 
We can output this changeset ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terraform Apply

`terraform apply`

This will run a plan and pass the changeset to  be execute by terrafrom. Apply Should prompt yes or no.

If we want to automatically pprove an apply we can provide the auto approve flag eg. `terraform apply --auto-aprove` 

#### Terraform Destroy

`terraform destoy`
This will destroy resources.

you can also use the auto approve flag to skip the approve prompt eg. `terraform apply --auto-aprove`



#### Terraform Lock Files
`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used with this project.

The Terraform Lock File **should be committed to your Version Control System (VSC) eg. Github**

#### Terraform State Files

`terraform.tfstate` contain information about the current state of your infrstructure.

This file **should not be commited** to your VCS.

This file can contain sensetive data.

If you lose this file, you lose knowing the state of your infrastructure.

`.terraform.tfstate.backup` is the previous state file state.

#### Terraform Directory's

`.terraform` directory contains binaries of terraform providers.


## Issues with Terraform Cloud Login and Gitpod Workspace

When try to run `terraform login` launch in bash a wiswig view to generate a token. However it does not work as expected in Gitpod Vsode in the Browser.

the work around in manually gererate a token in Terraform Cloud

```
https://aap.terraform.io/app/settings/token?source=terraform-login
```

Then creates the file manually here: 

```sh
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json
```
Provide de following code (replace your token in the file):

```json
{
  "credentials" : {
    "app.terraform.io": {
      "token": "YOUR_TOKEN_TERRAFORM_CLOUD"
    }
  }
}
```

We automated this workaroud with the following bash script [bin/generate_tfrc_credentials/] (bin/generate_tfrc_credentials)